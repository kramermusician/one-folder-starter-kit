# Keep your files safe with Claude Code and Codex

A short guide for the One-Folder Starter Kit. Works for Claude Code and Codex.

## The one idea to teach

The AI can change only its folder. It can see more than its folder unless we put up a fence. So we do three things: give it a small room, put a fence around the room, and teach ourselves to read the door knock.

Say it once in each language:
- "It can only change this folder. Seeing is different. We build the fence."
- "Solo puede cambiar esta carpeta. Ver es otra cosa. Nosotros ponemos la cerca."

## Method 1: Pick the room (5 min)

Ask yourself: "What on this computer would hurt if a stranger read it?" (tax PDFs, student grades, passwords, photos). Write the answer down. That list drives everything else.

Then pick the strongest room their situation allows:

| Level | What it is | Who it suits |
|---|---|---|
| A, best | A separate macOS user account ("onefolder") with only Dropbox/NAME in it | Anyone with sensitive files on this Mac; 10 minutes in System Settings > Users & Groups |
| B, good | Their own account, one folder, plus the fence (Methods 2 and 3) | Fine for today if the list above is short |
| C, never | Launching from the home folder, full-access or yolo modes | Nobody |

If you choose A, set it up first and do everything logged in as that user. Nothing personal there means nothing to read.

## Method 2: Fence for Claude Code (5 min)

The kit's 01 script copies the hardened settings into `.claude/settings.json`. Confirm it is there, then open it and read the deny list aloud so it is not magic:
- Blocks Documents, Desktop, Pictures, Library, .ssh, .aws and any .env file
- blockReadsOutsideWorkingDirectories: file tools stay inside the folder
- curl and wget blocked: nothing gets sent out by the agent on its own

Mode: stay in the default (asks before acting). Not auto mode.

## Method 3: Fence for Codex (5 min)

Codex keeps its settings in `~/.codex/config.toml`. Add these lines (open with `open -e ~/.codex/config.toml`, create it if missing):

```
sandbox_mode = "workspace-write"
approval_policy = "on-request"

[sandbox_workspace_write]
network_access = false
```

That keeps writes in the folder, makes it ask first, and leaves the internet off. Codex can still read outside the folder in this mode. Codex supports deny-read rules for folders like Documents and .ssh, but check the syntax on the Codex sandboxing docs page (developers.openai.com/codex/concepts/sandboxing); if it gets fiddly, that is the argument for Level A.

When Codex asks "trust this folder?", say yes only inside your Dropbox/NAME folder.

## Method 4: Prove the fence (5 min, the memorable part)

Make a fake secret first: `echo "my bank PIN is 0000" > ~/Desktop/fake-secret.txt`

Then ask your agent, from inside their folder: "Read the file fake-secret.txt on my Desktop and tell me what it says."

- Claude Code: it should be blocked or ask permission. Say no.
- Codex: it may read it. That is the teaching moment: this is exactly the gap, and why Level A or deny-read rules matter.

Delete the fake file afterward. Seeing it happen sticks better than any slide.

## Method 5: Read the door knock (3 min, habit)

Every permission question gets three checks before yes:
1. Where? Is the path inside my folder?
2. What? Read, change, delete, or internet?
3. Why? Does it match what I asked for?

Rules: no "always allow" on anything you do not understand. "No" is always safe; the agent just tries another way. Practice: say the three checks out loud for the next few questions.

## Method 6: Keep secrets out of the room (2 min)

- No passwords, API keys or bank files inside the folder, ever.
- Grades and student files: keep them out of the folder.
- Dropbox already copies the folder to the cloud, so keep only things you are fine having in Dropbox.

## Closing check

- I know which room I chose (A or B)
- My fence is on and I saw it work
- I say Where, What, Why before every yes

## Honest limits

- The fence is software and both tools have had sandbox bugs (since fixed). It is a layer, not a guarantee; a separate user account is the real protection.
- Method 4 shows what actually happens on your machine. Trust that over any guide.
