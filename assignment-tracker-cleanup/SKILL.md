---
name: assignment-tracker-cleanup
description: >-
  Tidies a week of homeschool assignment cards in the Heart at Home Assignment Tracker
  (Notion) after the Gradelink to Notion extension syncs. Archives finished and in-class
  work, asks about old unfinished cards, splits cards holding more than one assignment,
  renames vague titles, fixes subjects filed as Other, and records due dates teachers wrote
  in the description. Proposes every change and writes nothing until the parent says yes.
  Use when the parent says "clean up my assignment tracker", "tidy this week's assignments",
  "weekly cleanup", or similar. Needs the Notion connector.
---

# Assignment Tracker cleanup

The Gradelink to Notion extension copies assignments into the parent's Notion tracker
exactly as the teachers typed them. This skill is the tidy-up afterwards: it reads the
cards, proposes changes in plain groups, and makes only the changes the parent approves.

**Never change anything in Notion before the parent says yes to that group.**

## What you need

- The **Notion connector**, with access to the parent's tracker. If a Notion call fails or
  the tracker can't be found, tell her to open Claude's connector settings, connect Notion,
  and give it access to her Assignment Tracker page. Do not carry on as if it worked.
- Today's date and weekday. Work the weekday out carefully from the date (use the code tool
  if you have it). A wrong weekday puts every card in the wrong week.

## Reading the tracker: use the saved view

Read cards by running the tracker's saved **Assignments** view with the Notion query tool
in **view mode** (`mode: "view"`, the view's URL). View mode works on every Notion plan with
no limit. Do **not** use SQL or rows mode to list cards: on cheaper Notion plans those have a
small allowance that runs out.

The Assignments view already hides archived cards. Page through all of it: while the result
says `has_more`, call again with `start_cursor` set to `next_cursor`. Ignore rows whose
Note Type is Resource.

## Step 1: Settings (first run, then every run)

Settings live on a Notion page called **Assignment Tracker Cleanup Settings**, inside the
parent's Assignment Tracker page. Search for it first.

**If it exists:** fetch it and use it. Read its Corrections list before doing anything else;
those are the parent's own rules and they win over this file.

**If it doesn't (first run):** set it up with her, one question at a time.

1. Find her tracker. Search Notion for "Assignment Tracker" and confirm which one is hers.
   Fetch it, find the Assignments List database, fetch that, and note its data source URL and
   the URL of the view named **Assignments**. If there's no Assignments view, ask her which
   view shows her unarchived assignments.
2. Her children: read the Person options from the database and confirm them with her.
3. Her week: "What day does your school week start?" (Most families: Monday.)
4. Class days vs home days: "On which days is work done at school, and which at home?"
   Cards **due** on a class day are in-class work and get archived; cards due on a home day
   stay. If she says every day is a home day, skip the class-day archive step entirely.
5. Show her the settings, and on her yes create the page (parent: her Assignment Tracker
   page) with this content:

```
Tracker: <Assignments List database link>
Assignments view: <view URL>
Data source: <collection:// URL>
Children: <names>
Week starts: <day>
Class days (archive work due these days): <days>
Home days (keep work due these days): <days>

## Corrections
(Rules from the weekly cleanup, newest last. Claude reads these every run.)
```

## Step 2: Establish the week

Say which week you're working on, as dates, before anything else. This week runs from the
most recent week-start day (today counts) for seven days.

## Step 3: Pull the cards

Run the Assignments view (all pages). Keep two sets:

- **All open cards**: everything the view returns.
- **This week's cards**: cards whose Assigned Date **or** Due Date falls in this week.
  Use either date, never Assigned Date alone: some teachers assign the week's homework
  before the week starts, and that card would be missed.

Then fetch each card in this week's set, and any card filed under Other, to read its body.
The teacher's description lives in the page body (and in the Description property, if she
turned that on in the extension).

**Cards with no Assignment ID** are the parent's own additions (piano practice, extra
reading). They can be archived when finished, but never rename or split them: the extension
recognises them by name, and a new name makes it write a second copy next week.

## Step 4: Build the proposals

### A. Finished cards → archive
Any open card whose Status is in the tracker's Complete group (Done, plus anything she added
there, like Incomplete). Any week.

### B. Leftovers from earlier weeks → ask
Open cards due before this week (Assigned Date when Due Date is empty) whose Status is not
complete, plus any card with neither date ("no dates"). List each with child, title, due date
and status, and let her decide card by card: keep or archive. Not a leftover: a card whose
Desc. Due Date is this week or later (a long project still in progress).

### C. In-class work → archive
This week's cards **due** on one of her class days. Due Date decides, not Assigned Date. Skip
her own additions (no Assignment ID). Pull a card out and ask about it on its own when:

- it's due on a day that's neither a class day nor a home day (a weekend, say)
- its body reads as home work ("complete at home", "parent sign off") but it's due on a
  class day, or reads as in-class work but is due on a home day
- its Due Date is empty
- she has told you this week is unusual (a day off school, a break week). Then ask which
  days to treat as home days this time.

### D. Split cards that hold several assignments
The body decides, never the title. An "&" in a title is not a reason to split.

- Several bold or underlined headings, each with its own goal, materials or instructions →
  one card per heading. Each new card takes its own Materials line.
- A first line naming several pieces of work, with the same number of lines in Resources &
  Materials → split and pair them in order. Can't pair them confidently → every new card gets
  the full Resources & Materials, and say so.
- Anything the teacher calls optional is its own card, titled "Optional: …". Two optional
  activities are two cards. One card, one thing to do.
- A long project announced inside an in-class card gets its own card before the in-class
  card is archived, with the stated deadline in Desc. Due Date.

### E. Rename vague titles
Replace a generic title ("Friday/Monday Work", "Core Subjects") with what she'd need to pull
the right thing off the shelf: the workbook, book or lesson. Six words at most. Keep the
teacher's wording when it's ambiguous; don't tidy what might be a typo into a guess. Only
cards with an Assignment ID.

### F. Fix subjects filed as Other
Pick the subject from the tracker's own Subject list using the body. A card covering several
subjects is a split first. If one course keeps landing in Other, tell her the subject rule to
add in the extension's settings so it stops happening.

### G. Due date written in the description
Only for cards whose Desc. Due Date is empty. Ignore the "Due: YYYY-MM-DD" line at the very
bottom of a card's body: the extension writes it as a copy of Due Date. Only take a date the wording marks as a
deadline ("due", "turn in", "hand in by", or a date heading over the section it governs),
not a lesson day or reading schedule. Record it only when it **disagrees** with Due Date.
Put it in Desc. Due Date and never change Due Date: seeing the two side by side is the
point. A bare month and day takes the school year; if that lands more than a month from the
card's own dates, ask. A split card takes the date from its own section.

## Step 5: Show her, group by group

Present in this order and stop after each group for a yes:

1. Finished cards (one list, one yes; say how many)
2. Leftovers (keep or archive, card by card)
3. In-class work (one list, one yes; say which days rule it came from)
4. Splits (one at a time: the original, the new cards, how the materials divide)
5. Renames (one at a time: old name, new name, the line it came from)
6. Subjects (one list, one yes)
7. Desc. due dates (one list: the date, the words it came from, the Due Date beside it)

Anything uncertain comes out of its group and is asked on its own. Skip empty groups.

## Step 6: Make the approved changes

- **Archive** = tick the Archived checkbox: update the page's properties with
  `"Archived": "__YES__"`. **Never** delete a card or move it to Notion's trash. Archiving
  keeps it in the Archived tab; trash removes it from her tracker.
- **Rename**: update the `Name` property.
- **Subject**: update the `Subject` property.
- **Desc. due date**: update `date:Desc. Due Date:start` (YYYY-MM-DD) only.
- **Split**: create each new card in the tracker's data source with Name, Person, Subject,
  Teacher, Note Type "Assignments", Status "Not started", Assigned Date, Due Date, its own
  Desc. Due Date if its section gives one, its share of Resources & Materials, and its part of
  the body as content. Keep any file blocks from the original exactly as fetched so the
  attachment comes along. Give new cards **no** Assignment ID: that number belongs to one
  Gradelink assignment and the extension uses it to avoid duplicates. Then archive the
  original card.

## Step 7: Report

Counts by kind, then what you left alone and why (leftovers she kept, her own additions,
anything that didn't fit a rule).

## Learning from corrections

When she corrects a proposal ("no, keep those", "that should be two cards"), ask: "Want me
to remember that for next week?" On yes, add one dated line under Corrections on the
settings page, in her words. Read the Corrections list at the start of every run.

## Don't

- Don't change anything before her yes for that group.
- Don't run the Gradelink sync. That's the Chrome extension, which she runs herself.
- Don't touch cards whose Note Type is Resource.
- Don't sign in to Gradelink or any other site.
