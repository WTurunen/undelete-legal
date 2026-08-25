---
title: Known limitations
---

# Known limitations

Every recovery tool has edges. These are Undelete's, stated plainly, so that you can
decide whether it fits before you rely on it rather than afterwards.

**What restore can and can't do — the honest version**

- **Restored issues get a new issue key.** Jira Cloud's API doesn't allow re-using keys.
  We label every restored issue and record the old→new mapping, and the restored issue
  carries an audit comment naming the original key.
- **Original created date and comment timestamps can't be re-applied.** Jira doesn't
  allow setting them. The original creation date, deletion date, and who deleted the
  issue are recorded in the audit comment; each restored comment is prefixed with its
  original author and timestamp.
- **Only deletions that happen *after* you install are recoverable.** We can't see into
  the past — Jira deletes are permanent and leave nothing behind. **Install before you
  need it.**
- **Reporter is restored when permissions allow.** Setting the reporter requires the
  Modify Reporter permission; without it, the restored issue's reporter falls back and
  the original reporter is noted in the audit comment.
- **Restored issues start at the workflow's initial status.** Jira doesn't allow setting
  the status when an issue is created, so a "Done" issue comes back at its workflow's
  first status (e.g. "To Do"). Everything else about the issue restores normally.
- **Attachment files come back when attachment capture is on.** Attachment capture ships
  OFF — turn it on in Settings before you need it. When on, the app keeps a copy of each
  attachment's file (per-file cap adjustable 1–50 MB, default 10 MB; monthly storage
  budget adjustable up to 5120 MB, default 200 MB) and re-uploads the files on restore,
  with inline image embeds in the description re-pointed at the restored files. Files
  over the cap, over the monthly budget, or uploaded while capture was off restore as
  metadata only — the restore report lists every affected file, and the CSV export
  gives a per-file reason, so nothing is silently lost.
- **A restore that carries attachment files runs from your browser tab — keep it open.**
  The files upload from the admin's browser in batches. If the tab closes mid-restore,
  the recreated issue exists but has no description, comments, or links yet — those land
  when the restore finishes. To finish it: open the app, click Restore on the same trash
  item, and use the Resume button on the "restore already in progress" notice. The app
  has no permission to delete Jira issues, so it cannot clean up a half-finished restore
  by itself.
- **Change history starts fresh.** The restored issue is a faithful copy of content, not
  of Jira's internal history stream.
- **Restoring a deleted file charges your monthly attachment budget again.** The file is
  uploaded back to the issue as a new attachment, and the app re-captures it like any other
  upload — so those megabytes count twice in total for the month. There is no refund path.
  The confirmation dialog names the file's size and states the cost before you click.
- **Two people clicking Restore on the same file at the same instant can produce two copies.**
  The app blocks the ordinary double-click, but Jira's storage offers no way to lock a row that
  carries an index, so two genuinely simultaneous clicks from different browsers can both get
  through. Delete the spare from the issue.
- **A crash mid-upload can leave a file attached with no record of it.** If the app dies in the
  instant between Jira accepting the upload and the app writing that down, a retry uploads a
  second copy. It is one write wide and cannot be closed on this platform. It is visible on the
  issue, not silent.
- **With capture off, the project excluded, or the file now over your per-file cap, a restored
  file is not protected again.** The restore still succeeds and the file comes back — it just
  will not be re-banked, so deleting it again is permanent. The confirmation dialog tells you
  which of the three applies before you commit to the restore.
- **The project and issue shown for a deleted file are from the moment it was deleted.** If the
  issue has since been moved to another project, the list still shows the old one — and so can
  the warning about whether a restored file will be protected again. Permission to restore is
  always checked against where the issue lives *now*, so the stale label can never grant access
  it should not; only the display and that advisory notice can be out of date.
- **A file's stored copy outlives the restore by up to the grace window.** Restoring puts the
  file back immediately; releasing the app's own copy is background work that runs on the
  nightly job. Nothing is billed for holding it.
- **Inline images in an issue's description are re-pointed at the restored file; images inside
  comments are not.** The app does not edit comments people wrote — an embed in a comment keeps
  pointing at the removed file, and the file itself is back on the issue either way.
- **If re-pointing a description embed fails, the file still comes back but the image stays
  broken, and restoring again will not retry it.** The app re-points the embed only on the
  restore that puts the file back; a second restore of the same file recognises it is already
  restored and stops there. It also skips the re-point deliberately if anyone edited the
  description in the meantime — overwriting someone's edit to fix an image is the worse trade.
  Re-inserting the image by hand is the fix.
