# Installing and using gumee 0.2.0

gumee is a free desktop app. Models use your OpenRouter account and are billed by OpenRouter.
An installed app does not require Node, Python, Docker, Git, or a local model.

## Install

Use the artifacts actually listed on the repository's release page. The configured targets are:

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
the artifact's stated signing status, or build from source using [README.md](README.md).

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

Supported inputs are TXT, Markdown, CSV, JSON, PDF, DOCX, XLSX, PPTX, and JPEG/PNG/WebP/GIF/BMP/TIFF images. Scanned PDF pages and image text are read with bundled local English OCR; no separate key or language download is needed. Recognized text is sent to your selected model as document context. OCR can misread handwriting, names, and numbers and does not interpret photographs or diagrams. For long scans, ask for batches of up to 20 pages. HEIC images must first be exported as JPEG or PNG.

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

The Effort slider scales steps/time; it leaves the budget unchanged. These settings govern
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

## File and tool limits

- Office inputs provide extracted text/data. XLSX formulas use stored results; gumee does not
  recalculate them. Charts, images, slide notes, and visual layout are not fully extracted.
- Generated DOCX/XLSX/PPTX/PDF files are real files created from structured content. They do not
  preserve every feature of an arbitrary source template. Spreadsheet formula results need
  verification in a spreadsheet application.
- PDF text replacement supports a defined subset of text-based PDFs. It is not OCR, a general
  page-layout editor, or support for all fonts and encodings.
- Public HTTPS research does not sign into websites, click forms, or inspect pages visually.
- JavaScript calculations run with a 32 MB interpreter limit and a short execution deadline.
  There is no arbitrary shell, Python, package installation, or access to desktop apps.

## Data, updates, and removal

Settings → Advanced shows the local data directory. It holds conversations, copied inputs,
outputs, memory, skills, schedules, and checkpoints. Model requests go through OpenRouter;
enabled research tools contact websites; approved connector calls contact their servers.
There is no gumee cloud account or synchronization.

Deleting a task removes its copies and generated outputs inside gumee. Files exported elsewhere
are unaffected. Back up the data directory while the app is closed if you want to preserve local
history before an upgrade. Install updates manually; no automatic updater is included.

Before uninstalling, choose **Disconnect** to remove the stored OpenRouter key and delete
connectors to remove their stored tokens. Remove the application and, if desired, the data
directory to delete local history. Uninstalling does not revoke your provider's API keys.
