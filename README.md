# PaperSync

PaperSync is a macOS menu bar app that records meetings and calls (Google Meet, Zoom, Microsoft Teams, Jitsi, Slack and others), transcribes them on your Mac, labels who said what and remembers voices, then writes a summary and a task list. Optionally, everything is exported to Notion, saved as Markdown notes for Obsidian, and tasks go to Reminders or Things.

- **Speech recognition runs locally.** Whisper transcribes on your Mac. Audio never leaves it.
- **Only text goes to AI.** To write a summary, PaperSync sends the transcript text and calendar event details to the AI option you choose. With a local model, nothing leaves your Mac at all.
- **It runs on its own.** It offers to record calendar meetings and calls, processes recordings after they stop, and updates itself.

## Download

**[Latest version](https://github.com/dimapaperpaperio/papersync-releases/releases/latest)** — the `.dmg` file under Assets.

Requirements:
- A Mac with Apple Silicon (M1 or later) and macOS 14.4 or later. Live transcript during a meeting requires macOS 26 or later.
- An internet connection for the first processing run: PaperSync downloads speech recognition models (about 1.3 GB) and spends a few minutes preparing them. After that, no further downloads are needed.

Updates install automatically: once a day the app checks for a new version and installs it. To check manually, open Settings… → Updates → Check Now….

## Install and first run

1. Open the `.dmg` and drag PaperSync to Applications.
2. Launch PaperSync from Applications. A "P" icon appears in the menu bar. The app turns on Launch at login by itself (you can switch it off in the menu).
3. The setup assistant opens. You can complete the steps in any order; each shows its status. To open it again later, go to Settings… → General → Open Assistant….

### What the setup assistant asks for

- **Your name** — used to tell your tasks apart from everyone else's.
- **Calendars** — shows which calendar accounts are connected (iCloud, Google, Exchange…). For Google and Outlook there is an Add Account… button. Which calendars trigger "Record this meeting?" is set in Settings… → Recording.
- **Permissions:**
  - **Microphone** — your voice.
  - **System audio recording** — the other participants' voices. Without it, only you will be in the recording.
  - **Calendar** — for the "Record this meeting?" prompt and meeting prep. Manual recording works without it.
  - **Notifications** — silence warnings, auto-stop, meeting prep, the weekly digest.
  - **Speech Recognition** — only for the live transcript (macOS 26+).

  If a permission is denied, the assistant shows a button that opens the right section of System Settings. Check Sound records 3 seconds from the microphone and system audio and shows the levels.
- **AI for summaries** (recommended; without it you get transcripts only):

  | Option | What you need |
  |---|---|
  | Claude (subscription) — default | The Claude command-line tool or just the Claude desktop app (PaperSync uses its built-in Claude Code), and a sign-in: Sign In… in the assistant, or `claude /login` in Terminal |
  | ChatGPT (subscription) | The Codex command-line tool: `brew install codex`, then `codex login` |
  | Gemini (Google subscription) | Antigravity CLI (`agy`) and a sign-in to your Google account |
  | Anthropic API, OpenAI API, Gemini API, OpenRouter, DeepSeek, Qwen, Kimi, GLM, MiniMax | An API key for that service; you pay per request at its rates |
  | Ollama, LM Studio (on this Mac) | A running local server; data stays on your Mac |
  | Custom address | Any OpenAI-compatible service: DeepSeek, Mistral, Groq, Azure OpenAI, a company gateway |

  If you already have a subscription, use it — there is nothing extra to pay. Each option shows where your data goes and whether it may be used for model training. API keys are stored in the macOS Keychain. You can switch options in Settings… → AI.
- **Notion** (optional): create an internal integration at notion.so/profile/integrations, connect it to a page (page menu → Connections), paste the token into the assistant and click Create automatically — Meetings and Tasks databases will appear. Then click Check Connection and turn on Settings… → Integrations → Export meetings to Notion.

## Features

### Recording

- **"Record this meeting?"** When a calendar meeting with a call link starts and another app is using the microphone, PaperSync asks whether to record it. Recording starts only after you click Record, and never before the scheduled start, even if the call app opened the microphone early.
- **Calls outside the calendar.** If Zoom, Teams, Slack, Telegram (including Telegram Desktop), FaceTime, WhatsApp, Discord, Skype or a browser (including Arc, Dia and Firefox) has been using the microphone for 20 seconds and there is no calendar meeting, "Record this call?" appears. In the 5 minutes before a meeting with a call link this prompt doesn't appear: the calendar recording takes over.
- **Manually** — menu → Start Recording / Stop, or F5.
- **The start is never lost.** While the prompt is on screen, audio is already being written to a temporary folder. If you click Record, the recording includes everything from the moment the prompt appeared. If you decline, the temporary audio is deleted.
- **Microphone choice** — Settings… → Recording → Microphone. For example, use the MacBook microphone while AirPods are connected: the headphones stay in normal mode and the other participants don't get quieter.
- **Pause** (F6) — nothing is recorded while paused.
- **Marks** (F7) — mark an important moment. The summary gets a section listing marked moments.
- **End of meeting.** A calendar recording stops exactly at the scheduled end. A minute before, "Meeting is ending" offers "+10 min" or "Stop Now". You can extend as many times as you need, also from the menu (Extend by 10 Minutes); the menu shows when the recording will stop.
- **Back-to-back meetings.** If the next meeting with a call link is already under way when the recording stops, PaperSync switches to it. If it starts before the current one ends, you choose "+10 min", "Stop" or "Record the Next One".
- **Manual recordings** are not stopped on schedule. When the meeting ends, PaperSync asks whether to stop ("The meeting has ended").
- **Auto-stop.** A recording started from "Record this meeting?" or "Record this call?" also stops early by itself when nothing is using the microphone and there has been no sound for 2 minutes. For a recording you started manually, PaperSync asks "Stop recording?" after 2 minutes without a call or sound, and stops it a minute later if you don't answer.
- **Silent recording.** If the microphone or system audio is silent after 30 seconds, you get a notification — usually a sign that a permission is missing.
- **Several people at the mic.** If other people speak into your Mac's microphone (a meeting room, a colleague next to you, a conversation without a call), check Several people at the mic in the prompt or the recording menu. You stay "Me"; the others become separate speakers marked "at the mic". PaperSync can also detect this on its own: Settings… → Recording → Detect several people at the mic.
- **Notes during a call** (F10) — a floating Notes window for short timestamped notes, saved immediately. The summary expands each note from the transcript and gives it extra weight in the summary and tasks.

### Transcription

- **During the meeting.** PaperSync transcribes in the background, so after you stop only the last few minutes remain. Background transcription pauses on battery and under heavy load.
- **Live transcript** (macOS 26+, F8) — a small always-on-top window with "Me" and "Others" lines in near real time, in your main meeting language.
- **Meeting languages** — any of the roughly one hundred languages speech recognition supports: Settings… → Recording → Languages. Choose Single language or Auto among selected; the first language is the main one.
- **Interface in 10 languages** — English, Español, Deutsch, Français, Português (Brasil), Русский, العربية, 简体中文, 日本語, हिन्दी. The system language is used by default; change it in Settings… → General. Summaries, tasks, follow-ups and digests are written in the interface language.
- **Dictionary** — names and terms Whisper should recognize (Settings… → Recording → Dictionary…). Put the important ones higher.
- **Learning from edits.** If you replace a misrecognized word with a term in the transcript window, PaperSync offers to add it to the dictionary. Nothing is added without your consent.

### Speakers

- **Naming participants.** Speakers start out numbered. In Recent Meetings → a meeting → Name Participants…, listen to short clips and type names; suggestions come from calendar attendees and known voices. The transcript, summary, tasks and Notion page are updated without running the AI again. If someone is marked "at the mic" but no one was next to you, the microphone probably picked up call audio from your speakers; headphones help.
- **Voice memory.** With "remember voice" checked, the name is filled in automatically at future meetings. The list of voices is in Settings… → Voices.
- **Voices in old meetings.** Settings… → Voices → Find Voices in Old Meetings… re-analyzes meetings processed before voiceprints existed (the transcript doesn't change) and remembers named participants. For a single old meeting, use Capture Voiceprints in the naming window.

### Meetings library

- **Meetings… window** (menu → Meetings…, or Recent Meetings → All Meetings…): meetings grouped by date — today, yesterday, this week, by month. Search covers full transcripts, summaries, tasks and notes ("exact phrases" too); each result shows snippets, and clicking one opens the transcript at that line. Filters: My Tasks, Confidential, No Summary.
- Each meeting has tabs for the **summary**, **tasks** with checkboxes (synced with My Tasks, Notion and Reminders), **notes** (click a time to play; edit them and choose Make Summary Again), the **transcript**, and **participants**.
- **Talk analytics** in the Participants tab: talk share, longest monologue, takeovers and yields, and your questions. Computed on your Mac.
- **Meeting series.** Recurring meetings are linked by their calendar series or by title and participants. The summary gets a "What changed since last time" section and checks the previous meeting's open tasks; the meeting window lists previous and upcoming meetings of the series.
- **People… window** (menu → People…): who you meet with, how much each person talks, your share of speaking time, weekly trends of your talk share and takeovers, and promises both ways.
- **Built-in player:** Space to pause, ←/→ to skip 15 seconds, speed up to 2×, Skip Pauses, Others Only, lines and marks on the timeline, the current line highlighted. The position is remembered for each meeting.
- **Transcript editing** — click a line to play from there, ⌘F to search, double-click to edit. After editing you can rebuild the summary.

### Results

- **Summary and tasks** are produced automatically after each meeting. Progress is shown in the menu under Processing.
- **My Tasks** — open tasks from all meetings, overdue first. All Tasks… opens a window with search and filters. With Notion enabled, the Done status syncs both ways.
- **Follow-up** — Write Follow-Up… drafts an email to attendees: what was decided, who does what, open questions. Open in Mail only opens the draft; you send it.
- **Weekly digest** — every Friday at 17:00: highlights, decisions, your tasks, tasks you're waiting on from others, open questions. Open it any time from menu → Weekly Digest…. Day and time are set in Settings… → General.
- **Meeting prep** — 5 minutes before a calendar meeting, a notification with past meetings with the same people (preferring the same series) and open tasks.
- **Ask About Meetings…** (F9) — ask about past meetings: "what did we decide about the banner?", "what did I promise Alex?". Period: 7, 30 or 90 days, or all time. Answers cite the meeting date and title.
- **Spotlight** — processed meetings can be found with ⌘Space. The index stays on this Mac; turn it off in Settings… → General.

### Markdown notes for Obsidian

- For each processed meeting PaperSync writes a Markdown file to the Notes folder inside the recordings folder, arranged by year and month: the summary and tasks in Obsidian Tasks format. Notes update themselves.
- Open Note opens the note in Obsidian if the folder is inside your vault. Change the folder or turn notes off in Settings… → Storage.

### Integrations

Settings… → Integrations. Everything here is off by default.

- **Tasks to Apple Reminders or Things.** Choose a destination to start. Reminders get a due date and a link back to the meeting, and Done syncs both ways with My Tasks; Things is one-way. You can also send tasks manually from My Tasks or the meeting window.
- **Summary to Telegram, Slack or a webhook** (Send summary after the meeting): either ask with a notification after each meeting or send right away. Only the title, date, participants, summary and tasks are sent — never the transcript, audio, your notes or confidential meetings.

### Access from Claude Desktop and Claude Code (MCP)

PaperSync can let AI assistants read your meetings over MCP: search and list meetings, open one (summary, tasks, transcript), list tasks. It is read-only and off by default. Turn it on in Settings… → Privacy → Access for AI Assistants (MCP); setup for Claude Desktop and Claude Code is shown there, with an Add to Claude Code button. Confidential meetings are not served unless you allow them.

### Notion

- Each summary is exported with the title, date, attendees, outcome, summary sections, tasks and open questions. Exporting again does not create duplicates.
- Meetings are grouped by week and month and shown on a calendar; tasks have My Open and Overdue views. Page titles start with the date, and the icon shows the meeting type.
- If Notion was set up in an earlier version: Settings… → Integrations → Update Structure, then optionally Export Meetings Again….

### Storage

- **Compression** — Settings… → Recording → Compress recordings after processing: about 6 times less space, no effect on transcription. Compress Old Recordings handles existing ones.
- **Audio cleanup** — Settings… → Recording → Keep audio: forever, or 1, 7, 30 or 90 days. Older audio goes to the Trash; transcript, summary and tasks stay. After cleanup the meeting can't be transcribed again.
- **Transcript and summary cleanup** after 30, 90 or 365 days — Settings… → Storage.

## Keyboard shortcuts

They work in any app.

| Key | Action |
|---|---|
| F5 | Start recording; while recording, stop (press again within 2.5 s). On "Record this meeting?" — Record; on "The meeting has ended" — Stop |
| F6 | Pause / resume |
| F7 | Mark a moment |
| F8 | Show or hide the live transcript |
| F9 | Ask About Meetings… |
| F10 | Open the Notes window |
| ⌃⌥N | Don't Record or Continue on a prompt |

Change shortcuts in Settings… → General. On a MacBook, press the F-keys together with fn, or turn on "Use F1, F2, etc. keys as standard function keys" in System Settings → Keyboard.

## Where your data lives

- **Meeting recordings** — `~/PaperSync/` (outside iCloud; change it in Settings… → Storage), one folder per meeting: audio, transcript, summary, tasks, notes. Open it from menu → All Recordings in Finder. Files are readable only by your macOS user. PaperSync warns you if the recordings or notes folder is synced (iCloud, Dropbox, Google Drive, OneDrive, Obsidian Sync).
- **Markdown notes and weekly digests** — in their own folders inside the recordings folder.
- **Dictionary, voices, models, search index** — `~/Library/Application Support/PaperSync/`.
- **API keys and the encryption key** — the macOS Keychain.

## Privacy

- **Stays on your Mac:** audio, transcription, speaker separation, voiceprints, live transcript, search index, talk analytics, meeting prep.
- **Goes to the AI option you chose:** for a summary — the transcript text and calendar event details; for Ask About Meetings… — summaries, tasks and parts of transcripts from the selected period. Audio is never sent. With Ollama or LM Studio, nothing leaves the Mac.
- **Goes to Notion** (if enabled): title, date, attendees, outcome, summary sections and tasks. Transcripts, audio and voices are not exported.
- **Goes to Telegram, Slack or your webhook** (if enabled): title, date, participants, summary and tasks. Never the transcript, audio, notes or confidential meetings. Tasks sent to Reminders or Things go to those apps on your Mac.
- **Goes to an AI assistant over MCP** (if enabled): whatever the assistant reads — summaries, tasks, transcripts — then goes to that assistant's AI service.

Additional protection, mostly in Settings… → Privacy:

- **Confidential meetings** — a checkbox in the recording prompt or the menu, or set automatically by words in the event title (NDA, 1:1, confidential…). Such meetings are not sent to cloud AI, Notion, the summary integrations, MCP (unless you allow it), Spotlight, Ask About Meetings… or the digest, and get no Markdown note by default. A summary is still possible with a local model.
- **Masking before AI** — card numbers, IBANs, phone numbers, email addresses, keys and passwords are replaced with a placeholder before text is sent.
- **Encryption of recordings and voiceprints** — audio after processing and voiceprints are encrypted with a key kept in the Keychain. Save Recovery Key… lets you move them to another Mac. Full encryption: meeting texts also encrypts transcripts, summaries, tasks, follow-ups and digests; Spotlight and Markdown notes are then off.
- **Touch ID or password** — optional protection for transcripts, summaries, the Meetings window, Ask About Meetings…, voices, tasks, the digest and follow-ups.
- **Hiding windows from screen sharing** — PaperSync windows are hidden when you share your screen. Some apps on newer macOS versions may still show them, so check before an important call.
- **Recording reminder** — the "Record this meeting?" prompt reminds you to tell attendees they are being recorded.

## FAQ

**The other participants aren't in the recording.**
Grant "Screen & System Audio Recording" in System Settings → Privacy & Security. For your own voice, check Microphone. Check Sound in the setup assistant shows what PaperSync can hear. If you use AirPods, try choosing the MacBook microphone in Settings… → Recording.

**The recording stopped at the end of the meeting, but we kept talking.**
Calendar recordings stop at the scheduled end: click "+10 min" on "Meeting is ending" or use Extend by 10 Minutes in the menu. A recording you start yourself is never stopped on schedule.

**A speaker marked "at the mic" appeared, but I was alone.**
The microphone picked up call audio from your speakers. Headphones help.

**The first processing run is slow.**
The first time, PaperSync downloads about 1.3 GB of models and takes a few minutes to prepare them. This needs internet and happens only once.

**No summary, error "Failed to authenticate".**
Your Claude sign-in has expired (or, with only the Claude desktop app, you haven't signed in yet): click Sign In… in the setup assistant or run `claude /login`, then menu → Processing → Retry Processing. For ChatGPT, run `codex login`. For API keys, click Check in Settings… → AI.

**Can I use PaperSync without sending anything to the cloud?**
Yes. Choose Ollama or LM Studio in Settings… → AI and leave Notion, the summary integrations and MCP off. Transcription is always local.

**How do I update?**
Updates install automatically. To check now: Settings… → Updates → Check Now…. You can also download the new `.dmg` from the [releases page](https://github.com/dimapaperpaperio/papersync-releases/releases/latest) and replace the app in Applications.

**The app quit during a recording.**
On the next launch, the Processing menu offers to process interrupted recordings.

**How do I report a problem?**
Menu → Report a Problem…. PaperSync builds an archive with the version, settings (without keys), log and crash reports — no audio, text or names. You see the list of files before saving. Attach the archive to an [issue](https://github.com/dimapaperpaperio/papersync-releases/issues) in this repository.

Detailed help is built into the app: menu → Help.
