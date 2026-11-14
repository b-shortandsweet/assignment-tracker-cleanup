# Weekly cleanup for the Assignment Tracker

A Claude skill for parents using the Heart at Home **Assignment Tracker** in Notion with the
**Gradelink to Notion** Chrome extension.

Once the extension has synced your week, Claude tidies the tracker for you. It archives
finished and in-class work, splits cards that hold more than one assignment, renames vague
titles, and fixes subjects. It shows you every change first and changes nothing until you
say yes.

**[Download assignment-tracker-cleanup.zip](https://github.com/b-shortandsweet/assignment-tracker-cleanup/releases/latest/download/assignment-tracker-cleanup.zip)**

## What you need

- A paid Claude plan (Pro or higher). Custom skills don't work on the free plan.
- Your Assignment Tracker in Notion, already set up with the extension
  ([setup guide](https://b-shortandsweet.github.io/gradelink-notion-guide/)).

## Set it up (once)

1. **Download the skill.** Click the download link above. Keep the zip as it is; don't
   unzip it.
2. **Turn on code execution.** In Claude, open **Settings → Capabilities** and switch on
   **Code execution and file creation**. (Newer versions of the app may call it
   **Create and edit files**.)
3. **Connect Notion.** In Claude, open **Settings → Connectors**, connect **Notion**, and
   give it access to your Assignment Tracker page.
4. **Add the skill.** Go to **Customize → Skills**, click **+**, then **Create skill**, then
   **Upload a skill**, and choose the zip you downloaded.

## Each week

After you run the sync, start a new chat in Claude and type:

> Clean up my assignment tracker

The first time, it asks a few questions (your kids' names, what day your school week
starts, which days are school days and which are home days) and saves your answers on a
page in your tracker called **Assignment Tracker Cleanup Settings**. After that it
remembers.

If it gets something wrong, tell it. It will ask whether to remember that for next week.

## What's in here

`assignment-tracker-cleanup/SKILL.md` is the skill itself: the instructions Claude follows.
The zip in the download is that folder, packaged for Claude.
