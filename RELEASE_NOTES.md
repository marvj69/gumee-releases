# gumee 0.3.0 — macOS Apple Silicon

- Saved-workbook recalculation catches the audit's wrong formulas and misleading cached totals.
- Native document tables, supported chart/image structures, saved-file renders, persistent checks,
  approved canonical replacements and version restore.
- Domain-constrained research with fetched source evidence and practical fallback search.
- Awaiting-input and partial-completion states, deterministic finish, selected fresh context,
  cumulative reservations across retries, media, summaries and bounded subtasks.
- Scoped Google OAuth/API adapters, selected project knowledge and reviewed file changes.
- Bundled isolated Python, offline interactive previews, provider image/transcription tools and
  local audio/video processing.
- Deployable opt-in remote worker/shared-project service, migration backups and verified signed
  updater mechanism. Live hosting, Google registration, Apple signing/notarization and update
  hosting require external setup; this Mac build is unsigned and not notarized.

The Mac package passed 317 unit/integration tests and 18 desktop journeys. Provider video
analysis remains unverified live. Windows and Linux downloads remain at version 0.2.1.

Google live account acceptance, hosted remote/multi-device acceptance and a signed update feed remain setup gates. Local video tools work; provider video analysis has not passed live acceptance. No computer control or authenticated browser automation is included.

Install using the DMG or ZIP below. Verify the download against SHA256SUMS.txt. Close Gumee and back up its data directory before upgrading; see [installation instructions](https://github.com/marvj69/gumee-releases/blob/main/INSTALL.md).


# gumee 0.2.1

- Attach JPEG, PNG, WebP, GIF, BMP, and TIFF images and read their text with local English OCR.
- Automatically recognize PDF pages without text, including mixed text/scanned documents, with page citations and OCR confidence.
- Force OCR for incomplete text layers and request page ranges for larger scans (20 scanned pages per call).
- Image and PDF previews use the same reader. English language data and OCR runtime ship with the app; recognition requires no separate service or download.
- OCR text can contain errors, particularly handwriting, names and numbers. This reads text in images, not general photographic or diagram understanding. HEIC requires conversion to JPEG or PNG.
- macOS and Windows installers remain unsigned; macOS is not notarized.

# gumee 0.2.0

This release turns the initial file-agent experiment into a durable local agent workbench.
It adds real execution and tool capabilities while retaining visible activity, source snapshots,
per-run outputs, explicit approvals, and the existing light/dark design.

## Added

- Pause and resume from SQLite checkpoints, explicit recovery after interruption, persisted
  queued instructions, and serialized follow-ups in a task. Independent tasks can run
  concurrently. Usage and elapsed execution limits survive recovery.
- Up to 2,000 configured steps and 24 hours per run; new-install defaults of 120 steps,
  60 minutes, $5 per run, and two concurrent runs. Metering includes retries, delegated model
  calls, and context summaries. Unknown pricing blocks budgeted model execution.
- Transient provider retries with visible backoff, context compaction with durable history,
  and a bounded read-only researcher for up to three sequential delegated subtasks sharing
  the parent run's limits.
- Local scheduled tasks: once or repeating, existing or new task context, pause/edit/delete,
  manual run, one missed-occurrence catch-up, no overlapping pending occurrence, and persisted
  failure details. Requires an open app and awake computer.
- Task search and status filters, progress/plan/checkpoint/usage readouts, a queue panel,
  desktop notifications, idle-sleep prevention, and expandable activity history for long tasks.
- Folder snapshot imports, up to 100 supported files and 50 MB per batch, with hidden files,
  symlinks, and unsupported types skipped.
- DOCX/XLSX/PPTX text/data extraction and real structured DOCX/XLSX/PPTX/PDF/HTML artifact
  creation, alongside existing CSV/text/Markdown tools and supported PDF edits. PDF and Office
  artifacts can open in their installed application.
- Public HTTPS search and page retrieval with source URLs and private-network protections.
- Isolated QuickJS WebAssembly calculations: 32 MB memory, short execution deadline, bounded
  input/output, and no host filesystem, network, processes, packages, or Python.
- Persistent memory scoped to each task, editable notes with conflict detection, reusable
  skills, standing instructions, and expanded per-profile tool choices.
- Public HTTPS MCP Streamable HTTP connectors with optional encrypted bearer tokens,
  tool discovery/testing, explicit approval for every call, and fresh approval before replaying
  an external operation whose completion is uncertain.

## Verification

The automated suites cover checkpoint/restart recovery, cancellation, same-task queue ordering,
concurrency, budgets, retries, context compaction, delegated usage, schedules, connector
transport, web boundaries, isolated code, folder snapshots, and Office/PDF files. Electron
journeys exercise the actual app, including light/dark themes, 900 × 650 layout, 200% zoom,
pause → restart → resume → queued completion, schedule management, skills, memory, connector
configuration, and native folder selection.

Deterministic automated tests use local/scripted providers and make no paid model calls.
`scripts/live-harness-smoke.mjs` is a separate opt-in real-provider check; its completed run
records and cost report are saved to `docs/verification/live-harness.json` when executed.
Treat the actual report and final release check output as the evidence, not the presence of
the script. No 24-hour real-provider soak or every-website/every-connector compatibility claim
has been made. See [the acceptance matrix](docs/AGENT-HARNESS.md) for precise coverage.

## Known limits and remaining work

This version does **not** match every capability of Claude Cowork or ChatGPT Work. Outstanding
areas include cloud work with the computer off, native computer use and authenticated visual
browser control, OAuth connector catalogs, image understanding/OCR, arbitrary Python or shell
workloads, full-fidelity Office/template editing, enterprise synchronization/collaboration,
and a signed automatic update channel.

Web research handles public HTTPS text; it does not interact with sites or reuse browser logins.
MCP support excludes OAuth, local stdio servers, and private-network endpoints. Office previews
are extracted content, not rendered visual review; spreadsheet formulas are not recalculated.
Scanned PDFs remain unsupported. Application cost reservations are not a provider-enforced
billing cap. Configure a limit on the OpenRouter key/account when that guarantee is needed.

## Distribution and upgrade

The configured targets remain macOS Apple Silicon DMG, Windows x64 NSIS, and Linux x64 AppImage.
Only artifacts actually listed on a release have been built/published for that release. A
successful local macOS test does not verify the other platforms. Builds remain unsigned unless
the release explicitly states otherwise; no automatic updater is included.

The app preserves existing local tasks and checkpoints through schema migrations. Back up its
data directory with the app closed before replacing an installation. Source files and exported
copies are not modified by the upgrade. See [INSTALL.md](INSTALL.md) for operation and recovery.

## Previous release

0.1.0 introduced the initial Deep Agents desktop app, OpenRouter connection, two editable
profiles, bundled examples, document/CSV tools, per-run outputs and usage records, variations,
comparisons, and the Lake Superior visual system. The capabilities and limits above describe
0.2.0 and supersede the earlier release's restrictions on scheduling, delegation, and Office
files. [DESIGN.md](DESIGN.md) remains the historical design reference.
