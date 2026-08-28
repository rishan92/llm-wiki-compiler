# Build Plan

## MVP - Upstream-safe shared foundation

- [x] 1. **Blueprint coding standards migration** - preserve the new Blueprint `AGENTS.md` and migrate the original repository engineering rules from `AGENTS - Copy.md` into `blueprint/context/coding-standards.md`.
- [ ] 2. **Upstream-sync baseline** - document `origin`/`upstream` fork maintenance and establish the rule that custom work prefers profiles, isolated modules, and minimal upstream-file edits.
- [ ] 3. **Stable source identity** - represent text/video sources with stable IDs, hashes, source type, author/work metadata, and extraction-version provenance.
- [ ] 4. **Exact source spans** - add validated line/character source spans and deterministic exact-text resolution so LLM output is never authoritative for verbatim wording.

## MVP - Social text knowledge

- [ ] 5. **Social CLP profile** - define the social entities and relations for contexts, playbooks, frameworks, stages/states, cues, claims, actions, utterances, examples, and media links.
- [ ] 6. **Source-specific recommendation claims** - extract author/source-backed recommendations without collapsing them into universal rules.
- [ ] 7. **Source-specific interpretation claims** - represent observable cues separately from attributed interpretations so incompatible authors can coexist.
- [ ] 8. **Context scope semantics** - preserve explicit, inferred, unspecified, and explicitly general context without treating missing context as universal.
- [ ] 9. **Utterance classification** - distinguish verbatim utterances, suggested wording, and non-verbatim action instructions while preserving exact source text where applicable.
- [ ] 10. **Recursive rule conditions** - represent nested AND/OR/NOT, current cue/state/context, prior occurrence, and count-based historical prerequisites.
- [ ] 11. **Readable rule applicability** - deterministically render the complete condition AST on the recommendation so applicability is understandable without graph backtracking.
- [ ] 12. **Book chapter metadata** - ingest chapters with stable work/author/chapter identity so separately processed chapters can later reconcile.
- [ ] 13. **Same-book reconciliation** - connect preview examples, refinements, expansions, and later chapter knowledge back to earlier entities.
- [ ] 14. **Hierarchical subflows** - preserve high-level framework nodes while attaching detailed decompositions through relations such as `DECOMPOSES_INTO`.
- [ ] 15. **Canonical lower-level actions** - normalize equivalent lower-level actions across claims/contexts without merging or deleting source-native recommendations.
- [ ] 16. **Example interactions** - model example conversations as ordered turns with exact source/timestamp evidence and links to demonstrated rules/actions.
- [ ] 17. **Git-trackable social output** - persist deterministic structured social knowledge/provenance in the existing file-backed wiki/state model with readable diffs.

## MVP - Shared media ingest

- [ ] 18. **Video source abstraction** - support remote URLs and user-owned local video paths with explicit temporary-vs-external ownership.
- [ ] 19. **Downloader command adapter** - invoke the user's configured existing video downloader for remote sources without implementing downloader logic in llmwiki.
- [ ] 20. **Transcript normalization** - normalize supplied/native/downloaded/Whisper transcript sources into one timestamped internal representation.
- [ ] 21. **Whisper fallback adapter** - invoke the user's configured Whisper command only when a usable transcript is unavailable.
- [ ] 22. **Media segment model** - represent logical timestamp ranges independently of physical clip files and support proposed/reviewed/rejected lifecycle states.
- [ ] 23. **Many-to-many media usage** - allow one media segment to support multiple knowledge nodes without single ownership or stored reference counts.
- [ ] 24. **Media analysis provenance** - record video hash, transcript hash, extractor/prompt/profile versions, and generated dependencies per analysis run.

## MVP - Automatic social video extraction

- [ ] 25. **Social video knowledge extraction** - extract source-backed social claims/examples from timestamped video/transcript input while keeping observations separate from interpretations.
- [ ] 26. **Automatic segment proposals** - propose useful ranges for spoken examples, delivery, body language, explanations, live interactions, and meaningful response sequences.
- [ ] 27. **Segment-to-knowledge linking** - attach proposed media segments to the rules, utterances, body-language knowledge, stages, or example interactions they demonstrate.
- [ ] 28. **Exact shared-segment reuse** - reuse an existing reviewed segment when multiple knowledge items point to the same source range instead of creating duplicate clip artifacts.

## MVP - Human video review

- [ ] 29. **LosslessCut project writer** - generate a LosslessCut review project containing the full source and all current proposed/reviewed segments with stable IDs and useful labels.
- [ ] 30. **LosslessCut review importer** - import edited projects and detect boundary changes, deleted segments, and newly added missed segments.
- [ ] 31. **Stable segment identity** - preserve segment IDs across ordinary timestamp corrections and explicitly handle ambiguous review-import mappings.

## MVP - Selective invalidation and reruns

- [ ] 32. **Media dependency index** - track `VideoSource -> Transcript -> AnalysisRun -> KnowledgeItems -> MediaSegments -> PhysicalClips`.
- [ ] 33. **Boundary-only invalidation** - treat small semantic-preserving timestamp edits as clip-regeneration changes without unnecessary LLM re-extraction.
- [ ] 34. **New-segment analysis** - analyze and classify a user-added missed segment without reprocessing the full video.
- [ ] 35. **Changed-segment reanalysis** - reanalyze only a materially changed segment and reconcile only affected knowledge.
- [ ] 36. **Transcript-change invalidation** - mark transcript-derived knowledge stale without automatically invalidating unchanged physical clips.
- [ ] 37. **Extractor-version invalidation** - identify artifacts produced by older prompt/extractor/profile versions and support targeted refresh.

## MVP - Clip materialization and storage

- [ ] 38. **FFmpeg clip materializer** - create physical clips only from reviewed segments using stream-copy or exact re-encode as appropriate behind one abstraction.
- [ ] 39. **Filesystem media store** - store clip binaries outside Git under a configurable root suitable for a cloud-synchronized folder.
- [ ] 40. **Clip hashing and verification** - hash finalized clips, persist storage metadata, and verify successful storage before temporary remote-source cleanup.
- [ ] 41. **Safe remote-source cleanup** - delete only temporary downloaded full videos after required derived assets are safely persisted and never delete user-owned local videos.

## MVP - Media lifecycle

- [ ] 42. **Usage cleanup on knowledge changes** - remove only affected media usages when knowledge nodes or segment links are deleted while preserving still-supported knowledge and media.
- [ ] 43. **Media GC dry run** - report truly unreferenced clips with source, timestamps, storage location, and deletion reason before any destructive cleanup.
- [ ] 44. **Media garbage collection** - delete only clips with zero live usages and preserve any clip still referenced by another knowledge item.
- [ ] 45. **Clip deduplication** - avoid rematerializing identical reviewed source ranges/content and reuse the existing physical artifact where safe.

## MVP - End-to-end user workflow

- [ ] 46. **Remote video end-to-end flow** - ingest a URL through downloader, transcript resolution, automatic extraction, review, selective rerun, clip storage, cleanup, and Git-visible manifests.
- [ ] 47. **Local video end-to-end flow** - ingest an existing local file through the same pipeline while skipping download and guaranteeing the original file is never deleted.
- [ ] 48. **Representative book workflow** - prove chapter preview, high-level framework, nested subflow, exact wording, body-language interpretation, and historical conditions on a small real/synthetic book set.
- [ ] 49. **Second-source coexistence** - prove an alternative or unscoped second source can coexist without being forced into the first author's framework.
- [ ] 50. **LosslessCut correction scenario** - prove timestamp adjustment, deletion, missed-segment addition, and selective reprocessing in one review round trip.
- [ ] 51. **Shared-clip deletion scenario** - prove deleting one knowledge usage does not delete a clip still used by another node and GC removes it only after the final usage disappears.
- [ ] 52. **Deterministic Git review** - ensure structured extraction/reconciliation/media changes produce stable, understandable diffs and never auto-commit.
- [ ] 53. **MVP CLI documentation** - document social text ingest, remote/local video ingest, downloader/Whisper/LosslessCut/media-store configuration, selective refresh, materialization, status, and GC workflows.

## Post-MVP - Software knowledge graph

- [ ] 54. **Software knowledge profile** - add a separate software project/profile for Python, C++, Django/FastAPI, databases, software design, architecture, system design, code examples, practices, pitfalls, and tradeoffs.
- [ ] 55. **First-class code examples** - preserve source code examples as source-backed entities with language/framework metadata and links to concepts/practices/pitfalls.
- [ ] 56. **Technical video repository links** - associate video segments with linked Git repositories, commit SHAs, files, and line ranges and prefer repository code over pixel reconstruction.
- [ ] 57. **Technical visual extraction** - add screenshot/frame extraction for code, diagrams, and UI examples only when linked source material is insufficient.

## Post-MVP - Broader growth

- [ ] 58. **Automatic book chapter splitting** - add robust whole-book chapter segmentation after chapter-by-chapter ingestion is proven.
- [ ] 59. **Advanced cross-source synthesis** - add richer agreement/disagreement views and confidence-aware normalization without deleting source-native claims.
- [ ] 60. **Optional query integrations** - evaluate MCP/agent integrations after the file-backed knowledge model and APIs are stable.
- [ ] 61. **Optional graph projection/UI** - evaluate Neo4j or richer graph visualization only as a derived projection, never as the canonical knowledge source unless the project direction changes explicitly.
