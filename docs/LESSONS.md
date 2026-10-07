# Lessons: where the agents failed, and how it was caught

AI agents fail in confident ways. Each lesson below came from a real mistake. The fix never went into a chat
message; it went into a rule, a skill step or a memory file, so the same mistake doesn't happen twice.

## 1. "Decided" was recorded as "done"

**What happened:** The social media records kept decisions ("chosen", "approved") and things actually done
on Instagram in the same rows. I could no longer tell what had really happened. The opposite also occurred:
steps I had done myself were never recorded, because nobody asked.

**Fix:** A system-wide rule: a decision is not a completed action. Records keep decisions and the real
state in separate tables. Anything in the outside world (an account, a post, a domain, a setting in a
dashboard) counts as done only after the human confirms it, and when work is handed to me, the agent asks
about its status at the next chance.

## 2. The browser check passed before the page had finished loading

**What happened:** Automated layout measurements ran before lazy-loaded images had rendered, so the check
reported a clean page that wasn't actually finished.

**Fix:** The definition-of-done script now scrolls through the whole page and waits for images before it
measures, at 360, 768 and 1280 px.

## 3. Testing a contact link opened real apps on my computer

**What happened:** Clicking `mailto:`, `tel:` and WhatsApp links during a test opened the mail client,
a calling app and WhatsApp Desktop on the machine.

**Fix:** Tests never click these links. They read and validate the `href` instead.

## 4. Long generated shell commands broke silently

**What happened:** While building a site, double backslashes in heredocs were written as single ones,
which broke a regex and Windows paths. Commands over roughly 10k characters failed with an unhelpful
"unexpected EOF" error, and none of their lines ran.

**Fix:** A size limit per command, forward slashes in paths, and a file-writing tool instead of the shell
whenever a backslash can't be avoided. Agents check what was written instead of assuming it worked.

## 5. An API token didn't have the access it seemed to have

**What happened:** A scoped cloud API token returned 401/403 errors in the middle of a release task.

**Fix:** The DevOps agent now uses the provider's interactive login, which the human approves in the
browser, and logs out when the task is done. Credentials are never written to any file.

## The pattern

1. The agent makes a mistake that looks like success.
2. An independent step (QA, a measurement, or the human) catches it.
3. The lesson is written into the agent's rules or memory, not just fixed once.
