# Pretty Picture Roster Maker

Turn a class list spreadsheet into a printable name-study sheet — photo, first name
and last initial, major — so you can learn your students' names in the first week.

**One file. No install, no server, no internet.**

- **Use it now:** <https://csu-fye.github.io/Pretty_Picture_Roster_Maker/>
- **Or keep your own copy:** download
  [`picture-roster-maker.html`](picture-roster-maker.html), double-click it, and it
  runs from your Desktop — no internet required. (From the hosted version:
  right-click → *Save Page As*.)

Either way the work happens in your browser. The hosted copy is just a convenient
way to get the file; a saved copy works with the wifi switched off, which is the
easiest way to satisfy yourself that nothing is being uploaded.

## Student data never leaves your computer

This is the whole point of the tool, so it is worth being precise about it:

- The spreadsheet is read **inside the browser tab**. There is no upload, no server,
  and no third-party library — the .xlsx is unzipped with the browser's own
  `DecompressionStream` and parsed with its own XML parser.
- The page ships a `Content-Security-Policy` of `default-src 'none'`. The browser
  itself refuses to let the page make *any* network request, so photos and names
  cannot be sent anywhere even in principle.
- The study sheet it saves carries the same policy, so the finished file can't
  phone home either.

It works with the wifi off. That's a reasonable way to convince yourself.

## Using it

1. Download the class list from your student information system as **.xlsx**, with
   photos included.
2. Open `picture-roster-maker.html` (double-click — it opens in your default browser).
3. Drag the spreadsheet onto the page.
4. Adjust the title, cards per row, name format, and sort order to taste.
5. **Print / Save as PDF**, or **Download study sheet (.html)** to keep a
   self-contained copy you can reopen offline.

The saved sheet has *Toggle names* and *Toggle majors* buttons — hide one and quiz
yourself on the other.

### What the spreadsheet needs

A header row with a **Name** column (`Last, First`, or separate `First name` /
`Last name` columns) and the student photos embedded in the sheet. `Major`,
`Pronouns`, and `CSU ID` are used when present and skipped when not.

A title row above the header is fine — the header row is found by keyword, and
columns are matched by their names rather than by position, so exports whose column
order differs still work.

### How photos are matched to students

Photos in these exports are floating images anchored to rows, not cell values, which
is why spreadsheet libraries usually show an empty Photo column. The tool reads the
drawing part and matches each picture to a student two ways: by the picture's own
`name="Photo<ID>"` against the ID column, falling back to the drawing's anchor row.
It also handles exports whose worksheet relationship part is missing (Banner's are).

Students without a photo get a grey tile with their initials rather than being dropped.

## Options

| Option | Notes |
| --- | --- |
| Sheet title | Guessed from the sheet name (e.g. `ENGR 111-003`); editable |
| Cards per row | 3–7; applies on screen and in print |
| Name format | First + last initial, first and last, first only, or `Last, First` |
| Order | Roster order, last name, first name, or by major |
| Show major / pronouns | Pronouns only offered when the file has them |
| Shrink photos | Roughly halves the saved file size; slower, off by default |

## Requirements

Any current browser — Chrome, Edge, Firefox, or Safari 16.4+. Nothing to install.

## A note for contributors

Please don't commit real class lists or generated study sheets. `.gitignore` blocks
the obvious filenames, but it can't catch everything.
