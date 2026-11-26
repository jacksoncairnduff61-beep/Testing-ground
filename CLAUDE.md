# CLAUDE.md — Thinking Repo Operating Rules

This repo is the user's **thinking space**: a place to test, brainstorm and explore
ideas, condensed into a map so past work can be referenced later. Ideas that become
real get graduated into their own repos.

**These rules apply to EVERY interaction in this repo, whatever the request looks like**:
casual chat, a coding task, a quick question, a brainstorm. If the user is thinking
about anything, this system is in play.

## 1. At the start of every session (always, before anything else substantive)

1. Read `MAP.md`. It's the index of every idea, past and present.
2. Read `INBOX.md` for unsorted captures, and check for open GitHub Issues labeled `inbox` (ideas captured from the phone). Offer to file them in, then close each one with a link to where it went.
3. Make sure you're working from the latest `main` (`git fetch origin main` and merge it
   into the session branch if it's behind). Then check for `claude/*` branches with
   commits not on `main` (`git log origin/main..origin/<branch> --oneline`). If any
   contain notes or map updates, merge them into `main` (see 3.4) and tell the user.
4. When the user raises a topic, check the map for related ideas and **mention the
   connections** ("this links to X from November, where you decided Y").

## 2. During the session

- Messy is fine. Capture first, structure later.
- New idea → create `ideas/<YYYY-MM>-<short-slug>/notes.md` from `ideas/_template/notes.md`.
- Existing idea → append to its `notes.md` under a dated heading. Never overwrite past thinking.
- Record decisions with their **why** in the idea's `## Decisions` section.
- Prototypes and test code go in that idea's folder (e.g. `ideas/<slug>/prototype/`).
- A quick thought that doesn't fit yet → one line in `INBOX.md`.
- Link related ideas with relative Markdown links, in both directions.

## 3. At the end of every session (or whenever the user says "condense", "wrap up", "save")

1. **Condense** — update each touched idea's `## Summary` and `## Open questions`.
2. **Update `MAP.md`** — add or refresh entries (summary, status, key decision, links, last touched).
3. **Triage `INBOX.md`** — file items into ideas, or leave them with a note.
4. **Commit and push, then merge into `main`.** The container is ephemeral: anything
   unpushed is lost. Commit message format: `think: <what changed>`.
   - Push the session branch as usual.
   - **Standing authorization from the user:** merge thinking-repo work into `main` and
     push `main` directly, no PR needed:
     `git fetch origin main && git checkout main && git merge --no-edit <session-branch> && git push origin main`,
     then switch back to the session branch. `main` is the single source of truth that
     every future session reads.
   - If the merge conflicts (usually in `MAP.md`), resolve it by keeping both sides'
     entries. Never drop map entries or notes. Never force-push `main`.
   - This authorization covers notes, map, inbox and prototype files in this repo only.
5. Tell the user what was saved, in 2–4 lines.

If the session ends abruptly or the user moves on, still do steps 2 and 4 before
finishing any turn where thinking content was added. Unsaved thinking is the worst outcome.

## 4. Commands the user may use (also respond to natural phrasings)

| User says | Do this |
|---|---|
| "capture: …" / "note this" | Add to `INBOX.md` or the relevant idea, commit, push |
| "condense" / "wrap up" | Run section 3 |
| "review the map" / "what have I got" | Summarize `MAP.md` by status; point out connections, stale ideas, and ideas worth reviving |
| "connect" / "what relates to this" | Search `ideas/` and the map for related past work |
| "promote <idea>" | Set status to `promising`; draft `PROPOSAL.md` from `templates/PROPOSAL.md` |
| "graduate <idea>" | Run section 5 |
| "drop <idea>" | Set status to `dropped` and record why. **Never delete the folder.** |
| "triage inbox" | Sort `INBOX.md` items into ideas |

## 5. Graduating an idea to its own repo

1. A completed `ideas/<slug>/PROPOSAL.md` must exist (draft one with the user if not).
2. The user creates the empty GitHub repo (this session can only reach this repo).
   Then the new repo is added to a session (`add_repo`) and the folder's contents moved
   over, keeping git history if wanted (`git subtree split --prefix=ideas/<slug>`).
3. In this repo, add `graduated/<slug>.md` (pointer + link + date + one-line summary),
   set the map status to `graduated → <repo link>`. Keep the original folder as the
   historical record of how the idea started.

## 6. Statuses

`raw` → `exploring` → `promising` → `graduated` (or `dropped` / `parked` at any point)

## 7. Principles

- Nothing is deleted. Dead ends are labeled, not removed.
- Links beat folders. Connect ideas.
- Always capture the *why*.
- `MAP.md` stays scannable: at most ~6 lines per idea. Detail belongs in `notes.md`.
- Plain Markdown only.
