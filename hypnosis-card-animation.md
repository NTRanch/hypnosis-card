# ANIMATION STUDY NOTE — Hypnosis Card v1.02 ↔ Hypnosis Card 2 Happy Life (S2b)

Per-title animation note for the Lane D programme (PIPELINE.md
stage **S2b**, D-1094 research-study framing). One note per
title, filed beside its dynamics spec as
`games/<title>-animation.md`. Template sections retained in
full; a section that cannot be filled is closed with
**UNKNOWN — <what was tried>** (the D-713 rule).

**Clean-room law, restated for animation:** this note records
*techniques and measurements* — rig shapes, state graphs,
timing numbers, easing shapes described as parameters — never
copied assets, curve files, keyframe dumps shipped verbatim,
or code. What crosses into the remake is a principle Torogao
re-implements in its own terms (§8/§9 of the template's
numbering, kept here). Verbatim material stays RE-side,
cited by evidence pointer only.

**Grade vocabulary:** MEASURED (read from source/data — IL,
file listings) > DERIVED (computed from measured facts,
derivation shown) > REVIEW (observed or inferred only;
never presented as measured).

---

## 0. Header

| Field | Value |
|---|---|
| Title / version(s) studied | Hypnosis Card v1.02 ↔ Hypnosis Card 2 Happy Life v1.0 — version pair (identity proof in the dynamics spec's pair verdict) |
| Engine + version | Unity Mono both: v1 = 2019.4.40f1 (built-in RP), v2 = 2021.3.13f1 (URP) — MEASURED from `globalgamemanagers` strings |
| Original language | UNKNOWN — I2.Loc ships in both builds; no language asset was read this pass (route in §10) |
| Study date / student | 2026-11-08 / Lane D QUEUE #5 night lane (Pax subagent) |
| Dynamics spec (sibling) | `games/hypnosis-card-dynamics.md` |
| Census | `CENSUS.md` §1 row 37 (v2, as `Hypnosis_Card_2`) + §2 row 105 (v1.02); fathom MonoBehaviour census row for v2: 11,517 MonoBehaviours, 306 distinct script classes (`summary-17.json`, primo) |
| Evidence home | primo `~/.local/share/pax/hypnosis-card-20261108/` (`il/`, `inv/`, `chunks/`, `bundles/`) |
| Note grade overall | REVIEW — the architecture and inventories are MEASURED, but no clip, rig, or controller *contents* were parsed this pass (typetree absent; AssetRipper-class rung uncommissioned), so every motion-quality claim routes to §10 |

## 1. Animation architecture at a glance

The pair is a before/after of a whole presentation philosophy.
**v1 animates in 2D:** battle girls are Spine skeletal puppets
(`spine-unity.dll` in Managed; `SpineEventAudioHandler` glue),
action scenes are composed from a kit of PNG+prefab pairs
naming actor-part × target-part, dialogue runs on Fungus, and
UI motion is DOTween. **v2 animates in 3D:** girls are FBX
models driven by Unity Animator with per-character/per-costume
OverrideControllers, scripted scenes are Unity Timeline
playables (103 timeline bundles) fired through a TimelineManager,
expressions are named face clips + blend-shape changes, hair/
cloth-class secondary motion is DynamicBone, cameras are
Cinemachine, and voice gets a lip-sync assembly plus a 10-second
idle wait-talk/breathing clock in battle. Both versions evaluate
animation per-frame through Unity's standard player loop; the
game-specific driver classes are named below.

- Summary: 2D Spine + prefab-kit composition (v1) → 3D Animator +
  Timeline scene system (v2); the *game logic* driving both is
  the shared card-battle core (dynamics spec §4).
- Evaluation rate + source: Unity per-frame (Update/Animator/
  Timeline defaults); game-side pacing hooks MEASURED in IL —
  v2 `BattleManager.GetTimelineDelayTime`, `StartDetectWaitTime`,
  `Const.BATTLE_WAIT_VOICE_TIME = 10.0` (`chunks/v2-Const.txt`) —
  **Evidence:** `il/v2-main-a.il`, `inv/v2-main-methods.tsv`
- Animation data location: v1 — Spine assets inside
  `battlegirl_0N_new_prefab` bundles + scene/sharedassets;
  v2 — FBX + controller bundles in StreamingAssets
  (`ac_*`, `*_fbx`, `timeline_*_playable`) + scene/sharedassets
  — **Evidence:** `bundles/v1-bundles.txt`, `bundles/v2-bundles.txt`

## 2. Rig / puppet structure

### 2a. Skeletal / puppet rigs (Live2D-class, Spine, …)

**v1 applies — Spine.** `spine-unity.dll` (281,088 B) in
Managed, Spine runtime + example classes in
Assembly-CSharp-firstpass (v1 only), game glue class
`FishChain.Common.SpineEventAudioHandler` (v1 only — audio
keyed to Spine animation events), five battle-girl prefabs
`battlegirl_01..05_new_prefab` (+ `extend_` copies).

| Field | Entry |
|---|---|
| Rig kind + part count | Spine (spine-unity); part/bone count UNKNOWN — rig contents not parsed (bundle parse not run) |
| Hierarchy shape | UNKNOWN — same route |
| Parameter set | Spine animation/state names UNKNOWN; the game's own mapping table is `FishChain.HStone.CardAnimTable` + `CardAnimInfo` + `BattleAnimData` (v1 classes, deleted in v2) — card → animation is a *data table*, that shape is MEASURED from class inventory |
| Mesh/deformer notes | UNKNOWN |
| Physics on the rig | None found in v1's stack (no DynamicBone/Obi classes) — honest negative, MEASURED at assembly level |

- **Evidence:** `inv/v1-main-classes.txt`, `inv/v1-firstpass-classes.txt`,
  Managed listings in the pair probe (`probe-pair.sh` output, evidence home)

**v2: N/A for Spine** — the runtime, its examples, and the
glue class are all deleted (MEASURED absence across both
assemblies' inventories).

### 2b. Sprite / frame rigs (cel, sprite-sheet, …)

**v1 partially applies — a prefab composition kit instead of
sheets.** Action imagery ships as paired bundles
`<actor>_<part>_<target>_png` + `_prefab` over parts
(hand/mouth/dick/feather/tongue/…) × targets
(breast/mouth/pussy/…), for both `boy_*` and `girl_*`
prefixes, plus `special_*` compositions and
`extend_hcg_01..05` scene prefabs. The swap model is
whole-prefab composition selected by (actor part, target
part) — a combinatorial scene kit, MEASURED at bundle-name
level; per-prefab internals UNKNOWN.

- **Evidence:** `bundles/v1-bundles.txt` (271 v1-only names)

**v2: N/A** — the kit is deleted with the 2D presentation.

### 2c. 3D rigs (where a title is 3D)

**v2 applies.** One base girl model (`girlmodel_fbx`,
`girlanimator_controller`) re-skinned per character-costume:
mother / sister / beach / bunny / doctor sets, each with
`<name>_hair_fbx`, `<name>_clothes_fbx`, `<name>_idle_fbx`,
`<name>_myroom_fbx`, PBR texture sets (basecolor / normal /
metal-smooth / AO / opacity), and an Animator
**OverrideController** bundle (`ac_mother_overridecontroller`,
`ac_sister_…`, `ac_beach_…`, `ac_bunny_…`, `ac_doctor_…`) —
one shared controller graph, per-character clip overrides.
A male model set mirrors the pattern (`male_fbx`,
`ac_male_controller`, `malehand_controller`, transparent-body
material option in the save).

| Field | Entry |
|---|---|
| Skeleton | UNKNOWN — bone count/names not parsed (FBX contents not read) |
| Skinning / morphs | Blend shapes are a named game system: `BlendShapeChangeInfo`, `ClothesBlendShapeChangeInfo` classes (v2) — morph *targets* driven by game state incl. clothing state; target names/weights UNKNOWN (serialized) |

- **Evidence:** `bundles/v2-bundles.txt`, `inv/v2-main-classes.txt`

## 3. Animation state machine

| State (v2) | Entered from | Trigger / condition | Exits to | Evidence |
|---|---|---|---|---|
| Battle states (Default → Start → … → ReadyToEnd → Ended → Clean) | Map / battle start | `BattleStateMgr` over shared `BattleManager` | Clean → map | `inv/v2-main-consts.tsv` (`BattleStateType`), class inventory |
| Timeline scene playback | Battle & room/anim states | `TimelineManager` + `AnimTimelineTable` rows; battle waits via `isPlayingTimeline` / `OnBattleTimelineEnd` | Return to battle/room | `inv/v2-main-fields.tsv` (BattleManager, MapManager), methods diff |
| Girl expression states | Timeline/anim events | Named face clips (`face_normal/cold/high/love/sex/lick/blowjob/closeeyes_anim`) + Timeline **signals** (`signal_*` bundles: damage, high, lick, sex, end, clearanimatorstate, closeeyes…) | Next signal/clip | `bundles/v2-bundles.txt` |
| Idle / wait | Battle, no input | Wait clock: `BATTLE_WAIT_VOICE_TIME = 10.0` s → `PlayWaitTalk`, `PlayBreatheSound` | Any battle action | `chunks/v2-Const.txt`, `inv/v2-main-methods.tsv` |

- Machine owner: v1 — `CardAnimTable` (card → Spine anim
  mapping) + Fungus flowcharts for talk; v2 — Unity Animator
  (override controllers) for bodies, **Timeline + signals**
  for scenes, `GirlAnimatorEvents` as the code bridge —
  **Evidence:** class inventories both versions
- Transition mechanics: UNKNOWN — crossfade/blend values
  live in the serialized controllers (AssetRipper route,
  §10). What is MEASURED: v1→v2 replaces *table-mapped clip
  swaps* (CardAnimTable) with *timeline-composed scenes*
  (103 playables) — the transition *philosophy* changed even
  where numbers are unread.
- Diagram (text form):
  v1: card played → CardAnimTable lookup → Spine anim +
  event audio → VFX (BattleAnimVfx) → talk (Fungus)
  v2: card played → battle state machine → TimelineManager
  plays row from AnimTimelineTable → signals fire face/
  voice/damage beats → Animator override clips + blend
  shapes carry the body → OnBattleTimelineEnd returns control

## 4. Clip inventory

Bundle-name level (MEASURED names and counts; durations and
sample rates UNKNOWN for every row — no clip contents were
parsed, and none are back-computed):

| Clip family | Used by (state) | Count | Loop? | Evidence |
|---|---|---|---|---|
| v1 `battlegirl_0N_new_prefab` (Spine girls) | Battle | 5 (+5 extend) | rig-dependent, UNKNOWN | `bundles/v1-bundles.txt` |
| v1 `boy_*` / `girl_*` composition prefabs | Action scenes | ~120 pairs | N/A (static composition) | `bundles/v1-bundles.txt` |
| v2 `timeline_*_playable` | Scenes (battle actions, room, love) | 103 | per-playable, UNKNOWN | `bundles/v2-bundles.txt` |
| v2 `face_*_anim` (normal, cold, high, love, sex, lick, blowjob, closeeyes, lick_male) | Expression states | 9 | UNKNOWN | `bundles/v2-bundles.txt` |
| v2 body anims: `angry_anim`, `happy_anim`, `hurt_anim`, `normal_anim`, `normal_cold_anim`, `normal_sexy_anim`, `question_anim`, `closeeyes_anim`, `eyeblink_anim`, `breastrotate_anim`, `breastrotater_anim` | Girl states | 11 | blink/idle presumably loop — REVIEW | `bundles/v2-bundles.txt` |
| v2 per-costume `*_idle_fbx`, `*_myroom_fbx` | Room / idle | 5 costumes × 2 | UNKNOWN | `bundles/v2-bundles.txt` |
| v2 `girlN_step12/step34_*_asset` (action1–6, win1–3, wait1–3, lose, protect, room, roomclick) + voice clips | Battle steps, room | 255 girl bundles total | UNKNOWN | `bundles/v2-bundles.txt` |

- Total clip count: not a single number this pass — the
  countable units are bundles (v1 596, v2 1,165) and the
  named families above; clip-level counts are §10 work.
- Naming pattern: v1 names compositions by
  (actor, part, target); v2 names by (character, context,
  action) for FBX and by content ID for timelines
  (`timeline_<cardid>_<part>_<variant>_playable` — the
  timeline namespace *is* the card namespace, e.g.
  `timeline_10101_touch_upper_playable` beside card 10101):
  animation content is addressed in the game's data IDs,
  not in art-side names. Both conventions are best-practice
  evidence (§8).

## 5. Timing & easing evidence

| Motion | Duration / rate | Easing shape | Parameters | Evidence |
|---|---|---|---|---|
| Battle idle → wait-talk / breathe | 10.0 s wait threshold | n/a (clock, not a curve) | `BATTLE_WAIT_VOICE_TIME = 10.0` | `chunks/v2-Const.txt` (MEASURED) |
| Rhythm judgement windows (dynamics §8b) | perfect 0.1 / good 0.2 / bad 0.3 / ignore 0.5 | step bands | `KEY_*` constants | `chunks/v2-Const.txt` (MEASURED) |
| Timeline pacing in battle | delay read via `GetTimelineDelayTime` | UNKNOWN | serialized in playables | methods inventory (name MEASURED, values REVIEW) |
| UI tweens | DOTween in both versions (same `DOTween.dll` size 174,592 B — consistent with an unchanged DOTween version, DERIVED) | DOTween default + per-call eases, UNKNOWN per call | — | Managed listings |
| Card play → animation | v1: `CardAnimTable` lookup (instant map); v2: timeline row play | — | — | class inventories |

- Frame-level timing patterns: UNKNOWN — anticipation/hold/
  smear counts require clip parsing (§10).
- Gameplay timing cross-reference: dynamics spec §1.5
  (all code-side constants) and §8b (rhythm windows/scores).
- Pointer/input → animation mapping: card targeting is a
  named UI system in both (`Battle.UI.CardLayout`,
  `TargetArea`, BezierArrow shared); v2 adds
  `cardTargetingPos` (Vector2 field on BattleManager) —
  the arrow/target layer survived the 2D→3D change intact
  (MEASURED from shared classes + field diff).

## 6. Secondary motion

| Element | Driven by | Mechanism | Parameters | Evidence |
|---|---|---|---|---|
| Hair / cloth / soft parts (v2) | character motion | **DynamicBone** (classes in v2 firstpass: `DynamicBone`, `DynamicBoneCollider`, …) | stiffness/damping UNKNOWN (serialized) | `inv/v2-firstpass-classes.txt` |
| Fluids / soft-body (v2) | — | **Obi** ships (581,632 B) but its visible classes in the game assembly are vendor *samples* (BucketController, FluidJetController, MazeManager…) — gameplay use UNKNOWN (§10) | — | `inv/v2-main-classes.txt` |
| Breathing (v2) | wait clock | sound + (presumed) idle clip — `PlayBreatheSound` is MEASURED as a verb; the visual half is REVIEW | 10.0 s cycle gate | methods inventory |
| Blink (v2) | idle | dedicated clip `eyeblink_anim` + `closeeyes_anim` bundles (clip-driven, not procedural) | period UNKNOWN | `bundles/v2-bundles.txt` |
| v1 secondary motion | — | **none found**: no physics/secondary-motion library in either v1 assembly; Spine rigs may bake follow-through into clips, which is clip content (§10) — honest negative at the code level | — | `inv/v1-*.txt` |

- Blink / breathing cycles: v2 has named blink clips and a
  breathe *sound* verb on a 10 s wait clock; periods and
  whether blink is randomized are UNKNOWN.
- What has NO secondary motion: v1, entirely, at the code
  level (above); v2's UI layer (DOTween only).

## 7. Engine animation plumbing

| Engine feature used | How this title uses it | Evidence |
|---|---|---|
| Unity Animator + OverrideControllers (v2) | one controller graph (`girlanimator_controller`), per-character clip sets swapped by override bundles (`ac_*`) | bundle inventory |
| Unity Timeline + custom playables (v2) | 103 playable bundles as the scene system; firstpass gains custom Timeline tracks (LightControl, Loop, ScreenFader, TextSwitcher, TimeDilation, NavMeshAgentControl, VideoPlayable) — scenes choreograph light, fades, text, and time itself | `inv/v2-firstpass-classes.txt`, bundles |
| Timeline signals (v2) | `signal_*` + `audio_*_signal` bundles (10 + 14) — events inside timelines drive animator states, damage, and voice | bundles |
| Spine (v1) | skeletal battle girls; animation events keyed to audio (`SpineEventAudioHandler`) | firstpass + Managed |
| Fungus (both) | dialogue/talk flow (`BattleTalk` shared class; Fungus.dll both) | Managed listings |
| DOTween + Pro (both) | UI and object tweens | Managed listings |
| Cinemachine (v2) | `FreeVirtualCamera` game class + CameraMgr; head/lower detail views in battle are camera work, not UI swaps | class/field inventories |
| LipSync (v2) | `LipSync.dll` ships; consumption by girl views is REVIEW (§10) | Managed listing |
| Asset bundles (both) | all animation content streams from bundles; v2 generates per-bundle manifest classes (dynamics §1.2) | bundles + class inventories |

- Update-order facts: battle waits on timeline completion
  (`OnBattleTimelineEnd`) before returning control —
  MEASURED from method names; finer ordering UNKNOWN.
- Determinism-relevant facts: animation is presentation-
  only in both versions; battle outcomes resolve in the
  shared effect layer (dynamics §4), timelines *display*
  them. The wait clock (10 s, Time-based) is the one
  wall-clock dependency found; the remake should drive it
  from the session clock, not wall time (DERIVED guidance).

## 8. Best practices worth adopting

| # | Practice (as this title demonstrates it) | Evidence | Disposition |
|---|---|---|---|
| 1 | Address animation content in the game's own data IDs (`timeline_<cardid>_<part>_playable`), so a card row and its scene can never drift apart | v2 bundle names | ADOPT into Torogao scene addressing |
| 2 | One controller graph, per-character override sets — cast growth costs clips, not graphs | `ac_*` override bundles + single `girlanimator_controller` | ADOPT for lumen/torogao puppet rigs |
| 3 | Name clips by (character, context, action) and expressions as a closed named set (`face_*`) | bundle inventories | ADOPT as naming convention |
| 4 | Give idle a clock and a voice: 10 s wait → breathe + wait-talk | `BATTLE_WAIT_VOICE_TIME`, battle methods | ADOPT (session-clock driven) |
| 5 | Choreograph scenes as data (Timeline rows in a table — `AnimTimelineTable`) rather than code branches | v2 classes | ADAPT — Torogao's scene graph should be rows, as its cards already are |
| 6 | (v1's lesson) A card → animation *mapping table* (`CardAnimTable`) decouples content from code even in the cheap 2D version | v1 classes | NOTE-ONLY — superseded by #5's fuller form |
| 7 | AVOID: absorbing vendor sample scenes into the shipping assembly (Obi samples in v2) — census noise + licensing surface | v2 class inventory | AVOID (also dynamics §9 #9) |

## 9. What Torogao would emulate

| Original technique (§ ref) | Torogao mechanism | Difference, if any, + reason |
|---|---|---|
| Override-controller cast pattern (§2c) | one puppet rig graph per body plan; per-character clip/expression sets as data rows | Torogao rigs are 2D puppets (lumen stack), not FBX — the *pattern* (one graph, many sets) transfers, the medium doesn't |
| Timeline scenes addressed by content ID (§4) | scene rows keyed by card/content ID in the systems RON, played by the session layer | Torogao scenes must stay deterministic — timeline playback becomes a fixed-step beat table, not wall-clock Timeline |
| Signal-driven beats inside scenes (§7) | named beat events in the scene row (face/voice/damage cues as data) | same shape, estate-owned names |
| Expression clip set (§4) | whole-body expression states per estate doctrine (posture + face), not face-only clips | parity-or-better: v2's face clips are face-only; Torogao expressions are whole-body by Owner rule |
| DynamicBone-class secondary motion (§6) | lumen physics groups (spring/pendulum) on hair/cloth/soft parts | parameters re-derived, never copied |
| Wait-clock idle life (§3, §6) | session-clock idle timer firing breathe + talk rows | driven by session time so replays/Android stay in lockstep |
| Card → scene mapping table (§3, v1) | the RON itself: a card row names its scene row | collapse the two tables into one row set |

- Feeds: the cross-medium animation pipeline (grammar of
  scene addressing + idle life) and the lumen puppet stack
  (override-set pattern, physics groups). Landing step:
  this note + the dynamics spec are the S2b/S3 record for
  QUEUE #5; the S4 commission consumes §9 when written.

## 10. Gaps & REVIEW register

| # | Unknown | What was tried | Route that would close it |
|---|---|---|---|
| 1 | Clip durations, sample rates, loop flags (all families) | bundle-name census (names/counts only) | AssetRipper-class parse of `timeline_*`, `*_anim`, `*_fbx` bundles |
| 2 | Spine rig structure (v1) and FBX skeletons (v2) | assembly/class census | same rung over `battlegirl_*` / `*_fbx` bundles |
| 3 | Animator transition/blend values (v2) | class inventory | controller asset parse (same rung) |
| 4 | DynamicBone parameters; whether blink is randomized; breathe cycle shape | class/bundle census | serialized-component read (same rung) |
| 5 | Whether Obi does gameplay work in v2 | class census (samples visible, consumers untraced) | IL call-site read for Obi types outside sample namespaces |
| 6 | LipSync consumption (which views, which phoneme set) | assembly presence | IL call-site read + asset parse |
| 7 | Original language (header §0) | I2.Loc presence only | I2Languages asset read (same rung as 1) |
| 8 | Fungus flowchart contents (talk structure) | class census (`BattleTalk`) | scene/sharedassets parse (same rung) |

## 11. Provenance

- Fathom routes used: S1 census of record (fathom
  MonoBehaviour census, `summary-17.json`); S2/S3 ikdasm IL
  route per PIPELINE §2 (Unity Mono class).
- Extraction commands of record (primo, evidence home
  `~/.local/share/pax/hypnosis-card-20261108/`):
  `probe-hypnocard.sh`, `find-card2.sh`, `probe-pair.sh`,
  `probe-v2.sh` (disk identity); `il-export-hypnocard.sh`
  (ikdasm ×2 per assembly, byte-identical sha256s in the
  dynamics spec header); `ilparse2.py` (inventories —
  parser of record; see the bloch candidate for the
  one-class shift bug its predecessor had and the header
  assertion that caught it); `diff-state.sh`,
  `diff-content.sh`, `diff-bundles.sh`, `diff-final.sh`.
- Bloch candidates filed from this note:
  `../bloch/hypnosis-card-pair-diff-lessons.md`.
- Skill-trust records: local-fathom-game-extract +
  local-game-intake (recorded at lane close).
