# Project Plan

## 1. Problem - What problem are we solving?

This is a **personal fork of `llm-wiki-compiler`** used to build long-term, source-backed personal knowledge bases while staying easy to update from the upstream repository.

The first target is a **Social Interaction Knowledge Graph MVP**. The system should ingest social-interaction books, chapters, articles, notes, short posts, and videos and compile them into structured knowledge that preserves:

- what the original author actually said;
- the author's own framework/stage model;
- the context in which a recommendation applies;
- exact or suggested wording;
- body-language and voice-delivery guidance;
- observable cues;
- the author's interpretation of those cues;
- conditional branches;
- examples;
- source provenance;
- disagreements or overlap between different authors.

This must be more structured than a generic wiki or summarizer.

A social-interaction source may contain a complete flowchart-like system such as:

- approach/open;
- start a conversation;
- move through stages;
- respond differently depending on the other person's behavior;
- use a specific line;
- use a particular tone or cadence;
- change body language;
- fall back to another action if the response is poor;
- move to a deeper subflow;
- apply a later rule only if something happened earlier in the interaction.

The knowledge system must support this without flattening everything into disconnected summaries.

### 1.1 The system must preserve source-specific theories

Different authors may:

- use different stage models;
- use different terminology;
- interpret the same cue differently;
- recommend different actions for the same situation;
- agree on the same lower-level action even when their frameworks differ.

The system must **not** create a fake universal theory by silently merging them.

Bad representation:

`CrossedArms -> MEANS -> Defensive`

Preferred representation:

`InterpretationClaim_A`
- `ABOUT_CUE -> CrossedArms`
- `INTERPRETS_AS -> Defensive`
- `ASSERTED_BY -> AuthorA`
- `FROM_SOURCE -> BookA`

Another author may produce a different interpretation of `CrossedArms`; both claims must survive.

Similarly, recommendations should be source-native claims.

Bad:

`SituationX -> DO -> AskFollowUpQuestion`

Preferred:

`RecommendationClaim_A`
- source-specific wording and reasoning;
- source-specific context;
- `NORMALIZES_TO -> AskFollowUpQuestion`

`RecommendationClaim_B`
- different author/source;
- different wording;
- `NORMALIZES_TO -> AskFollowUpQuestion`

The shared `CanonicalAction` allows later cross-source retrieval without destroying provenance.

### 1.2 Different conversation contexts must remain separate

A nightclub/dating interaction, a daytime stranger interaction, and a work/social-event interaction can have different playbooks.

Do not create one global social-state machine.

Example:

`ClubContext`
- `Playbook_A_Club`
  - stages/states/rules

`DaytimeStrangerContext`
- `Playbook_A_Daytime`
  - stages/states/rules

Both playbooks may use the same lower-level action:

`AskFollowUpQuestion`

but their top-level flows remain separate.

This matters because the same action may be appropriate in one context but inappropriate in another.

### 1.3 Unknown context is not universal context

A short post or tweet may say:

> "If she says X, say Y."

The source may provide no context.

The system must distinguish:

- `explicit` context — the source explicitly identifies the situation;
- `inferred` context — context is inferred but not directly stated;
- `unspecified` context — the source does not give enough context;
- `general` context — the author explicitly presents it as broadly applicable.

`unspecified` must never be automatically treated as `general`.

Unscoped claims should remain usable knowledge without being inserted arbitrarily into club/daytime/work playbooks.

### 1.4 Rules can depend on earlier interaction history

A recommendation may depend on events far earlier than the current node.

Example requirement:

Current cue:
- she looks away now

Apply the recommendation only if:

- she already looked away at least twice earlier in the conversation

OR

- her friend previously tried to talk to the user.

This is not adequately represented by only graph edges between adjacent states.

The rule needs an explicit typed condition expression such as:

`ALL`
- `CURRENT_CUE(she_looks_away)`
- `ANY`
  - `COUNT(she_looks_away) >= 2 within current_conversation`
  - `OCCURRED(friend_tried_to_talk within current_conversation)`

The model must support nested:

- AND / ALL;
- OR / ANY;
- NOT;
- current cue;
- current state;
- current context;
- event occurrence;
- event count;
- history scope.

The structure should be extensible later for:
- last N turns;
- since a named event;
- time window.

The rule page must show a deterministic readable version of the complete condition locally. The user should not need to trace the graph backward to understand when a rule applies.

### 1.5 Exact wording matters

Exact lines can be useful because the user may want to memorize fallback language for moments when they do not know what to say.

The system must distinguish three cases:

1. `VERBATIM_UTTERANCE`
   - the source presents literal dialogue.

2. `SUGGESTED_WORDING`
   - the source says "you could say...", "say something like...", etc.

3. `ACTION_INSTRUCTION`
   - the source describes an action but does not provide literal wording.
   - Example: "ask her why she chose Spain."

The system must never convert an `ACTION_INSTRUCTION` into invented dialogue.

For exact/suggested wording, the LLM must identify the source span rather than regenerate the text. The program should then copy the exact characters from the original source.

The authoritative data should therefore include:

- source ID;
- source hash;
- line start/end;
- optional character start/end.

The LLM identifies the span. Deterministic code resolves the exact text.

### 1.6 Books can be hierarchical and non-linear

A book may be structured like:

Chapter 1:
- gives preview examples;
- introduces ideas before explaining the complete framework.

Chapter 2:
- defines the full conversation system.

Chapter 3:
- expands one high-level stage into a detailed subflow.

Later chapters:
- add examples, exceptions, refinements, and branches.

The system must not delete or replace the earlier high-level node just because a later chapter provides more detail.

Example:

High-level:

`A -> B -> C`

Later:

`B`
- `B1`
- `B2`
- `B3`

The preferred model is:

`B -> DECOMPOSES_INTO -> B1/B2/B3`

not replacing `B`.

Useful relations include:

- `EXAMPLE_OF`;
- `REFINES`;
- `EXPANDS`;
- `DECOMPOSES_INTO`;
- `PART_OF_SUBFLOW`.

The MVP should ingest books chapter by chapter. Automatic full-book chapter splitting is deferred.

A minimal same-work reconciliation pass is required so later chapters can attach to earlier extracted knowledge.

### 1.7 Example conversations are first-class evidence

A worked dialogue or live interaction should not be reduced to one blob of prose.

Represent:

`ExampleInteraction`
- ordered `ConversationTurn`s;
- speaker role;
- exact source span or timestamped transcript span;
- optional relation to demonstrated rules/stages/actions.

This enables later queries such as:

- "Show me three examples of this rule."
- "Show me how this action appears in different contexts."
- "Show me a complete interaction demonstrating this stage."

### 1.8 Video is first-class source material

For social learning, a transcript alone is insufficient because useful information may include:

- the exact words;
- voice intonation;
- cadence;
- pause timing;
- emphasis;
- volume;
- facial expression;
- gaze;
- posture;
- gesture;
- physical movement;
- the other person's response;
- timing between turns.

The permanent learning reference should often be the original short video clip rather than only an AI description.

The system should still extract searchable descriptions where practical, but these descriptions are secondary to the clip.

AI observation must be kept separate from psychological interpretation.

Example:

Observation:
- speaker looks away for approximately one second.

Author claim:
- the author says this reduces pressure.

AI must not store:
- "looking away means confidence"

as objective truth unless explicitly represented as an attributed interpretation claim.

### 1.9 The video pipeline must minimize user work

The user does not want to manually watch every full video and manually find/cut every useful section.

The automatic pipeline should:

1. obtain the source video;
2. obtain or generate a timestamped transcript;
3. analyze the source;
4. propose useful segments automatically;
5. prepare a LosslessCut project containing those segments;
6. let the user correct only the mistakes;
7. import the corrections;
8. selectively rerun affected processing;
9. materialize only reviewed clips;
10. store only final useful media.

During review the user should be able to:

- adjust a start/end timestamp;
- delete a useless proposed segment;
- add a missed segment.

The full source video should be visible in LosslessCut with the proposed segments already marked.

### 1.10 Video sources can be remote or local

A source may be:

- an internet URL;
- an already-downloaded local file.

The user already has a command-line video downloader.

The system must not implement another downloader. Instead, add a generic adapter/configuration that invokes the user's existing command for remote URLs.

For local sources:
- skip downloading;
- treat the source as user-owned;
- never delete it.

For remote sources:
- download to temporary/cache storage;
- process it;
- preserve only reviewed derived clips;
- delete the full temporary download only when all required derived assets have been safely stored and verified.

### 1.11 Transcript resolution should reuse existing tools

The user already has a command-line Whisper tool.

Transcript resolution priority:

1. explicitly supplied transcript/subtitles;
2. native/downloadable transcript/subtitles available from the source/downloader;
3. configured Whisper command as fallback.

Do not implement speech recognition in this fork.

Normalize every transcript source into one timestamped internal representation.

### 1.12 Clips are derived reusable artifacts

A `MediaSegment` is a logical timestamp range.

A `PhysicalClip` is a derived binary created from a reviewed segment.

These are separate concepts.

Example:

`MediaSegment`
- source video;
- start/end;
- semantic purpose;
- review state.

`PhysicalClip`
- derived file;
- content hash;
- external storage reference.

A single clip may be used by multiple nodes:

`RecommendationClaim_17 -> MediaSegment_8`

`Utterance_4 -> MediaSegment_8`

`ExampleInteraction_3 -> MediaSegment_8`

Do not store one `ownerNodeId` on a clip.

Do not store an authoritative `referenceCount`.

Usage count must be derived from current `MediaUsage` relations.

### 1.13 Clip review must be reversible and selectively rerunnable

The pipeline must track dependencies so corrections do not require full reprocessing.

Cases:

#### Case A: timestamp-only boundary correction

AI proposes:

`12:31.4 - 12:47.8`

User changes to:

`12:29.8 - 12:48.4`

If semantic content is unchanged:

- mark physical clip stale;
- recut/reupload clip;
- do not rerun semantic LLM extraction unnecessarily.

#### Case B: user adds a missed segment

User adds:

`42:13 - 42:31`

The system should:

- create a new reviewed/unclassified segment;
- analyze only that segment;
- identify which existing/new knowledge it supports;
- attach relations;
- avoid reanalyzing the full video.

#### Case C: user changes boundaries enough to change meaning

If the segment now contains materially different content:

- reanalyze only that segment;
- reconcile only affected knowledge nodes.

#### Case D: transcript changes

If a better transcript is generated:

- transcript-derived knowledge may become stale;
- unchanged physical clips should not automatically be regenerated.

#### Case E: extractor/prompt/profile version changes

The system should identify which generated artifacts were created by older versions and allow targeted reprocessing.

### 1.14 Deleting knowledge must not accidentally delete shared media

Deleting one knowledge node should:

- remove that node's `MediaUsage`;
- preserve the segment/clip if other nodes still reference it.

Only garbage collection should delete unreferenced media.

Required workflow:

`media gc --dry-run`

shows:
- clip;
- source;
- timestamps;
- storage reference;
- reason it is unused.

Then actual GC may remove the binary and manifest.

Deleting media evidence must also not automatically delete a knowledge rule that still has other valid source provenance.

If a knowledge item loses its only surviving source/evidence, normal orphan/reconciliation logic should determine what happens to it.

### 1.15 Git remains the structured review/history mechanism

Do not build a second structured-knowledge review application for the MVP.

Preferred workflow:

1. clean working tree;
2. ingest/extract/reconcile;
3. inspect `git diff`;
4. correct/revert as needed;
5. commit approved changes manually.

LosslessCut is the exception because timestamp review benefits from a visual timeline.

Binary clips must not be stored in Git.

Git should store only:

- structured knowledge;
- media manifests;
- segment IDs/timestamps;
- hashes;
- storage references;
- dependency information;
- provenance.

### 1.16 This is a personal fork that must remain easy to sync with upstream

The user wants to regularly merge new upstream `llm-wiki-compiler` updates.

Upstream compatibility is therefore a hard architectural constraint.

Preference order for changes:

1. profile/configuration;
2. isolated new module;
3. small extension point in existing module;
4. invasive core modification only when unavoidable.

Avoid:

- broad refactors;
- moving upstream files;
- renaming upstream APIs;
- social-specific branches in generic modules;
- duplicating existing upstream capabilities.

Keep shared media/provenance code generic because it will later support the software knowledge graph.

### 1.17 AI Blueprint `AGENTS.md` and old repository standards

Running AI Blueprint replaced the original repository `AGENTS.md` with the Blueprint workflow entry point.

This is intentional and should remain.

The older repository-specific `AGENTS.md` is preserved as `AGENTS - Copy.md`.

During adoption, the repository-specific engineering rules from the old file must be migrated into:

`blueprint/context/coding-standards.md`

Do not restore the old file over Blueprint's `AGENTS.md`.

Preserve at least:

- code files under 400 lines excluding comments;
- test files under 400 lines excluding comments;
- functions under 40 lines excluding catch/finally;
- tests under 40 lines excluding catch/finally;
- meaningful names;
- one responsibility per function;
- avoid magic numbers;
- descriptive booleans;
- DRY code;
- avoid deep nesting;
- explain why, not obvious mechanics;
- limit function arguments;
- substantial file-level JSDoc for TypeScript/JavaScript;
- public API/function documentation;
- required verification:
  - `npx tsc --noEmit`
  - `npm run build`
  - `npm test`
  - `fallow`
- `fallow` issues must be fixed before considering work complete.

## 2. Users - Who is this for?

Primary user:

- the owner of this personal fork;
- technically comfortable with Git, CLI workflows, Codex, Claude, Markdown, and local tools;
- wants a durable personal knowledge base and learning system;
- wants AI to perform the expensive extraction/search work and only require human review where AI made mistakes.

Secondary user:

- AI coding agents maintaining the fork through AI Blueprint.

The upstream audience of developers, researchers, and AI agents remains relevant because the fork should preserve the general-purpose nature of `llm-wiki-compiler`.

The first release is:

- single-user;
- local-first;
- CLI-first;
- Git-oriented;
- not a hosted service.

The system should minimize repetitive manual work.

The user should not have to:

- manually find every useful sentence in a book;
- manually trace every graph path to understand a rule;
- manually search through an entire video;
- manually cut every clip;
- rerun an entire source because one clip changed;
- manually delete binary clips after knowledge changes.

## 3. Features - What does the MVP need?

### Shared foundation required by both social and later software graphs

- Stable source identity and source hashing.
- Exact `SourceSpan` support with line and optional character bounds.
- Extraction/prompt/profile version provenance.
- Git-trackable structured output.
- Remote/local video source abstraction.
- Generic command adapter for the user's existing video downloader.
- Generic transcript provider and Whisper-command adapter.
- Normalized timestamped transcript representation.
- Logical `MediaSegment` model separate from physical clip files.
- Many-to-many `MediaUsage`.
- Media analysis-run provenance.
- Dependency tracking and targeted invalidation.
- LosslessCut project import/export integration.
- FFmpeg clip materialization.
- Configurable external filesystem media store.
- Media garbage collection.
- Temporary remote-source cleanup.

### Social-specific MVP

- Social CLP/profile.
- Authors, works, contexts, frameworks, playbooks, stages, states.
- Observable cues.
- Source-specific `InterpretationClaim`.
- Source-specific `RecommendationClaim`.
- Shared `CanonicalAction`.
- Exact utterances, suggested wording, and action instructions.
- Body-language actions.
- Response patterns/branches.
- Recursive historical condition AST.
- Deterministic readable condition rendering.
- Chapter metadata and same-book reconciliation.
- Hierarchical stage/subflow decomposition.
- First-class example interactions and turns.
- Minimal cross-source canonical-action normalization.
- Automatic social-video segment proposals.
- Media links from rules/examples/utterances/body-language knowledge.
- LosslessCut review of proposed segments.
- Selective segment-level reanalysis.

### Explicit MVP exclusions

Do not implement yet:

- Python/C++/Django/software knowledge extraction;
- code-example/best-practice extraction;
- software-video Git repository linking;
- Neo4j;
- another canonical graph database;
- custom graph visualization UI;
- custom video editor;
- provider-specific OneDrive/S3/Google Drive APIs;
- automatic full-book chapter splitting;
- permanent storage of full downloaded videos;
- automatic Git commits;
- advanced consensus/expert scoring;
- author ranking;
- personal outcome-learning;
- sophisticated numerical prosody analysis;
- face recognition;
- emotion detection presented as fact;
- mobile/live conversation coaching;
- new MCP/ChatGPT integration work.

## 4. Data - What are we storing?

### 4.1 Source and provenance

Store:

- source ID;
- source type;
- title;
- author ID;
- work ID;
- chapter number/title where applicable;
- local path or remote URL;
- source hash;
- ingestion metadata;
- extractor version;
- prompt version;
- profile/schema version.

### 4.2 Source spans

Store:

- source ID/path;
- source hash;
- line start;
- line end;
- optional character start;
- optional character end.

Exact source text is resolved deterministically from the source after span validation.

The LLM is never authoritative for "verbatim" text.

### 4.3 Social entities

At minimum:

- `Author`
- `Work`
- `ConversationContext`
- `Playbook`
- `Framework`
- `Stage`
- `State`
- `Cue`
- `InterpretationClaim`
- `RecommendationClaim`
- `CanonicalAction`
- `Utterance`
- `BodyLanguageAction`
- `ResponsePattern`
- `ExampleInteraction`
- `ConversationTurn`

Use existing CLP/entity conventions rather than creating a parallel entity system.

### 4.4 Social relationships

At minimum support equivalents of:

- `AUTHORED_BY`
- `FROM_SOURCE`
- `ASSERTED_BY`
- `USES_FRAMEWORK`
- `HAS_STAGE`
- `HAS_STATE`
- `NEXT_STATE`
- `TRIGGERED_BY`
- `APPLIES_IN`
- `ABOUT_CUE`
- `INTERPRETS_AS`
- `RECOMMENDS_ACTION`
- `HAS_UTTERANCE`
- `HAS_BODY_ACTION`
- `EXPECTS_RESPONSE`
- `LEADS_TO`
- `NORMALIZES_TO`
- `OVERLAPS_WITH`
- `EXAMPLE_OF`
- `REFINES`
- `EXPANDS`
- `DECOMPOSES_INTO`
- `HAS_TURN`
- `DEMONSTRATES`
- `HAS_MEDIA_EXAMPLE`

Exact relation names may follow repository conventions, but the semantics must remain.

### 4.5 Context scope

Store one of:

- `explicit`
- `inferred`
- `unspecified`
- `general`

Do not silently upgrade `unspecified` to `general`.

### 4.6 Utterance kind

Store one of:

- `verbatim`
- `suggested_wording`
- `action_instruction`

An action instruction is not a quotation.

### 4.7 Rule condition AST

Store recursive typed conditions.

MVP node types:

- `all`;
- `any`;
- `not`;
- `current_cue`;
- `current_state`;
- `context`;
- `event_occurred`;
- `count`.

MVP history scopes:

- `current_stage`;
- `current_subflow`;
- `current_conversation`.

Design for future extension:

- `last_n_turns`;
- `since_event`;
- `time_window`.

Store the structured AST as source of truth.

Generate readable condition text deterministically.

### 4.8 Book/chapter data

For each chapter:

- work ID;
- author ID;
- chapter number;
- chapter title;
- source identity/hash.

A later chapter can add relations to earlier extracted entities without deleting them.

### 4.9 Example interaction data

Store:

- interaction ID;
- source;
- ordered turn IDs;
- speaker role;
- exact source span or timestamp;
- demonstrated rule/action/stage relations.

### 4.10 Video source

Store:

- stable video source ID;
- source type: remote/local;
- original URL or local path;
- source hash where practical;
- title/channel/author metadata where available;
- resolved local path while processing;
- ownership:
  - `temporary`
  - `external/user_owned`

Only temporary remote downloads are eligible for cleanup.

### 4.11 Transcript

Store normalized timestamped segments:

- start ms;
- end ms;
- text.

Also store:

- transcript hash;
- origin:
  - supplied;
  - native/downloaded;
  - Whisper;
- relevant generation metadata.

### 4.12 Media analysis run

Store:

- analysis run ID;
- video source ID/hash;
- transcript hash;
- extractor version;
- prompt version;
- profile version;
- produced knowledge IDs;
- produced/proposed segment IDs.

This allows stale-artifact detection.

### 4.13 Media segment

Store:

- stable segment ID;
- source video ID;
- start ms;
- end ms;
- review status:
  - `proposed`;
  - `reviewed`;
  - `rejected`;
- semantic purpose, e.g.:
  - rule demonstration;
  - live interaction;
  - vocal delivery;
  - body language;
  - explanation;
- analysis-run provenance.

A segment is logical metadata, not the binary clip.

### 4.14 Media usage

Store many-to-many relation:

- knowledge node ID;
- media segment ID;
- role:
  - demonstrates;
  - vocal example;
  - body-language example;
  - full interaction;
  - source evidence.

Do not store one clip owner.

Do not store authoritative reference counts.

### 4.15 Physical clip

Store:

- clip ID;
- source video ID;
- reviewed start/end;
- content hash;
- external media-store relative reference/path;
- materialization metadata.

The actual binary is outside Git.

### 4.16 Binary storage

Store outside Git:

- reviewed video clips;
- optional extracted audio clips;
- optional retained frames if later needed.

Do not permanently store full downloaded remote videos.

Original local source videos remain external and user-owned.

## 5. Tech - What stack are we using?

Use the current upstream architecture unless a concrete MVP requirement forces a change.

Keep:

- current supported Node version;
- TypeScript;
- strict typing;
- ESM;
- filesystem-backed state;
- existing CLI architecture;
- existing CLP/profile system;
- existing rule extraction/provider abstractions;
- existing source hashing/provenance/incremental processing;
- dependency-light viewer;
- Git-friendly Markdown/JSON knowledge representation.

### 5.1 Generic new infrastructure

Add isolated generic modules/interfaces for:

- source span resolution;
- video source resolution;
- remote/local ownership;
- downloader command adapter;
- transcript resolver;
- Whisper command adapter;
- timestamped transcript normalization;
- media segment storage;
- media usage;
- analysis-run provenance;
- dependency index;
- invalidation;
- LosslessCut project writer/reader;
- FFmpeg clip materializer;
- filesystem media store;
- garbage collection.

These should not live under a social-only namespace unless they truly contain social semantics.

### 5.2 Social-specific implementation

Social-only functionality includes:

- social CLP/profile;
- social rule extraction prompt/schema;
- social condition semantics where not generic;
- recommendation/interpretation claim extraction;
- same-book framework reconciliation;
- canonical-action normalization;
- social video classification/segment proposal.

### 5.3 Downloader integration

The user already has a command-line video downloader.

Implement a configurable adapter that can execute a command template.

Do not build a downloader.

Remote source:
- invoke configured command;
- write temporary full video.

Local source:
- use existing path;
- skip downloader.

### 5.4 Whisper integration

The user already has a command-line Whisper tool.

Implement a configurable transcript-provider adapter.

Do not implement speech recognition.

Transcript priority:

1. supplied/native transcript;
2. source/downloader transcript;
3. Whisper fallback.

### 5.5 LosslessCut integration

Do not fork or modify LosslessCut for MVP.

Prefer a file-based integration:

- write a LosslessCut project/segment file;
- open the source with proposed segments;
- let the user edit;
- read the saved result;
- diff segments.

Stable segment IDs should survive ordinary boundary edits.

### 5.6 FFmpeg integration

Use FFmpeg for reviewed clip materialization.

Support:
- stream-copy where suitable;
- re-encode when exact boundaries require it.

Hide implementation choice behind a materializer abstraction.

### 5.7 Media storage

Implement a simple filesystem `MediaStore`.

The configured root may point to:

- OneDrive-synced directory;
- Google Drive-synced directory;
- another cloud-backed folder.

Do not add provider APIs in MVP.

Knowledge nodes should reference stable media IDs/relative storage keys, not provider-specific URLs.

### 5.8 Dependency tracking

Track dependencies among:

`VideoSource -> Transcript -> AnalysisRun -> KnowledgeItems -> MediaSegments -> PhysicalClips`

Distinguish invalidation causes:

- video bytes/source changed;
- transcript changed;
- semantic extraction version changed;
- segment boundary changed;
- clip materialization changed.

Avoid broad "everything stale" invalidation when a narrower dependency is known.

### 5.9 Upstream sync strategy

Use Git remotes:

- `origin` = personal fork;
- `upstream` = original `llm-wiki-compiler`.

Prefer frequent upstream sync before large divergence.

Keep custom commits small and focused.

Avoid editing upstream files when a profile/new module/adapter can solve the requirement.

### 5.10 Coding standards after AI Blueprint adoption

The Blueprint `AGENTS.md` remains the workflow entry point.

Move old repository-specific rules from `AGENTS - Copy.md` into:

`blueprint/context/coding-standards.md`

Preserve:

- <400-line code/test file limits;
- <40-line function/test limits;
- clean-code rules;
- JSDoc requirements;
- verification commands;
- `fallow` clean requirement.

## 6. Monetize - How will this make money?

No monetization is planned.

This is a personal local-first tool.

Do not add:

- SaaS accounts;
- subscriptions;
- billing;
- hosted databases;
- telemetry for monetization;
- hosted multi-user infrastructure.

Optimize for:

- reliability;
- maintainability;
- local ownership;
- low recurring cost;
- easy upstream merging.

## 7. UI/UX - How should this look and feel?

### 7.1 General interaction model

CLI-first and file-first.

The system should feel like:

- run a command;
- AI performs extraction;
- Git shows structured changes;
- the user reviews only meaningful differences.

Avoid requiring manual database editing or bespoke administration screens.

### 7.2 Structured knowledge review

Preferred workflow:

1. ensure clean Git working tree;
2. run ingest/extraction/reconciliation;
3. inspect wiki/structured output;
4. inspect `git diff`;
5. edit/revert if needed;
6. manually commit.

Do not auto-commit.

### 7.3 Recommendation display

A recommendation should display locally:

- source/author;
- context;
- playbook/framework;
- stage/state;
- current cue;
- full applicability condition;
- recommended action;
- exact/suggested wording;
- body-language action;
- expected/observed response branch;
- examples;
- media clips;
- provenance.

The user should not have to navigate backward through many graph nodes to reconstruct applicability.

### 7.4 Video ingest UX

Target end-state command conceptually:

`social ingest-video <url-or-path>`

Remote flow:

URL
-> configured downloader
-> temporary video
-> transcript resolution
-> Whisper fallback if needed
-> automatic knowledge/segment extraction
-> write manifests
-> optional review
-> selective reanalysis
-> reviewed clip materialization
-> external media storage
-> temporary full-video cleanup
-> Git diff

Local flow:

local file
-> skip downloader
-> same pipeline
-> never delete original local file.

Exact CLI names should follow existing project conventions.

### 7.5 LosslessCut review UX

Target command conceptually:

`media review <source-id>`

The system should:

1. ensure full source is available;
2. generate LosslessCut project;
3. preload all current segments;
4. open LosslessCut;
5. user adjusts/deletes/adds;
6. import saved project;
7. diff segment changes;
8. determine whether each change needs:
   - recut only;
   - segment semantic reanalysis;
   - usage cleanup;
9. leave structured changes visible in Git.

### 7.6 Review states

Media segment lifecycle:

- `proposed`;
- `reviewed`;
- `rejected`.

Do not permanently materialize every proposed clip before review.

### 7.7 Selective rerun UX

Support targeted commands/workflows conceptually equivalent to:

- refresh one source;
- refresh one segment;
- media-only recut;
- semantic reanalysis;
- stale-media status;
- garbage-collection dry run.

The user should not have to know the full dependency graph manually.

### 7.8 Garbage collection UX

Dry-run first:

`media gc --dry-run`

Display:

- clip ID;
- source;
- timestamp range;
- storage location;
- reason unused.

Then actual delete only when explicitly requested.

### 7.9 Failure behavior

If remote download fails:
- preserve manifests/source registration where useful;
- do not pretend ingest succeeded.

If Whisper fails:
- report transcript unavailable;
- do not fabricate transcript-derived knowledge.

If media upload/store fails:
- do not delete the temporary full remote source needed to retry.

If LosslessCut review import cannot map a segment:
- preserve old state;
- surface the ambiguity rather than silently reassigning identity.

If exact source span fails validation:
- reject the "verbatim" claim rather than storing potentially incorrect exact wording.

## 8. Deployment - Where and how will this ship?

This remains a local npm/CLI-style application/fork.

No hosted deployment is required.

### 8.1 Runtime dependencies

Use existing project dependencies plus configured local executables:

- user video downloader;
- user Whisper tool;
- FFmpeg;
- LosslessCut.

### 8.2 Structured storage

Inside Git repository:

- project plans/profile;
- structured knowledge;
- source metadata;
- source spans;
- media manifests;
- segment metadata;
- dependency metadata;
- storage references.

### 8.3 Binary storage

Outside Git:

- final reviewed clips;
- optional audio/frame assets.

Use configurable filesystem root.

Cloud backup is achieved initially by placing that root in a cloud-synchronized folder.

### 8.4 Temporary storage

Temporary/cache directory may contain:

- downloaded remote full video;
- intermediate transcript files;
- temporary cut outputs;
- LosslessCut review project.

Cleanup rules:

- never delete user-owned local source;
- delete remote full video only after required reviewed artifacts are safely persisted/verified or when an explicit safe cleanup is performed;
- failed downstream storage must preserve enough temporary state to retry where practical.

### 8.5 Fork maintenance

Configure:

- `origin` -> personal fork;
- `upstream` -> original repository.

Before major custom feature work:
- fetch upstream;
- integrate upstream changes;
- resolve conflicts early.

Architecture should minimize long-lived modifications to upstream hotspots.

### 8.6 MVP acceptance dataset

Use a deliberately small but representative dataset.

#### Book source

Several chapters from one social-interaction book:

- chapter 1 contains preview examples;
- chapter 2 defines a high-level framework;
- chapter 3 decomposes one stage;
- at least one exact line;
- at least one suggested line;
- at least one non-verbatim action instruction;
- body-language interpretation;
- history-dependent recommendation.

#### Second text source

One small independent source containing:

- a different recommendation;
- or an unscoped recommendation;
- or a different interpretation of a shared cue.

#### Video source

One social-interaction video containing:

- a spoken demonstration;
- a delivery/body-language demonstration;
- a useful clip that needs timestamp adjustment;
- at least one place where adding/removing a segment during review can be exercised.

### 8.7 MVP acceptance behavior

The MVP is complete when the user can:

1. ingest the book chapters;
2. preserve work/author/chapter provenance;
3. inspect the author's framework without merging it with other sources;
4. see exact source wording copied from validated spans;
5. distinguish suggested wording from action instructions;
6. inspect one nested historical condition;
7. see readable applicability locally;
8. ingest the second source without forcing it into the first framework;
9. ingest a remote or local social video;
10. get transcript automatically with Whisper fallback;
11. receive automatically proposed media segments;
12. open the full source in LosslessCut with those segments marked;
13. adjust one segment;
14. delete one segment;
15. add one missed segment;
16. import review changes;
17. recut boundary-only changes without unnecessary semantic LLM work;
18. analyze a newly added segment without reanalyzing the entire video;
19. materialize reviewed clips only;
20. store clips outside Git;
21. reference one clip from multiple knowledge nodes;
22. delete one knowledge usage without deleting still-shared media;
23. dry-run GC and remove a truly unused clip;
24. inspect all structured changes in `git diff`;
25. rerun without unnecessarily processing unchanged sources.

### 8.8 Post-MVP direction

After the social MVP is stable, create a separate software knowledge project/profile covering:

- Python;
- C++;
- Django/FastAPI;
- SQL/databases;
- design principles;
- design patterns;
- architecture;
- distributed systems;
- system design;
- code examples;
- best practices;
- pitfalls;
- tradeoffs.

Reuse the generic source/provenance/media pipeline.

Later software-video support should additionally associate relevant segments with:

- Git repository;
- commit SHA;
- tag/release where relevant;
- file path;
- line range.

Repository code should be preferred over reconstructing code from video pixels when the linked source is available.
