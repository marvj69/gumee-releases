# Installing and using gumee 0.3.0

gumee is a free desktop app. Models use your OpenRouter account and are billed by OpenRouter.
An installed app does not require Node, Python, Docker, Git, or a local model.

## Install

Version 0.3.0 is available for macOS Apple Silicon. Windows and Linux remain at 0.2.1; the 0.3.0 features below apply to the Mac release. Use the artifacts actually listed on each release page. The configured targets are:

| Platform | Artifact pattern |
| --- | --- |
| macOS Apple Silicon | `gumee-<version>-macos-arm64-unsigned.dmg` |
| Windows x64 | `gumee-<version>-win-x64.exe` |
| Linux x64 | `gumee-<version>-linux-x86_64.AppImage` |

On macOS, open the DMG and drag gumee into Applications. On Windows, run the per-user installer.
On Linux, mark the AppImage executable and launch it; systems without FUSE may need
`--appimage-extract-and-run`. The interface supports a minimum 900 × 650 window.

Use the release's `SHA256SUMS.txt` to check your download:

```sh
shasum -a 256 gumee-<version>-macos-arm64-unsigned.dmg
sha256sum gumee-<version>-linux-x86_64.AppImage
```

On Windows use `certutil -hashfile <installer-path> SHA256`. Compare the resulting checksum
with the entry for that exact artifact.

Builds without signing credentials are unsigned and may be blocked by platform security
checks. A local successful build does not imply Apple notarization or Windows signing. Read
the artifact's stated signing status, and see [release notes](RELEASE_NOTES.md) for tested platforms and remaining setup.

## Connect your model account

1. Create a key in [OpenRouter key settings](https://openrouter.ai/settings/keys).
2. Paste it into gumee, choose **Test**, and then **Save**.
3. Choose secure system storage or **This session only**. Session-only keys disappear when the
   app quits; reconnect before resuming work or expecting schedules to run.

The profile determines which OpenRouter model receives your instructions and the file/context
content the agent reads. Settings shows available models and catalog pricing. Erie and Huron
are editable configurations, not model names. Unavailable models prompt for an explicit choice
rather than switching silently.

## Run a task

Choose an example, or describe a task in the composer. **Attach** selects files. The folder
icon beside it selects a folder and copies supported files into the task: up to 100 files and
50 MB per batch, skipping hidden files, symbolic links, and unsupported formats. Attachments
are snapshots, not a live synchronized folder. Later changes to the original files are not
picked up automatically.

Supported inputs are TXT, Markdown, CSV, JSON, PDF, DOCX, XLSX, PPTX, and JPEG/PNG/WebP/GIF/BMP/TIFF images. Scanned PDF pages and image text are read with bundled local English OCR; no separate key or language download is needed. Recognized text is sent to your selected model as document context. OCR can misread handwriting, names, and numbers and does not interpret photographs or diagrams. For visual questions, the agent uses view_image to send PNG/JPEG/WebP/GIF pixels to your selected model through OpenRouter. Enable Document tools and choose a model that supports image input. Image viewing is limited to 25 MB and 40 megapixels, resized to a maximum 2048-pixel edge; animated GIFs use the first frame. Image pixels are retained in local conversation checkpoints for follow-ups. For long scans, ask for batches of up to 20 pages. HEIC images must first be exported as JPEG or PNG.

Press **Run** or **⌘/Ctrl+Enter**. Expand activity entries to inspect actual tool arguments and
results. The **Plan** panel shows agent-written steps, run ceilings, costs, checkpoint time,
queued instructions, and task memory. File creation opens the Outputs panel. **Preview** reads
content; **Save as…** exports a copy; **Reveal in folder** locates it. PDF and Office outputs
also have **Open** for the installed desktop app. Review the resulting file's appearance in
that app: extracted text is not a visual layout check.

## Longer work and recovery

Use **Pause** to save progress and release execution. **Resume** continues the same run from
its saved checkpoint, retaining recorded usage and outputs. **Stop** cancels the run. An
interrupted run after a crash or quit waits for an explicit **Resume**; it is not silently
replayed. A request in flight may need to be issued again, and its prior cost or external
result may be uncertain. gumee asks again before replaying an uncertain connected operation.

While work is active, type another instruction and choose **Queue**. It waits behind earlier
work in the same task. Paused/interrupted work must be resumed or cancelled before later
queued instructions proceed. Other tasks can run independently up to the concurrency limit.
Use **New task** for unrelated work and **Try a variation** to compare another approach on the
same source files. Task titles can be renamed; **⌘/Ctrl+K** focuses task search.

Settings → **Long-running tasks** controls:

| Setting | New-install default | Supported range |
| --- | --- | --- |
| Steps | 120 | 4–2,000 |
| Time | 60 minutes | 1–1,440 minutes |
| Budget | $5 per run | $0.01–$100 |
| Concurrent runs | 2 | 1–4 |

The Effort slider scales requests/time; it leaves the budget unchanged. These settings govern
new runs. A 24-hour setting is an allowed ceiling, not evidence of a 24-hour reliability test.
Provider-reported charges and estimates can differ. OpenRouter account/key limits provide the
provider-side spending control. Waiting for approval or manually paused work is not active
model execution. **Keep computer awake** prevents idle sleep during active work; it cannot
keep working through shutdown or guarantee execution with a closed laptop lid. Desktop
notifications depend on your operating system's permissions.

## Scheduled tasks

Open **Scheduled tasks** in the sidebar, then **New schedule**. Enter a name, instructions,
profile, and local next-run time. Choose an existing task to reuse its attached files and
context, or start a new task each time. Repeat once, hourly, daily, weekly, or with a custom
interval of at least 15 minutes.

**gumee must be open and the computer awake.** This is a local scheduler. When the app returns,
one overdue occurrence catches up; it does not replay a burst of missed intervals. Repeating
intervals are elapsed durations, so a daily interval can shift its local clock time across
daylight-saving changes. A pending previous occurrence prevents overlap. Failures show a
saved error for review. Pause, edit, delete, or **Run now** from the schedule list.

A scheduled task can still wait for approval when it reaches a connected operation or an
output overwrite. Scheduling is not blanket permission to bypass those decisions.

## Memory, skills, and connected tools

Task memory lives in **Plan → Task memory**. Review or edit it there. The memory tool can
recall notes across runs in that task; it is not global account memory. If the agent changes
notes while you edit them, gumee shows the newer version before your next save. Do not put
credentials in notes, skills, or standing instructions.

Settings → **Standing instructions** adds your preferences to new runs. **Skills** stores
reusable workflow guidance; enabled skills are included for the agent. These are instructions,
not installed executables or a plugin marketplace.

Settings → **MCP connectors** accepts a public HTTPS server using MCP Streamable HTTP. Add its
URL and optional bearer token, save, and **Test** to list its advertised tools. Tokens are
stored encrypted and are not returned to the interface. Enable the connector and the profile's
MCP tools to make it available. Each tool invocation shows the service, operation, and arguments
for **Allow** or **Decline**. OAuth login, local stdio servers, private-network endpoints, and a
preconfigured application catalog are not supported in this release.

## Added workflows in 0.3.0

Settings includes Google account setup, selected projects, a Python test panel, signed-update
setup, and optional remote work/shared projects. Each shows its actual configured state.

- Google: register a desktop OAuth client, enter its ID in Settings, choose only needed scopes,
  and link an account. Exact setup and supported operations (available in the source repository).
- Projects: choose a folder, refresh its bounded text index, search it, and assign new tasks to
  it. Task, project and global instructions are separate. Changes preserve source backups and
  require review; switching project access after a task has run is refused.
- Python: offline runtime is bundled in the Mac app. No Python installation, shell, pip or
  Docker is needed. NumPy, pandas, matplotlib and Pillow are pinned; 60 seconds/256 MiB WASM.
- Media: enable artifact/document tools, ask for the operation, and review the selected model,
  provider and maximum charge before paid image/transcription work. Unsupported pricing or
  codecs are blocked. See media workflows (available in the source repository).
- Remote: configure your own HTTPS worker and account token, explicitly select transfer files,
  review the job/budget, then submit. Local files/accounts are not silently uploaded or made
  available remotely. Shared projects use owner/editor/viewer roles and revision conflicts.

## File and tool limits

Saved-file reports distinguish creation, reopen, structure, calculation, rendering, model visual
review and source consistency. Expand the output's checks and view rendered pages. A valid file
without a source expectation is not certified as factually correct. Repairs replace the canonical
filename only after overwrite approval and retain reviewable versions.

XLSX common formulas are recalculated from saved cells, across sheets and ranges. Unsupported
functions/references/features block certification. DOCX/PDF use real tables; supported DOCX/PPTX
content renders into page images. Office preview fidelity is bounded and not identical to native
Word/PowerPoint. Exact-run Office text/cell edits preserve unrelated ZIP parts and refuse complex
features. Supported formats and limits (available in the source repository).

Source-only writing applies bounded deadline/year/recipient/commitment checks and preserves
unspecified years; this is not a universal fact checker. Public research fetches supporting pages
and records source URL/time/excerpts. Search failures remain explicit.

There is no native computer control, authenticated browser automation, unrestricted shell or
arbitrary dependency installation. JavaScript and Python execute within separate bounded runtimes.
HTML previews support standalone offline apps; they cannot call the Gumee bridge or network.

## Data, updates, and removal

Settings → Advanced shows the local data directory. It holds conversations, copied inputs,
outputs, memory, skills, schedules, and checkpoints. Model requests go through OpenRouter;
enabled research tools contact websites; approved connector calls contact their servers.
Optional remote transfers and result synchronization use only the worker you configure. Personal local use needs no remote account.

Deleting a task removes its copies and generated outputs inside gumee. Files exported elsewhere
are unaffected. Back up the data directory while the app is closed if you want to preserve local
history before an upgrade. Versioned migrations first create a consistent database backup. Signed updates are opt-in and verified before opening the system installer; release signing/feed setup is unavailable in this unsigned local build. See recovery and release setup (available in the source repository).

Before uninstalling, choose **Disconnect** to remove the stored OpenRouter key and delete
connectors to remove their stored tokens. Remove the application and, if desired, the data
directory to delete local history. Uninstalling does not revoke your provider's API keys.
