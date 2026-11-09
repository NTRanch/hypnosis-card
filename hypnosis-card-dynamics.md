# Hypnosis Card — dynamics spec (S3), from the version-pair diff

Lane: NTRanch / nsfw. Study of record for QUEUE #5.
Sibling note: `hypnosis-card-animation.md` (S2b).

**PAIR VERDICT (read first — the census names mislead).** The
pair QUEUE #5 means is real and both members are on disk, but
neither sits where its census name suggests:

- **v1** = `Hypnosis.Card.v1.02` — at
  `originals/Hypnosis.Card.v1.02/Hypnosis.Card.v1.02/Hypnosis Card/`
  (build nested two wrappers deep, beside its own zip).
  Product string `HyponsisCard` (sic — the shipped typo is in
  the exe and data-dir names too), company **Naku Kinoko**.
  The "1.02" is the distribution's naming (folder + zip); no
  version string was found inside the build itself.
- **v2** = `Hypnosis Card 2 Happy Life` — **not** at the
  top level under the census §1 name `Hypnosis_Card_2` (no such
  directory exists in the current tree). It lives nested at
  `originals/convert/Hypnosis_Card_2/Hypnosis_Card_2_/`.
  Identity proven, not assumed: app.info company **Naku
  Kinoko** (same developer), product `Hypnosis Card 2 Happy
  Life`; wrapper archive named
  `Hypnosis.Card.2.Happy.Life.v1.0.Uncensored.zip` (so v2 is
  itself **v1.0** of the sequel); Unity **2021.3.13f1**,
  matching CENSUS §1 exactly; wrapper 1,993,351,134 B ≈ the
  census's 1.99 GB; and the fathom MonoBehaviour census row
  titled `Hypnosis_Card_2` (11,517 MonoBehaviours, 6,894
  monoscripts, 306 distinct script classes, exhaustive over
  1,180 files — `~/.local/share/pax/fathom-unity-mono-pilot-20261103/out/summary-17.json`)
  belongs to this build.
- **`Hypnosis2` is neither member and not a duplicate.** It is
  an **uncensor patch payload**: 5 files, 51,799,284 B —
  `Hypnosis2_Uncensor_1.017.zip` plus an `app_Data/StreamingAssets/aa/`
  Addressables set (3 bundles). No executable, no `Managed/`,
  no game code at all. It patches some other "Hypnosis 2"
  title the estate does not hold as a build. Recorded here so
  no later lane re-chases it; nothing further is needed to
  "complete" the pair — the pair is v1.02 ↔ Happy Life.

**Evidence home (primo):** `~/.local/share/pax/hypnosis-card-20261108/`
— `il/` (ikdasm exports; every export run twice, byte-identical:
v1-main 108,173 lines sha256 `a7775d6a…`, v2-main 184,635 lines
sha256 `6ca66e8f…`, plus both firstpass exports), `inv/`
(class/field/method/literal inventories + enum extracts,
parser of record `ilparse2.py`), `chunks/` (per-class IL
extracts), `bundles/` (bundle-name inventories both versions),
probe/diff scripts. DLLs of record: v1 `Assembly-CSharp.dll`
437,760 B sha256 `413d74f5…`; v2 952,832 B sha256 `8e2804c6…`.

**Grade summary:** identity, engine versions, all class/field/
enum inventories, the `Const` tuning table, and bundle counts
are **MEASURED** (read from IL or file listings). Card/girl/
enemy *table values* (stats, costs, prices, map layouts) are
serialized into asset bundles and scenes in both builds —
**REVIEW**, registered in §10 with the route that closes them.
Cross-version interpretations are marked DERIVED with the
derivation shown.

---

## 1. The version diff

### 1.1 Build facts (MEASURED)

| Fact | v1.02 | v2 Happy Life |
|---|---|---|
| Unity | 2019.4.40f1 | 2021.3.13f1 |
| Render pipeline | Built-in (no URP assemblies) | URP (Universal RP assemblies; `Volume` + `UniversalAdditionalCameraData` fields in MapManager) |
| Backend | Mono (`Assembly-CSharp.dll` + MonoBleedingEdge) | Mono (same shape) |
| Build root | 747,416,012 B, 1,500 files | 1,155,981,480 B, 2,597 files |
| Scenes | level0–level4 (5) | level0–level4 (5) |
| Game DLL | 437,760 B | 952,832 B (2.18×) |
| IL export | 108,173 lines | 184,635 lines |
| Classes (main asm) | 435 | 1,759 headline — see §1.2 |
| Game classes (`FishChain.HStone.*`) | 200 | 322 (159 shared, 41 removed, 163 added) |
| Fungus (dialogue) | 1,003,008 B | 1,011,200 B |
| DOTween | present (+ Pro) | present (+ Pro), same `DOTween.dll` size 174,592 B |
| Spine | `spine-unity.dll` 281,088 B + Spine example classes in firstpass | **absent entirely** (no dll, no classes) |
| New in v2's stack | — | Cinemachine 288,256 B; Obi 581,632 B; LipSync 76,288 B; CFXR; KinoBloom; LeTai TranslucentImage (URP); SoX/mp3 native libs (`libsox-3`, `libmp3lame`, `libmad`, `libflac` in Plugins) |
| Steam | Steamworks.NET, both | same |

Both builds also carry I2.Loc (localization), QFramework/
ResKit/UIKit, Sirenix serialization, DamageNumbersPro,
AudioKit, TextMeshPro, Unity Timeline — the framework layer
barely changed; the game layer is what moved.

### 1.2 The class-count artefact (DERIVED)

v2's headline 1,759 classes overstates its code growth:
**1,151 of them are `QAssetBundle.*` manifest classes** — one
generated class per asset bundle (v2 holds 1,165 bundles;
v1 generates 0 such classes and loads its 596 bundles without
them). Like-for-like code size is ≈435 → ≈608 classes
(1,759 − 1,151). Any cross-version class-count comparison
that skips this subtraction is measuring the bundler, not
the game. (Derivation: `grep -c '^QAssetBundle'` over
`inv/v1-main-classes.txt` = 0, `inv/v2-main-classes.txt` =
1,151; bundle counts from `bundles/`.)

### 1.3 Loop diff — what the developer changed their mind about

**Kept (the shared skeleton, MEASURED by shared classes):**
the node-map deckbuilder run. Both versions share
`MapManager`, `GameManager`, `SaveData`/`Saver`, the whole
battle core (`Battle.BattleManager`, `Battle.CardPlayer`,
`Battle.Turn`, `Battle.Player`, `Battle.Enemy`, `BuffSystem`),
68 of the effect classes, and the data-table classes
(`NewCardTable`, `GirlTable`, `EnemyTable`, `EventTable`,
`BuffTable`, `StatTable`, `DreamStoneTable`,
`DifficultTierTable`, `GodessTable`). The battle's design
language (§5) is byte-for-byte the same vocabulary.

**Restructured:**

- *Map nodes:* v1 models map nodes as view classes —
  `BasePoint` + `BattlePoint`, `EventPoint`, `RestPoint`,
  `ShopPoint`, `RecallPoint`, `DigPoint`-window — each paired
  with a flat `*Window` class (41 HStone classes deleted in
  v2, incl. `DeckManager`, `CharacterManager`,
  `RecallManager`, `CardAnimTable`, `CgMgr`, `CGWindow`,
  `GalleryWindow` family, `MainManuManager`).
  v2 deletes the whole layer and rebuilds UI as a
  **Panel + PanelData framework** (`UI.*Panel` /
  `*PanelData` pairs: deck, shop, rest, event, dig, gallery,
  rewards, remove/upgrade-card…) — the same screens, re-homed
  behind a data-driven panel system.
- *Top-level flow (FSM, MEASURED from GameManager fields):*
  v1 states = MainMenu / Map / **Character** / **Recall**.
  v2 drops Character and Recall as states and adds **Anim**
  (`state_Anim`, `Anim_2_MainMenu`) — character management
  and recall were folded into panels; scene playback became
  a first-class state. v2 GameManager gains speech/dialog
  helpers (`ShowSpeech`, `ShowDialog`, `ShowFinalLoveAnimation`),
  popups as fields, and a `graphicQuality` setting.
- *Battle:* v1's battle presentation hooks
  (`battleTalk`, `BattleAnimVfx`, `PlayBattleTalk`,
  `StopCardAnim`) are deleted with Spine. v2 wraps the shared
  BattleManager in an explicit state machine —
  `Battle.BattleStateMgr` + `BattleStateBase/Clean/Data` +
  enum `BattleStateType {Default, Start, ReadyToEnd, Ended,
  Clean}` — adds a `BattleData` object, a **`BattelAutoTester`**
  (sic) with `StartTest`/`testMode`, and timeline integration
  (`isPlayingTimeline`, `OnBattleTimelineEnd`,
  `GetTimelineDelayTime`).

**Added in v2 (no v1 counterpart):**

- **Tower mode.** `GameMode {Story, Tower}`; `UITowerPanel`,
  `TowerScoreMgr`, `KeyScoreBase`, `ScoreTable`/`ScoreInfo`,
  `BuildTowerMap`/`ClearTowerLevel` beside the story map;
  save carries `unlock_towerMode`, `towerScoreRecords`,
  `towerBestScore`, `towerGotGold`, `towerBestData`.
- **Rhythm "massage" minigame.** `MusicTable`/`MusicInfo`/
  `MusicResult`, `UIMassagePanel`, `MassageItem`,
  `RhythmVisualizatorPro` (a field on MapManager), four
  `massage_music_N_mp3` + `massage_score_N_txt` bundles, and
  the `KEY_*`/`SCORE_*` constant family (§1.5). Judgement is
  timing-window based (perfect/good/bad/ignore bands).
- **Love / hypnosis steps.** `UIHypnosisStepPanel`,
  `MapManager.curHypnosisProgress`, per-girl `hypnosisSteps`
  dict in the save, `Const.LOVE_STEP1_VALUE 1000`,
  `LOVE_STEP2_VALUE 1500`, `LOVE_ADD_SPEED 50`,
  `AnimType {Girl, Both, Love}`, `GirlInfo.LoveAnimId` /
  `ExtraLoveAnimId`, talk content reorganized from per-action
  string fields into `AllTalkStep12` / `AllTalkStep34` lists
  (steps 1–2 vs 3–4) — the sequel's progression spine is a
  per-girl step ladder the original did not have.
- **Ability XP tracks.** `AbilityTable`/`AbilityInfo`,
  `AbilityType {Hand, Mouth, Dick, Item}`; the save gains
  both current ability values and per-track XP
  (`abilityHand` + `expHand`, … ×4); battles award XP
  (`RecordExpToSave`, BattleManager `exp*` fields).
- **Enemy desire.** `Battle.Enemy` gains `desire` +
  `desiredPart` (a `BodyPart`); trigger `OnHitDesirePart`
  and BattleManager verb `OnEnemeyGotDesire` (sic) +
  `SelectFixedBodyPart` — the enemy telegraphs a wanted
  body part and play can answer it.
- **Dress-up.** `GirlClothesPartTable`,
  `GirlStepClothesTable`, `ClothesPartInfo`, `DressUpItem`,
  `FoldPart {None, Upper, Lower, Foot}`,
  `ClothesBlendShapeChangeInfo` — clothes as data, gated
  by hypnosis step.
- **Gallery growth:** dream-stone gallery panel
  (`UIGalleryDreamstonePanel`), anim gallery panels,
  score panel, room panel + bed-interact panel
  (`UIRoomPanel`, `UIBedInteractPanel`), speech panel,
  first-run language select (`UIFirstSelectLanguagePanel`).

**Removed in v2:** Spine everywhere; the Recall system as a
place (manager + point + window + FSM state all gone);
`CGWindow`/`CgMgr` as a separate CG viewer (gallery panels
absorb it); v1's `playerInitialGold` field on MapManager
(moved into per-mode save data, §7).

### 1.4 State-vocabulary diff (MEASURED, from literal fields)

The core battle vocabulary is **unchanged** — same members,
same values:

| Enum | Members (both versions) |
|---|---|
| `CardType` (3) | Action, Item, Special |
| `TurnType` (2) | Player, Girl |
| `BattleStat` (4) | Numbness, Physical, Sexual, Strength |
| `BodyPart` (11) | None, Hand, Mouth, Dick, Breast, Pussy, Anal, Foot, Item, Shoot, NextAll |
| `EffectTarget` (5) | Boy, Girl, Self, Target, None |
| `GirlState` (4) | Locked, Virgin, Playing, Played |
| `CardTarget` (15), `CardMode` (9), `CardShowMode` (3), `PositiveType` (3) | identical sets |

Growth is additive everywhere except one hazard:

- **`CardEffect`: 89 → 106 names — all 89 v1 names retained,
  17 added, but 83 of the 89 shared names changed numeric
  value.** The effect ID space was renumbered between
  versions (the table was rebuilt, IDs are positional).
  Names are the stable key; IDs are not. New names:
  AddBattleStatSexsualOnGold, AddRandomCard,
  AddStatItemDamageModify, AddStatRemoveCardInShop,
  AddStatSalivaSpread, DamageWithActionCardUsed,
  DamageWithDrawCardsInTurn, DamageWithExtraSelfDamaged,
  DamageWithExtraShield, DeckcardCountDamage,
  DoubleBuffLayer, DrawCardWithBuffCount,
  ItemcardCountDamage, ModifyGirlBodyCastable,
  ModifyGirlBodyTargetable, ModifyPlayerSpStat,
  RetriggerBuffTurnStart — condition-scaling damage (by
  gold, draw count, deck count, item count, shield, buff
  count) is v2's card-design direction. Effect *classes*
  grew 68 → 82 (+14, same names as the new enum members
  where a class was needed).
- `TriggerType`: 29 → 35, pure additions: OnUseActionCard,
  OnNextLayer, OnGotHeal, OnTurnStartWith4Mana,
  OnGirlGotHeart, OnHitDesirePart.
- `CardFeature`: +Action, +SexToy (29 → 31).
- `CostType`: +DecreasedByOpponentPoison (3 → 4) — a poison
  interaction with card cost exists in v2.
- `CastRequirement`: +HandCardEqualOne (17 → 18).
- `ShopItemType`: +DreamStone (3 → 4) — dream stones became
  purchasable.
- New v2 vocabularies: `GameMode {Story, Tower}`,
  `BattleStateType {Default, Start, ReadyToEnd, Ended,
  Clean}`, `SpStat {AddSexualWhenUseHandCard,
  AddShieldWhenUseDickCard, NextActionCardDDoubleDamage}`
  (sic), `AbilityType {Hand, Mouth, Dick, Item}`,
  `AnimType {Girl, Both, Love}`, `FoldPart {None, Upper,
  Lower, Foot}`. Deleted with v1's map layer: `PointType`.

### 1.5 Tuning-constants diff (MEASURED, `FishChain.HStone.Const`)

`Const` is the one place the game keeps code-side tuning.
v1 ships exactly one constant; v2 keeps it and adds the new
systems' constants:

| Constant | v1 | v2 |
|---|---|---|
| PLAYER_MAX_HANDCARDS | 16 | 16 (unchanged) |
| HARMONY_GIRL_CARD | — | 60101 (0xEAC5; an ID, not a magnitude) |
| HARMONY_RANDOM_EVENT | — | 101 (an ID) |
| LOVE_STEP1_VALUE | — | 1000 |
| LOVE_STEP2_VALUE | — | 1500 |
| LOVE_ADD_SPEED | — | 50 |
| BATTLE_WAIT_VOICE_TIME | — | 10.0 s |
| CARD_TAG_MODIFY_RATE | — | 0.2 (literal) |
| KEY_SPAWN_DISTANCE | — | 700.0 |
| KEY_SPAWN_ADVANCE | — | 2.0 |
| KEY_PERFECT | — | 0.1 |
| KEY_GOOD | — | 0.2 |
| KEY_BAD | — | 0.3 |
| KEY_IGNORE | — | 0.5 |
| SCORE_PERFECT | — | 100 |
| SCORE_GOOD | — | 50 |
| SCORE_BASE_VALUE | — | 384 |
| SCORE_VALUE_RATIO | — | 7 |

Everything else a tuner would touch — card costs/damages,
girl stats, enemy stats, node rewards, shop prices, map
shapes — is **serialized data** (asset bundles + scenes) in
both builds and does not appear in IL (§10, R1). The diff
therefore proves *which* tuning exists as code, and that
the sequel's new systems were tuned in code while the
original's single code constant survived untouched.

### 1.6 Content-surface diff (MEASURED, bundle inventories)

Asset bundles (StreamingAssets/AssetBundles/Windows):
**596 → 1,165 bundles**; only 325 names shared; bundle bytes
376,101,099 → 434,022,678.

- v1's surface is 2D: per-card PNG bundles (53 `card_*_png`
  + two `extend_card_*` waves), five `battlegirl_0N_new_prefab`
  Spine girls, and a compositional action-scene kit —
  `boy_<actor-part>_<target>` / `girl_<part>_<target>`
  PNG+prefab pairs (hand/mouth/dick/feather/tongue ×
  breast/mouth/pussy…), `extend_hcg_01..05`, per-girl event
  prefabs (`extend_newgirl0N_ev1..3`), sparse voice
  (hurt/orgasm wavs per girl).
- v2's surface is a 3D production: five costume characters
  (mother / sister / beach / bunny / doctor), each with FBX
  model + hair + clothes + idle/myroom FBX and full PBR sets
  (basecolor/normal/metal-smooth/AO); a male model set;
  **103 `timeline_*_playable` bundles** (the scene system);
  **255 `girlN_*` voice/step asset bundles** — per girl:
  step12 + step34 grids of action1–6 / win1–3 / lose /
  wait1–3 / room / roomclick / protect assets plus moan,
  scream, oral, kiss, orgasm clips; 9 `face_*_anim`
  expression clips; per-character Animator
  OverrideControllers (`ac_mother/sister/beach/bunny/doctor`,
  `ac_male_controller`); `extend_*` waves add 5 more girls'
  event chains (start + event1–4 each) and 8 extend cards.
  Per-card PNG bundles collapse 53 → 4 (card art re-homed —
  atlases/shared assets; the exact route is REVIEW, §10 R4).
- `extend_*` bundles: 225 → 232 — both versions grew by
  post-launch content waves under the same naming scheme;
  the *content* of the waves differs entirely (2D events
  vs 3D girls).

---

## 2. Shape of the game (the shared design)

A single-player **node-map deckbuilder**: a run walks a
branching map (`MapManager`; the map is stored in the save
as `mapRawData` + X/Y per game mode) through battle, event,
shop, rest, and dig nodes; battles are card duels against a
girl (`TurnType {Player, Girl}` — she has a deck and turns
too) where cards target **body parts** (`BodyPart`, 11
values) and push four battle stats (`BattleStat`:
Numbness, Physical, Sexual, Strength). Buffs stack through
a dedicated `BuffSystem`; a card's behaviour is a
composition of `CardEffect` entries (89–106) resolved by an
`EffectFactory`/`EffectMgr` strategy layer of `Effect_*`
classes (68–82) — the design is data-shaped exactly as
QUEUE #5's prior said: cards are rows (`NewCardInfo`,
schema unchanged across versions except +`ExtendOnly`
bool), girls are rows (`GirlInfo` + `GirlTable`), enemies,
events, buffs, stats, dream stones, difficulty tiers, and
"goddess" blessings all have `*Table`/`*Info` classes.
Dialogue and story flow run on Fungus in both versions
(`BattleTalk` shared); localization is I2.Loc in both.
The hypnosis theme is the progression fiction: girls move
`GirlState {Locked, Virgin, Playing, Played}`, and v2 makes
the ladder explicit as per-girl hypnosis steps (§8c).
...[truncated 13571 chars]
## 3. The run loop

**v1 form (MEASURED from class/field inventories).** MainMenu
→ Map, with Character and Recall as sibling FSM states.
On the map, nodes are typed points (battle / event / shop /
rest / recall / dig); each opens its window. Deck changes
happen at shop (buy), card-reward (add), rest (the
`RestWindow`), remove-card and upgrade-card windows, and a
run starts from `InitCardsWindow`. The save remembers where
you are: raw map data + point coordinates + point info
strings, current deck (list of card IDs), gold, health /
max health, "bless" value, cloth-state map value, dream
stones held + their values, an extra-mode unlock flag.

**v2 form.** The same loop, re-factored: the save's flat
fields move into a per-mode `GameModeData` slice (§7) —
{v1 `mapRawData`, `curPointCord_X/Y`, `dreamStoneValues`,
`curGold`} all reappear as GameModeData fields — and the
map gains a second builder: `BuildStoryMap` /
`BuildTowerMap`, `ClearStoryLevel` / `ClearTowerLevel`,
story/tower result handlers. New run verbs on MapManager:
`Surrender` (a run can be conceded — story-mode surrender
has its own end handler), `UnlockCard`, `StartBattleCo`,
`ShowHypnosisStepEnd`. The map screen now also owns the
girl camera, the room, and the rhythm visualizer field —
the map is the game's hub scene, not a menu.

## 4. The battle loop

**Shared core (MEASURED).** `BattleManager` (83 fields in
v1, 98 in v2) runs the duel; `CardPlayer` plays cards;
`Turn` alternates `TurnType {Player, Girl}`. The player
side has life, shield, mana (recover modified by effects),
a hand capped at **16** (`PLAYER_MAX_HANDCARDS`, both
versions), draw/discard/exile piles (effects name them:
`Effect_ReturnExileCards`, `Effect_DiscardRandHandCard`,
`Effect_ModifyDrawCardCount`). The girl side has life,
shield, body parts with per-part buff views
(`BodyPartBuffView`), and her own deck and turns
(`Effect_EnemyCardUsed`, `Effect_AddOpponentDeckCard`).
Damage/heal/shield/stat changes are all `Effect_*`
strategies triggered off `TriggerType` events (29 in v1,
35 in v2) and gated by `CastRequirement` (17 → 18) and
`CostType` (3 → 4). Buffs are data (`BuffInfo`/`BuffTable`)
with durations that effects can retrigger, double, or
convert to damage/shield.

**v2 changes (MEASURED from fields/methods).** Battle
state becomes explicit (`BattleStateType` + state manager,
incl. a Clean state — battles tear down through the same
machine that starts them); battle data moves into a
`BattleData` object (`mPrivateData`, `get_Data`); the
battle tracks `girlId` and **`hypnosisStep`** (battle
outcomes advance the step ladder); ability XP is tallied
in-battle and recorded to the save at end
(`RecordExpToSave`); the enemy's **desire** is rolled and
displayed (`OnEnemeyGotDesire`, `desiredPart`), with
`SelectFixedBodyPart` letting play lock a target part;
presentation waits on timelines and voice: `waitTime`,
`checkWait`, `StartDetectWaitTime`, `PlayWaitTalk`,
`PlayBreatheSound`, `BATTLE_WAIT_VOICE_TIME = 10.0` — an
idle battle breathes and talks on a 10-second clock;
head/lower detail views and HP/shield bars get dedicated
scene objects. The auto-tester (`testMode`, `StartTest`)
means the developer balance-tested battles headlessly —
a practice §9 adopts.

## 5. State vocabulary (consolidated)

Full enum extracts are in evidence (`inv/enum-v1-*.txt`,
`inv/enum-v2-*.txt`, `inv/*-consts.tsv`). The load-bearing
summary is §1.4: the battle's language did not change
between versions; everything new is additive except the
CardEffect ID renumbering. For S4, the vocabulary to carry
into the RON schema is the v2 set: 106 card effects,
35 triggers, 18 cast requirements, 4 cost types,
11 body parts, 4 battle stats, 3 card types, 2 turn types,
plus v2's mode/state/ability vocabularies (§1.4).

## 6. Tuning constants (consolidated)

§1.5 is the complete code-side tuning surface of both
builds — one constant in v1, eighteen in v2, one shared
value unchanged (hand cap 16). The design intent this
reveals (DERIVED): the developer tunes *systems* in code
and *content* in data. A remake inverts nothing here: the
RON should hold the content tables (once the DLL-metadata
rung reads them) and the systems crate should hold exactly
the §1.5-class constants as named values, so a future diff
of the remake against this spec is as mechanical as this
one was.

## 7. Meta-progression and save shape (MEASURED, field diff)

`SaveData` grows 29 → 43 fields, and its centre of gravity
moves:

- **Per-mode slices (v2):** `gamemodeData` — a
  `Dictionary<GameMode, GameModeData>`; each slice holds
  isPlaying, gold, mapRawData, X, Y, dreamStoneValues,
  totalTurn, girlEvents / playedGirlEvents, **hypnosisSteps**
  (per-girl step progress), **abilityExps** (per AbilityType).
- **Ability progression (v2):** four tracks (Hand, Mouth,
  Dick, Item), each with a level value and an XP value in
  the save (`ability*` + `exp*` ×4) — playing cards of a
  family grows that family.
- **Tower records (v2):** best score/data per the tower
  fields of §1.3, plus `towerScoreRecords` and a
  `MusicResults` dictionary (rhythm results persist too).
- **Lifetime stats (v2):** totalDamage, totalGold,
  onceMaxDamage, totalElitePlayed, totalEventPlayed,
  totalRestPlayed — the sequel tracks a career, the
  original tracked a run.
- **Scene/room state (v2):** sceneId, sceneLightId,
  animGirl; settings flags subtitleOn, TransparentMaleBody,
  isDemo; unlockedExtraCards.
- **Dropped as top-level fields (v2):** curDeck, curHealth/
  curHealthMax, curBless, mapClothStat, curPointInfo,
  notPlayingGirl, unlock_extraMode_1 — most reappear inside
  GameModeData; `curBless` and `mapClothStat` have no
  same-named v2 home (REVIEW whether renamed or removed,
  §10 R5).

Saver itself is a one-field class in both — serialization
is Sirenix-driven in both versions.

## 8. v2's new systems (detail)

**(a) Tower mode.** A second, score-attack run on its own
map (`towermap_prefab` bundle), with elite nodes
(`MapManager.isElite`, elite background gradient vs normal),
score records, and gold banking (`towerGotGold`). Story
mode remains the campaign; tower is the repeatable
endgame the original lacked.

**(b) Rhythm massage.** A timing minigame skinned as
massage: notes spawn at distance 700, advance 2.0 (units
REVIEW), and are judged in windows 0.1 / 0.2 / 0.3 / 0.5
(perfect/good/bad/ignored); scoring 100 / 50 per hit over
a base value 384 with ratio 7 (the exact score formula is
REVIEW — constants measured, composition not). Four music
tracks ship as bundles with score text files. `SpStat`'s
three modifiers and the `CARD_TAG_MODIFY_RATE 0.2` belong
to this layer's reward plumbing (DERIVED from co-location
in Const and the massage panels; the consuming code paths
are REVIEW).

**(c) Love / hypnosis steps.** Each girl has a step ladder;
`LOVE_STEP1_VALUE 1000` and `LOVE_STEP2_VALUE 1500` are
threshold-class values with `LOVE_ADD_SPEED 50` as the
gain rate (roles DERIVED from names + the step panel +
`hypnosisSteps` save dict; exact accumulation is REVIEW).
Talk and animation content is indexed by step bands
(12 vs 34), battles read the current step, and a
`ShowFinalLoveAnimation` caps the ladder. This is the
sequel's answer to v1's flat `GirlState` — progression
became a meter, not a flag.

**(d) Desire.** The enemy wants a body part; hitting the
desired part is a trigger (`OnHitDesirePart`) and its own
battle event flow. A telegraphed-target system inside a
targeting game — small, legible, and it changes card
valuation turn by turn.

## 9. Best practices — adopt / avoid

**ADOPT**

1. **Diff the pair before studying either.** This study's
   highest-density facts (the renumbering hazard, the
   deleted Recall system, the step ladder replacing a
   state flag) are visible *only* in the diff. Filed as a
   bloch candidate beside the intake-era
   `version-pair-diffing.md`.
2. **Effects as named strategies.** 68 → 82 one-class-per-
   effect strategies behind a factory, with card rows as
   pure data: new content = new rows + occasionally one
   new class. The remake's systems crate should keep this
   shape exactly.
3. **Key data by name, never by positional ID** — proven
   the hard way by the developer themselves: 83 of 89
   effect IDs changed value between versions while every
   name survived. Any estate table format (RON included)
   gets name keys and a renumbering test.
4. **Ship an auto-tester for the battle.** v2's
   `BattelAutoTester` + `testMode` is the developer
   balance-testing headlessly; Torogao's S7 determinism
   gates want the same harness from day one.
5. **Per-mode save slices.** v2's refactor of a flat save
   into `Dictionary<Mode, ModeData>` is the correct shape
   the moment a second mode exists; adopt for any
   multi-mode remake.
6. **Progression as a meter with thresholds in code**
   (love steps: thresholds + gain rate as named
   constants), content indexed by meter bands (talk
   step12/step34) — the banding keeps content authoring
   tractable as the ladder grows.
7. **Idle life in battle:** a wait clock (10 s) that fires
   breathing and wait-talk makes a static duel scene feel
   inhabited for near-zero system cost.

**AVOID**

8. **Positional enum IDs as data references.** The
   renumbering (§1.4) forced a full data remap between
   versions; in an estate pipeline it would silently
   corrupt saves. (The flip side of #3, recorded as the
   anti-pattern.)
9. **Absorbing vendor sample code into the game
   assembly.** v2's Assembly-CSharp carries Obi/CFXR
   sample classes (buckets, fluid jets, mazes) — build
   hygiene debt that inflates every later census and
   would be a licensing hazard in a shipped remake.
10. **Letting the bundler generate the class list.**
    1,151 generated manifest classes made the sequel's
    code growth unreadable at headline level (§1.2);
    generated artifacts belong in generated assemblies,
    visibly separate.
11. **Two progression vocabularies for one ladder.**
    v2 keeps `GirlState` *and* hypnosis steps *and*
    `AnimType.Love`; the overlap is design debt a remake
    should collapse into one meter + band table.

## 10. REVIEW / UNKNOWN register

| # | Unknown | What was tried | Route that closes it |
|---|---|---|---|
| R1 | All serialized tuning: card costs/damages, girl/enemy stats, event outcomes, shop prices, difficulty tiers, goddess blessings, map layouts | IL inventories + Const extraction (proves the values are not in code) | DLL-metadata (AssetRipper-class) rung over sharedassets + bundles — the queue's standing gap |
| R2 | Original (source) language of either build | I2.Loc presence in both; no language asset read | I2Languages asset read (same rung as R1); remake ships estate-original English regardless (Lane D rule) |
| R3 | Battle scene composition per girl in v1 (Spine rigs) and controller graphs in v2 | Bundle-name census (names measured; contents not parsed) | AssetRipper-class parse of `battlegirl_*` bundles (v1) and `ac_*` bundles (v2) |
| R4 | Where v2's card art lives after the per-card PNG bundles collapse (53 → 4) | Bundle inventory diff | Bundle/sharedassets parse (same rung) |
| R5 | Fate of v1's `curBless` / `mapClothStat` in v2 (renamed vs removed) | Field diff of SaveData + GameModeData | Targeted IL read of v2 save/load call sites |
| R6 | Rhythm score formula composition; KEY_SPAWN_ADVANCE units; LOVE_* accumulation rule | Const extraction (values measured) | Targeted IL read of the massage/step classes (`MusicResult`, step panel data classes) |
| R7 | Whether v2 gameplay uses Obi (vs sample code only) | Assembly/class census (Obi present; consumers not traced) | IL call-site read for Obi types outside sample namespaces |
| R8 | v1's in-build version (the "1.02" is distribution naming) | app.info + wrapper read | None available in-build; recorded as distribution-grade |

## 11. Candidacy read (S4/S5)

**The remake target is v2's design** — it is the later,
fuller game, and its systems are a superset of v1's except
for the deleted Recall/CG layer (which a remake may
re-add deliberately, not by accident). S4 remains
**coin-sized on the battle core** (one RON: card rows,
effect vocabulary by *name*, girl/enemy/event/buff tables
— values pending R1) but the full v2 surface is **not**
small: five 3D costume characters, 103 timeline scenes,
a 255-bundle voice grid, dress-up tables, a rhythm mode,
and a tower mode. Staged honestly: S4a = battle + map +
deck meta (the shared skeleton, v1-scale content); S4b =
steps + abilities + tower; the rhythm mode is a separate
spike (its clock is a judgement window table, closer to
Hypno_Rhythm's problem than to this crate's); S5 content
is the largest in the queue's front half after Milk
Factory's roster — five girls × step-banded talk/animation
is a production programme, not a pass. Dual-target parity
is a shell question only: nothing in either build's
dynamics is platform-bound.
