# llm-wiki-compiler Social Knowledge Fork - Project Overview

<!-- blueprint:source-hash 72888cc2d81ba313db849d5bb2ac0d0e1ee4f7912d2deee95c4ccb6aaa273f43 -->

> **Generated file. Do not hand-edit.** Regenerate it with `/overview` whenever
> `blueprint/project-plan.md` or `blueprint/build-plan.md` changes.

> A local-first personal fork of `llm-wiki-compiler` that compiles social-interaction
> text and video into source-backed, Git-reviewable structured knowledge while
> remaining easy to synchronize with upstream.

## Problem

Generic summarization loses the source-specific frameworks, context, history,
wording, observable evidence, and conditional branches needed to study complex
social-interaction material. This fork must preserve each author's theory and
provenance while still normalizing reusable lower-level concepts for retrieval.

The system must also reduce manual work. It should resolve source identity once,
extract useful knowledge and video segments automatically, support targeted
correction and reprocessing, and leave deterministic structured changes for Git
review instead of requiring a custom database or review application.

The first product slice is a Social Interaction Knowledge Graph MVP. Shared
source, provenance, invalidation, and media infrastructure must remain generic
enough to support a separate software knowledge graph after the MVP.

## Users

- **Primary user: the fork owner** - a technically comfortable single user who
  works locally with Git, CLI tools, Markdown, Codex, and Claude, and wants AI to
  perform costly extraction while limiting human work to meaningful review.
- **Secondary user: AI coding agents** - agents maintaining the fork through the
  AI Blueprint workflow and relying on stable plans, explicit provenance, and
  deterministic files.
- **Inherited upstream audience** - developers, researchers, and agents that
  benefit from preserving the general-purpose compiler, SDK, MCP, profile, and
  viewer architecture.

There are no accounts, roles, access tiers, hosted users, or multi-user behavior
in the first release.

## Core product rules

- Source-native claims remain distinct. Different authors may disagree without
  either claim being deleted or forced into a universal theory.
- `unspecified` context is not `general` context.
- Canonical Author, Work, and source identity are corrected once and referenced
  by derived knowledge rather than copied into every node.
- Critical identity ambiguity pauses expensive semantic extraction. Optional
  display metadata does not.
- Exact or suggested wording comes from validated source spans. The model never
  regenerates authoritative quotations.
- Rules use a recursive condition AST so current context and earlier interaction
  history can be evaluated and rendered deterministically.
- Later book chapters refine or decompose earlier framework nodes rather than
  deleting useful high-level structure.
- Generation provider, model, source hash, extractor, prompt, and profile/schema
  versions are recorded for every semantic extraction run.
- Model preference is ordered configuration, not a per-node quality score. A
  preference change identifies upgrade candidates but never triggers automatic
  library-wide re-extraction.
- Explicit re-extraction may bypass unchanged-source skipping. Accepted output
  replaces lower-authority source-owned knowledge while preserving shared
  canonical entities still referenced elsewhere.
- A lower-preference rerun never silently downgrades a higher-preference
  authoritative extraction.
- Model preference resolves competing runs for the same source only. It never
  erases genuine disagreement between independent sources.
- Git is the structured review and history mechanism. The application never
  auto-commits.
- Binary clips stay outside Git. Git stores stable metadata, hashes, timestamps,
  references, dependencies, and provenance.
- User-owned local sources are never deleted. Temporary remote downloads are
  removed only after required derived assets are safely persisted and verified.
- Upstream compatibility is a hard constraint. Prefer configuration, profiles,
  isolated modules, and small extension points over invasive core changes.

## Features

### MVP - Upstream-safe shared foundation

1. **Blueprint coding standards migration** - completed migration of preserved repository rules into the Blueprint coding standards while keeping Blueprint `AGENTS.md` authoritative.
2. **Upstream-sync baseline** - document fork remotes and the profile/module/extension-point preference for custom work.
3. **Stable source identity** - give text and video sources stable IDs, hashes, types, identity metadata, and extraction-version provenance.
4. **Canonical source metadata and ambiguity handling** - resolve Author, Work, and source identity once and gate extraction only on unresolved critical identity.
5. **Exact source spans** - validate line and character ranges and resolve authoritative exact text deterministically.
6. **Generation provenance and model preference** - record complete extraction provenance and configure an ordered model authority policy without automatic reprocessing.

### MVP - Social text knowledge

7. **Social CLP profile** - define social entities and relationships using the existing profile system.
8. **Source-specific recommendation claims** - preserve recommendations as attributed source claims instead of universal rules.
9. **Source-specific interpretation claims** - separate observable cues from each source's interpretation.
10. **Context scope semantics** - distinguish explicit, inferred, unspecified, and explicitly general context.
11. **Utterance classification** - separate verbatim wording, suggested wording, and non-verbatim action instructions.
12. **Recursive rule conditions** - model nested Boolean, current-state, occurrence, and count-based historical prerequisites.
13. **Readable rule applicability** - render complete rule conditions locally without graph backtracking.
14. **Book chapter metadata** - preserve stable Work, Author, chapter, source, and hash identity across separate chapter ingestion.
15. **Same-book reconciliation** - connect previews, refinements, expansions, and later chapter knowledge to earlier entities.
16. **Hierarchical subflows** - retain high-level framework nodes while attaching detailed decompositions.
17. **Canonical lower-level actions** - normalize reusable actions across claims without deleting source-native recommendations.
18. **Example interactions** - represent conversations as ordered source-backed turns linked to demonstrated knowledge.
19. **Git-trackable social output** - persist deterministic social knowledge and provenance in the existing file-backed model.
20. **Authoritative source re-extraction** - deliberately rerun one unchanged source, reconcile authoritative output, remove unsupported source-owned knowledge, and preserve shared entities.

### MVP - Shared media ingest

21. **Video source abstraction** - support remote URLs and user-owned local files with explicit ownership and cleanup rules.
22. **Downloader command adapter** - invoke the user's existing configured downloader instead of implementing one.
23. **Transcript normalization** - normalize supplied, native, downloaded, and Whisper transcripts into one timestamped representation.
24. **Whisper fallback adapter** - invoke the user's existing Whisper command only when another usable transcript is unavailable.
25. **Media segment model** - represent logical timestamp ranges separately from physical clip files and track review state.
26. **Many-to-many media usage** - let one segment support multiple knowledge items without a single owner or stored reference count.
27. **Media analysis provenance** - record video, transcript, extraction-version, dependency, knowledge, and segment outputs per analysis run.

### MVP - Automatic social video extraction

28. **Social video knowledge extraction** - extract source-backed social knowledge while keeping observations separate from interpretations.
29. **Automatic segment proposals** - identify useful spoken, delivery, body-language, explanatory, and interaction ranges.
30. **Segment-to-knowledge linking** - connect proposed segments to the knowledge they demonstrate or evidence.
31. **Exact shared-segment reuse** - reuse a matching reviewed source range instead of creating duplicate clip artifacts.

### MVP - Human video review

32. **LosslessCut project writer** - generate a review project with the full source and labeled stable segments.
33. **LosslessCut review importer** - import boundary edits, deletions, and newly added missed segments.
34. **Stable segment identity** - preserve segment IDs across ordinary edits and surface ambiguous mappings.

### MVP - Selective invalidation and reruns

35. **Media dependency index** - track dependencies from video and transcript through knowledge, segments, and clips.
36. **Boundary-only invalidation** - recut semantic-preserving timestamp edits without unnecessary model extraction.
37. **New-segment analysis** - analyze one user-added segment without reprocessing the full source.
38. **Changed-segment reanalysis** - reanalyze materially changed segments and reconcile only affected knowledge.
39. **Transcript-change invalidation** - stale transcript-derived knowledge without automatically invalidating unchanged clips.
40. **Extractor-version invalidation** - identify older generated artifacts and support targeted refresh.

### MVP - Clip materialization and storage

41. **FFmpeg clip materializer** - create reviewed clips with stream-copy or exact re-encoding behind one abstraction.
42. **Filesystem media store** - place binaries under a configurable external filesystem root.
43. **Clip hashing and verification** - hash and verify stored clips before temporary source cleanup.
44. **Safe remote-source cleanup** - delete only verified temporary downloads and never user-owned local files.

### MVP - Media lifecycle

45. **Usage cleanup on knowledge changes** - remove affected media links while preserving still-supported knowledge and media.
46. **Media GC dry run** - report unreferenced clips and exact deletion reasons before mutation.
47. **Media garbage collection** - remove only clips with zero live usages.
48. **Clip deduplication** - reuse safe identical reviewed artifacts instead of rematerializing them.

### MVP - End-to-end user workflow

49. **Remote video end-to-end flow** - prove remote ingest through download, transcript, extraction, review, storage, cleanup, and Git-visible manifests.
50. **Local video end-to-end flow** - prove the same pipeline for a local source while preserving the original file.
51. **Representative book workflow** - prove chapters, framework hierarchy, wording classes, interpretations, and historical conditions on a small book dataset.
52. **Second-source coexistence** - prove an independent or unscoped source remains distinct from the first author's framework.
53. **LosslessCut correction scenario** - prove edit, delete, add, import, and selective reprocessing in one review round trip.
54. **Shared-clip deletion scenario** - prove one deleted usage cannot remove a clip still used elsewhere.
55. **Deterministic Git review** - ensure extraction and reconciliation produce stable understandable diffs without auto-commit.
56. **MVP CLI documentation** - document ingest, external-tool configuration, review, refresh, materialization, status, and garbage collection.

### Post-MVP - Software knowledge graph

57. **Software knowledge profile** - add a separate profile for software concepts, examples, practices, pitfalls, and tradeoffs.
58. **First-class code examples** - preserve source-backed code with language/framework metadata and conceptual links.
59. **Technical video repository links** - link segments to repositories, commits, files, and line ranges, preferring repository code over pixels.
60. **Technical visual extraction** - extract screenshots or frames only when linked source material is insufficient.

### Post-MVP - Broader growth

61. **Automatic book chapter splitting** - segment complete books after chapter-by-chapter ingestion is proven.
62. **Advanced cross-source synthesis** - add richer agreement, disagreement, confidence, and normalization views without deleting source-native claims.
63. **Optional query integrations** - evaluate MCP and agent integrations after the file-backed model and APIs stabilize.
64. **Optional graph projection/UI** - evaluate richer graph projections only as derived views, not canonical storage.

## Data model

Serialized shapes must follow existing llmwiki entity, frontmatter, JSON/JSONL,
validation, atomic-write, and provenance conventions. The logical contracts below
are authoritative even when final field names differ to preserve upstream
compatibility.

### Shared scalar types

- `EntityId` (`string`) - stable repository-native identifier.
- `ContentHash` (`string`) - deterministic source, transcript, or clip hash.
- `TimestampMs` (`integer`) - non-negative video/transcript offset in milliseconds.
- `MetadataStatus` (`needs_metadata | needs_review | confirmed`) - whether critical identity is unresolved or accepted.
- `ContextScope` (`explicit | inferred | unspecified | general`) - how strongly a context is asserted by the source.
- `UtteranceKind` (`verbatim | suggested_wording | action_instruction`) - whether text is literal, suggested, or descriptive.
- `SegmentReviewStatus` (`proposed | reviewed | rejected`) - human review state for a logical media segment.
- `SourceOwnership` (`temporary | external/user_owned`) - whether a resolved video file is eligible for cleanup.

### Author

- `id` (`EntityId`) - canonical Author identity.
- `name` (`string`) - canonical display name corrected in one location.
- related from one or more `Work.authorIds` values.

Derived claims do not own authoritative Author strings. They resolve Author
through their Source or Chapter and canonical Work.

### Work

- `id` (`EntityId`) - canonical Work identity.
- `title` (`string`) - canonical title.
- `authorIds` (`EntityId[]`) - canonical Authors connected by `AUTHORED_BY`.
- `publicationYear` (`integer?`) - optional non-critical display metadata.
- has many chapter `Source` records.

### DetectedMetadataCandidate

- `field` (`string`) - detected identity or descriptive field such as Author or Work.
- `value` (`string`) - detected value before acceptance.
- `confidence` (`number?`) - detector confidence when available.

Detected candidates are evidence, not canonical identity.

### Source

- `id` (`EntityId`) - stable source identity.
- `type` (`book_chapter | article | note | post | video | other`) - source kind.
- `title` (`string`) - source or chapter display title.
- `workId` (`EntityId?`) - canonical Work reference.
- `authorIds` (`EntityId[]?`) - direct canonical Authors only where the source cannot resolve through a Work.
- `chapterNumber` (`number | string?`) - source-native chapter position.
- `chapterTitle` (`string?`) - chapter display title.
- `localPath` (`string?`) - local source location.
- `remoteUrl` (`string?`) - remote source location.
- `hash` (`ContentHash`) - current source version.
- `metadataStatus` (`MetadataStatus`) - critical identity review state.
- `detectedMetadata` (`DetectedMetadataCandidate[]`) - unaccepted candidates.
- `ingestionMetadata` (`record`) - repository-native ingest details.
- has many `SourceSpan`, `ExtractionRun`, claims, interactions, and media records.

A Source with unresolved critical identity is registered but does not proceed to
expensive semantic extraction. Confirmed Work and Author references are reused by
later chapters.

### SourceSpan

- `sourceId` (`EntityId`) - owning Source.
- `sourceHash` (`ContentHash`) - exact source version being cited.
- `lineStart` / `lineEnd` (`integer`) - inclusive validated line bounds.
- `characterStart` / `characterEnd` (`integer?`) - optional exact character bounds.

Exact text is copied deterministically from a validated span. LLM output is never
the authority for verbatim text.

### ExtractionRun

- `id` (`EntityId`) - stable run identifier.
- `sourceId` (`EntityId`) - extracted Source.
- `sourceHash` (`ContentHash`) - extracted source version.
- `provider` (`string`) - generation provider.
- `model` (`string`) - generation model.
- `extractorVersion` (`string`) - extractor/schema implementation version.
- `promptVersion` (`string | integer`) - prompt contract version.
- `profileVersion` (`string | integer`) - CLP/profile/schema version.
- `authorityState` (`repository-native authority/review state`) - at least authoritative versus non-authoritative candidate/superseded output.
- `producedKnowledgeIds` (`EntityId[]`) - source-owned knowledge generated by the run.
- `producedRelationshipIds` (`EntityId[]`) - source-owned relationships generated by the run when relationships have stable IDs.

Only the accepted run supplies live source-owned knowledge. A configured
`extractionModelPreference: string[]` orders models without embedding scores in
nodes or scheduling automatic extraction.

### ConversationContext

- `id` (`EntityId`) - stable context such as club, daytime stranger, or work event.
- `name` (`string`) - display label.
- has distinct `Playbook` records; contexts are never collapsed into one global state machine.

### Framework and Playbook

- `id` (`EntityId`) - stable framework or playbook identity.
- `sourceId` (`EntityId`) - source that asserts it.
- `contextId` (`EntityId?`) - applicable conversation context.
- `stageIds` (`EntityId[]`) - ordered or related stages.
- may be linked by `REFINES`, `EXPANDS`, or `OVERLAPS_WITH` without losing source ownership.

### Stage and State

- `id` (`EntityId`) - stable source-backed stage or state.
- `frameworkId` / `playbookId` (`EntityId`) - owning framework path.
- `sourceId` (`EntityId`) - source provenance.
- `childIds` (`EntityId[]`) - detailed nodes linked through `DECOMPOSES_INTO` or `PART_OF_SUBFLOW`.
- `nextStateIds` (`EntityId[]`) - source-defined flow transitions.

High-level nodes survive when later chapters add detailed subflows.

### Cue

- `id` (`EntityId`) - reusable observable cue identity.
- `description` (`string`) - observation without ungrounded psychological meaning.

### InterpretationClaim

- `id` (`EntityId`) - source-native claim.
- `sourceId` (`EntityId`) - asserting Source.
- `cueId` (`EntityId`) - observed Cue.
- `interpretation` (`string`) - what this source says the cue means.
- `contextId` (`EntityId?`) and `contextScope` (`ContextScope`) - applicability and evidence status.
- `sourceSpanIds` (`EntityId[]`) - supporting exact text spans where available.
- `extractionRunId` (`EntityId`) - generation provenance.

Different sources can hold incompatible InterpretationClaims about the same Cue.

### RecommendationClaim

- `id` (`EntityId`) - source-native recommendation.
- `sourceId` (`EntityId`) - asserting Source.
- `contextId` (`EntityId?`) and `contextScope` (`ContextScope`) - applicability and scope strength.
- `frameworkId`, `stageId`, `stateId` (`EntityId?`) - optional placement in a source framework.
- `condition` (`RuleCondition?`) - full applicability expression.
- `canonicalActionId` (`EntityId?`) - optional normalization target.
- `utteranceIds`, `bodyLanguageActionIds`, `responsePatternIds` (`EntityId[]`) - associated guidance.
- `sourceSpanIds` (`EntityId[]`) - source evidence.
- `extractionRunId` (`EntityId`) - generation provenance.

RecommendationClaims remain separate across sources even when they normalize to
the same action.

### CanonicalAction

- `id` (`EntityId`) - shared lower-level action identity.
- `name` (`string`) - neutral action label.
- referenced by many RecommendationClaims through `NORMALIZES_TO`.

CanonicalActions are shared objects, not source-owned claims. Re-extracting one
source cannot delete an action still referenced by another source.

### Utterance

- `id` (`EntityId`) - source-backed utterance or instruction.
- `sourceId` (`EntityId`) - Source provenance.
- `kind` (`UtteranceKind`) - literal, suggested, or non-verbatim instruction.
- `sourceSpanId` (`EntityId?`) - required for authoritative exact or suggested wording.
- `instruction` (`string?`) - descriptive action text for `action_instruction`.

An `action_instruction` must never be converted into invented dialogue.

### BodyLanguageAction and ResponsePattern

- `id` (`EntityId`) - source-backed guidance or expected/observed branch.
- `sourceId` (`EntityId`) - Source provenance.
- `description` (`string`) - grounded source content.
- linked from RecommendationClaims and optionally to States, Cues, or media evidence.

### RuleCondition

Recursive logical union:

- `all` / `any` - contains `RuleCondition[]`.
- `not` - contains one `RuleCondition`.
- `current_cue` - references a `Cue`.
- `current_state` - references a `State`.
- `context` - references a `ConversationContext`.
- `event_occurred` - references an event and one history scope.
- `count` - references an event, comparison, threshold, and one history scope.

MVP history scopes are `current_stage`, `current_subflow`, and
`current_conversation`. The AST is authoritative; readable applicability text is
generated deterministically.

### ExampleInteraction and ConversationTurn

`ExampleInteraction` fields:

- `id` (`EntityId`) - interaction identity.
- `sourceId` (`EntityId`) - source provenance.
- `turnIds` (`EntityId[]`) - ordered ConversationTurns.
- `demonstratedKnowledgeIds` (`EntityId[]`) - linked rules, actions, or stages.

`ConversationTurn` fields:

- `id` (`EntityId`) - turn identity.
- `interactionId` (`EntityId`) - owning ExampleInteraction.
- `order` (`integer`) - deterministic turn sequence.
- `speakerRole` (`string`) - source-defined speaker role.
- `sourceSpanId` (`EntityId?`) - text evidence.
- `transcriptSegmentIds` (`EntityId[]?`) - timestamped video evidence.

### VideoSource

- `sourceId` (`EntityId`) - Source record with type `video`.
- `origin` (`remote | local`) - resolution path.
- `originalUrl` (`string?`) - remote source URL.
- `originalLocalPath` (`string?`) - user-owned local source path.
- `resolvedLocalPath` (`string?`) - current processing path.
- `sourceHash` (`ContentHash?`) - resolved video version when practical.
- `ownership` (`SourceOwnership`) - cleanup eligibility.
- `title`, `channel`, `authorMetadata` (`string?`) - detected display metadata.

### Transcript and TranscriptSegment

`Transcript` fields:

- `id` (`EntityId`) - normalized transcript identity.
- `videoSourceId` (`EntityId`) - owning VideoSource.
- `hash` (`ContentHash`) - transcript version.
- `origin` (`supplied | native/downloaded | whisper`) - resolution source.
- `segmentIds` (`EntityId[]`) - ordered timestamped segments.
- `generationMetadata` (`record?`) - relevant provider/tool provenance.

`TranscriptSegment` fields:

- `id` (`EntityId`) - stable segment identity where available.
- `startMs` / `endMs` (`TimestampMs`) - timestamp bounds.
- `text` (`string`) - normalized transcript text.

### MediaAnalysisRun

- `id` (`EntityId`) - media analysis run.
- `videoSourceId` (`EntityId`) and `videoSourceHash` (`ContentHash`) - analyzed media version.
- `transcriptHash` (`ContentHash`) - analyzed transcript version.
- `provider`, `model` (`string`) - generation provenance when semantic analysis uses a model.
- `extractorVersion`, `promptVersion`, `profileVersion` (`string | integer`) - extraction contract versions.
- `producedKnowledgeIds`, `producedSegmentIds` (`EntityId[]`) - generated dependencies.

### MediaSegment

- `id` (`EntityId`) - stable logical segment identity.
- `videoSourceId` (`EntityId`) - source video.
- `startMs` / `endMs` (`TimestampMs`) - logical bounds.
- `reviewStatus` (`SegmentReviewStatus`) - proposed, reviewed, or rejected.
- `semanticPurpose` (`rule_demonstration | live_interaction | vocal_delivery | body_language | explanation | string`) - why the range matters.
- `analysisRunId` (`EntityId`) - proposal or analysis provenance.

A MediaSegment is metadata, not a binary clip. Ordinary boundary edits should
preserve its ID when review mapping is unambiguous.

### MediaUsage

- `knowledgeNodeId` (`EntityId`) - knowledge item using evidence.
- `mediaSegmentId` (`EntityId`) - logical segment used.
- `role` (`demonstrates | vocal_example | body_language_example | full_interaction | source_evidence`) - evidence role.

This is a many-to-many relation. Reference counts are derived from current
MediaUsage records and are never authoritative stored fields.

### PhysicalClip

- `id` (`EntityId`) - derived clip identity.
- `videoSourceId` (`EntityId`) - original source video.
- `mediaSegmentId` (`EntityId`) - reviewed logical segment.
- `startMs` / `endMs` (`TimestampMs`) - materialized bounds.
- `contentHash` (`ContentHash`) - finalized binary hash.
- `storageKey` (`string`) - relative reference under the external MediaStore root.
- `materializationMetadata` (`record`) - FFmpeg mode and relevant version/output details.

The binary is outside Git. One physical artifact may be reused where reviewed
source ranges and content are safely identical.

### DependencyRecord

- `upstreamId` (`EntityId`) - dependency source.
- `downstreamId` (`EntityId`) - generated or derived artifact.
- `dependencyKind` (`string`) - source, transcript, extraction, knowledge, segment, or clip relation.
- `versionOrHash` (`string`) - version used when producing the downstream artifact.
- `staleReason` (`string?`) - precise invalidation cause.

Required dependency paths include:

- `CanonicalMetadata -> SourceVersion -> ExtractionRun -> SourceOwnedKnowledge`
- `VideoSource -> Transcript -> MediaAnalysisRun -> KnowledgeItems -> MediaSegments -> PhysicalClips`

Metadata-only corrections should not stale semantics. Work identity, explicit
social context, source hierarchy, transcript meaning, or extraction-version
changes invalidate only affected downstream artifacts.

## Reconciliation and lifecycle rules

### Canonical metadata

- High-confidence critical identity can be accepted automatically.
- Unresolved critical identity enters metadata review before semantic extraction.
- Optional display metadata does not block ingestion.
- Correcting canonical metadata updates dependent views through references.
- A Work or semantic-context change may require targeted reconciliation and
  re-extraction; spelling or publication-year changes normally do not.

### Authoritative source re-extraction

- Normal incremental processing skips unchanged sources.
- An explicit selected-source rerun may bypass that skip for a chosen model or
  newer extractor, prompt, or profile.
- A successful higher-preference run can become authoritative for that Source.
- Matching source-owned knowledge is retained or updated, unsupported
  source-only knowledge is removed, and newly supported knowledge is added.
- Shared canonical entities survive while any other source references them.
- A lower-preference rerun remains non-authoritative or requires explicit
  override; it never silently downgrades live knowledge.
- Independent sources retain conflicting claims regardless of model preference.
- Replacement is atomic where existing candidate/review facilities permit.
- Git diff and extraction provenance provide history and rollback; conflicting
  model versions are not kept as simultaneously live knowledge.

### Media invalidation and cleanup

- A semantic-preserving boundary edit stales only the physical clip.
- A newly added segment is analyzed independently.
- A meaning-changing boundary edit reanalyzes only the affected segment and
  reconciles only its dependent knowledge.
- A transcript change stales transcript-derived semantics but not unchanged
  physical clips by default.
- Deleting knowledge removes its MediaUsage records but preserves shared media.
- Garbage collection deletes only clips with zero live usages and requires a
  useful dry run first.

## Tech stack

- **Node.js** - retain the current upstream-supported runtime for the npm CLI and library.
- **TypeScript with strict typing** - preserve the existing typed implementation and public boundaries.
- **ESM** - keep the existing module format.
- **Existing CLI, SDK, MCP, and lightweight viewer** - preserve upstream architecture; do not replace it with a hosted service or frontend framework.
- **CLP/profile system** - contain social schema and extraction behavior in profiles or isolated social modules.
- **Existing provider abstractions** - run model extraction and capture provider/model provenance.
- **Filesystem-backed Markdown, YAML frontmatter, JSON, and JSONL** - canonical Git-trackable structured state.
- **Git** - structured review, history, rollback, and manual approval mechanism.
- **Configured downloader adapter** - invoke the user's existing command for remote video sources.
- **Configured Whisper adapter** - invoke the user's existing speech-to-text command only as transcript fallback.
- **LosslessCut file integration** - review proposed ranges on the full video without building a custom editor.
- **FFmpeg** - materialize reviewed clips through stream-copy or exact re-encoding.
- **Filesystem MediaStore** - keep binaries outside Git under a configurable local or cloud-synchronized root.

Architecture preference for fork-specific work:

1. profile or configuration;
2. isolated new module;
3. small extension point in an existing module;
4. invasive core change only when unavoidable.

Do not add Neo4j or another canonical database, broad upstream refactors,
provider-specific cloud storage APIs, a custom graph UI, a custom video editor,
or automatic Git commits for the MVP.

## Verification constraints

Repository work must preserve the adopted standards:

- code and test files below 400 lines excluding comments;
- functions and individual tests below 40 lines excluding `catch` and `finally`;
- meaningful names, focused responsibilities, shallow control flow, and no
  unexplained magic values;
- substantial file-level JSDoc for TypeScript/JavaScript and public API docs;
- `npx tsc --noEmit`;
- `npm run build`;
- `npm test`;
- `fallow`, using the repository-owned command where configured, with no
  unresolved findings before completion.

## Monetization

No monetization is planned. This is a personal, local-first, open-source-style
fork with no SaaS accounts, subscriptions, billing, hosted database, telemetry,
or multi-user infrastructure.

## UI/UX

The product is CLI-first and file-first. The normal interaction is to run a
focused command, let extraction or reconciliation update deterministic files,
inspect the generated knowledge and `git diff`, correct or revert as needed, and
commit manually.

### Main workflows

- **Text/source ingest** - register a source, resolve identity, stop for critical
  metadata review when needed, then extract source-backed knowledge.
- **Metadata review** - show detected candidates and confidence, accept one
  canonical Author/Work association, and explain whether a correction is
  metadata-only or semantically invalidating.
- **Recommendation review** - display source/author, context, framework,
  stage/state, cue, complete applicability, action, wording class, body guidance,
  response branch, examples, media, and provenance together.
- **Selected-source re-extraction** - show current and requested provider/model
  and version provenance, classify upgrade/equivalent/downgrade, reconcile
  source-owned changes, and expose the result through Git diff.
- **Remote video ingest** - configured download, transcript resolution, semantic
  extraction, proposed segments, review, selective reanalysis, clip storage,
  safe temporary cleanup, and Git-visible manifests.
- **Local video ingest** - the same pipeline without download and with an
  absolute guarantee that the original local source is not deleted.
- **LosslessCut review** - open the full source with proposed segments, let the
  user adjust/delete/add ranges, import the result, and classify each change as
  recut, semantic reanalysis, or usage cleanup.
- **Selective refresh** - refresh one source or segment, recut media only, run
  semantic reanalysis, and inspect stale status without understanding the full
  dependency graph manually.
- **Media garbage collection** - show clip, source, timestamp, storage key, and
  reason unused in a dry run before explicit deletion.

Exact command names must follow existing llmwiki conventions. The project plan
uses conceptual examples such as `social ingest-video <url-or-path>`,
`media review <source-id>`, and `media gc --dry-run`; these are not yet fixed CLI
contracts.

### Failure behavior

- Preserve source registration and detected metadata when critical identity is unresolved.
- Never fabricate transcript-derived knowledge after transcript failure.
- Never delete retryable temporary source media after downstream storage failure.
- Preserve old segment state when a LosslessCut import cannot map identity safely.
- Reject invalid exact-span claims instead of storing uncertain quotations.
- Preserve the prior authoritative graph if re-extraction or reconciliation fails.
- Refuse a silent model-authority downgrade.

## Deployment

- **Application shape** - local npm CLI/library fork; no hosted deployment, domain, health endpoint, worker, or cron job is required.
- **Runtime** - current upstream-supported Node.js plus user-configured downloader, Whisper, FFmpeg, and LosslessCut executables.
- **Build** - `npm run build`; completion also requires typecheck, tests, and Fallow as documented above.
- **Start** - no long-running production service is planned; users invoke the CLI and lightweight local viewer through existing project commands.
- **Canonical structured storage** - repository files under Git containing knowledge, metadata, spans, extraction runs, dependencies, media manifests, and storage references.
- **Binary storage** - configurable external filesystem root, optionally inside a cloud-synchronized folder; no provider-specific storage API in the MVP.
- **Temporary storage** - downloaded remote videos, intermediate transcripts, cut outputs, and LosslessCut projects; cleanup follows ownership and verification rules.
- **Fork maintenance** - `origin` is the personal fork and `upstream` is the original repository; sync frequently and avoid long-lived edits to upstream hotspots.

> TODO: The plans intentionally leave exact configuration keys and CLI names for
> downloader, Whisper, media root, extraction model preference, and targeted
> re-extraction to repository-aware feature specs.

## Acceptance target

Use a small representative dataset containing several chapters from one book,
one independent text source, and one social-interaction video. The MVP must prove:

- ambiguous critical identity pauses extraction and accepted metadata is reused;
- metadata-only correction propagates without semantic re-extraction, while Work
  identity or semantic-context changes invalidate the appropriate dependencies;
- source-specific frameworks, interpretations, contexts, wording classes,
  hierarchical subflows, examples, and historical conditions survive extraction;
- an unchanged Flash-extracted source can be rerun deliberately with Pro, the
  accepted Pro result replaces conflicting Flash-owned live knowledge, and a
  later lower-preference rerun cannot silently downgrade it;
- shared canonical entities and shared media survive while referenced elsewhere;
- remote and local video sources follow safe ownership and transcript rules;
- LosslessCut edits trigger only the necessary recut, reanalysis, or cleanup;
- reviewed clips are materialized outside Git, shared through MediaUsage, and
  garbage-collected only after the final usage disappears;
- all structured changes are deterministic, understandable in `git diff`, and
  never auto-committed.

## Open questions

No material contradiction exists between `project-plan.md` and
`build-plan.md`. The following implementation choices remain intentionally open
and must be resolved through repository-aware feature specs rather than guessed:

- the exact supported Node.js version to lock in the plans;
- repository-native file shapes and field names for canonical metadata,
  authority state, source ownership, and candidate review;
- confidence thresholds and rules for automatic metadata acceptance;
- source-owned knowledge identity matching during reconciliation;
- exact CLI command names and configuration keys for model preference,
  re-extraction, downloader, Whisper, and media storage;
- the safe default representation or lifetime of non-authoritative lower-model
  candidate/debug output;
- the threshold or user signal that distinguishes boundary-only correction from
  a semantic segment change;
- LosslessCut project format details and ambiguous segment-mapping behavior;
- the filesystem MediaStore key layout and retry/retention policy.
