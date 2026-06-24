# CT in Fiction — Subject Database Documentation

## Overview

The CT in Fiction Subject Database is a Google Sheets spreadsheet that catalogs fictional characters typed using the Cognitive Typology (CT) framework developed by Juan E. Sandoval. Each row represents one character (subject) from a specific work of fiction. The database tracks the full CT profile of each character, epistemic metadata about the typing, links to extended analyses, and motif presence across all twelve cognitive functions.

---

## Sheet Structure

The single sheet ("Database") uses a four-row compound header before data begins.

- **Row 1** — Sheet title: "CT in Fiction · Subject Database"
- **Row 2** — Section group labels: Medium, Work, Subject, Motifs
- **Row 3** — Column names
- **Row 4** — Sub-column names (Metadata field names; Motifs function labels)
- **Row 5** — Motifs sub-sub-column names (Philosophical, Behavioural, Linguistic per function)
- **Row 6 onward** — Data rows, one per character

---

## Column Reference

### Work columns

| Column         | Description                                                                                                                                                                                                                                             |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Medium         | The medium of the source work (e.g. Animation, Game, Comic, Literature, Live Action)                                                                                                                                                                    |
| Title          | The title of the source work                                                                                                                                                                                                                            |
| Year           | Release year of the source work                                                                                                                                                                                                                         |
| Work Image URL | URL to the cover or poster art for the work. (high quality ones are on [TMDB](https://www.themoviedb.org/) for Movies and TV Shows, [Amazon](https://Amazon.com) for books and [SteamGridDB](https://www.steamgriddb.com/) for games on many platforms) |

### Subject columns

| Column | Description |
|---|---|
| Name | Character name. Prefixed with `(Demo)` for seed entries (see Entry Status below) |
| Subject Image URL | URL to an image of the character |
| Type | The character's CT type code (see Type Notation below) |
| Alternate Type | A secondary plausible typing, recorded when the primary type is not fully settled |
| Inter-Function Dynamics | The dominant introverted/extroverted function pairing driving the character's cognition (e.g. Fe+Ne, Fe+Se) |

### Derived columns

These columns are computed by formula from the Type field. They are hidden by default. They rely on custom Google Sheets formulas, and should not be edited by hand.

| Column              | Description                                       |
| ------------------- | ------------------------------------------------- |
| Lead Energetic      | Energetic polarity of the lead function           |
| Auxiliary Energetic | Energetic polarity of the auxiliary function      |
| Tertiary Energetic  | Energetic polarity of the tertiary function       |
| Polar Energetic     | Energetic polarity of the polar function          |
| Lead Function       | The lead cognitive function                       |
| Auxiliary Function  | The auxiliary cognitive function                  |
| Tertiary Function   | The tertiary cognitive function                   |
| Polar Function      | The polar (suppressed) cognitive function         |
| Judgment Axis       | The judgment axis pairing derived from the type   |
| Perception Axis     | The perception axis pairing derived from the type |
| Behaviour Qualia    | The behavioural qualia classification             |
| Quadra              | The quadra the character belongs to               |
| Emotional Attitude  | Whether the character is Guarded or Unguarded     |

### Auxiliary columns

| Column              | Description                                                               |
| ------------------- | ------------------------------------------------------------------------- |
| Unguardedness       | Degree of unguardedness: High, Medium, or Low                             |
| Guardedness         | Degree of guardedness: High, Medium, or Low                               |
| Raw Quadra          | A compact notation encoding quadra position (e.g. `I-I-`, `I---`)         |
| Initial Development | The character's subtype at the start of their arc                         |
| Final Development   | The character's subtype at the end of their arc                           |
| Analysis            | File path to the character's extended analysis (see Analysis Links below) |
| Notes               | Brief inline notes on the typing rationale or entry status                |

### Metadata columns

These are sub-columns nested under the "Metadata" header. They are managed by Apps Script automation.

| Column          | Description                                                                                                                                                                    |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| isPublished     | Boolean (1/0). Toggling this to 1 triggers the Apps Script to stamp publishedDate                                                                                              |
| publishedDate   | Timestamp. Set automatically when isPublished is first checked                                                                                                                 |
| editedDate      | Timestamp. Updated automatically whenever the isPublished column is changed                                                                                                    |
| isWorkArtOpaque | Boolean (1/0). When 1, tells the website to scale the work art to height  without distortion. Use this for portrait-oriented covers and Work art that aren't transparent logos |
| Author          | The username of the person who created or is responsible for the entry                                                                                                         |

---

## Type Notation

Type codes use the standard two-function CT shorthand (e.g. NeTi, FeNi, SeTi). When the typing is uncertain at a specific position, a macro-function group label replaces the individual function at that position:

| Macro label | Represents                  |
| ----------- | --------------------------- |
| Je          | Uncertain between Fe and Te |
| Ji          | Uncertain between Fi and Ti |
| Pe          | Uncertain between Se and Ne |
| Pi          | Uncertain between Ni and Si |

Examples:
- `NeTi` — fully settled: Ne lead, Ti auxiliary
- `NeJi` — Ne lead is confident, but whether the auxiliary is Ti or Fi is uncertain
- `PeTi` — lead is a Pe function (Se or Ne), auxiliary is Ti; Pe lead identity is uncertain

---

## Entry Status

Character names prefixed with `(Demo)` are seed entries. These were added to populate the database early and may be inaccurate. The Notes field on such entries typically reads "Seed entry. Might be inaccurate and removed eventually." Seed entries are published and visible but carry lower epistemic confidence than fully analyzed entries.

---

## Analysis Links

The Analysis column holds a relative file path in the format:

```
Work Title (Year)/Character Name.md
```

In the live website (that this spreadsheet feeds), this resolves to a GitHub raw file URL constructed using the latest commit SHA of the `trustosas/CT-in-Fiction-Analyses` repository. The linked `.md` file contains the full written analysis for the character.

---

## Motifs Section

The Motifs section spans the remainder of the sheet to the right of the main columns. It records the presence or absence of specific cognitive motifs for each character across all twelve CT functions.

The section is organized as a three-level header:

```
Motifs
	└── Energetics & Functions 
	    └── Category
	        └── Individual motifs
```

Each individual motif cell is a boolean (0 or 1). The name and description of each motif is stored as validation helper text on each cell in the live spreadsheet rather than as a visible column label.

---

## Apps Script Automation

Three automated behaviors are handled by a bound Google Apps Script:

**Publish date stamping** — When `isPublished` is changed to checked (true) for a row that has not previously been published, the script writes the current timestamp into `publishedDate` for that row. It uses `.getValue()` on the checkbox cell rather than `e.value` from the change event, because `e.value` is unreliable for checkbox changes in Sheets.

**Edit date stamping** — Whenever any cell in the `isPublished` column is edited, the script writes the current timestamp into `editedDate` for that row.

**Input Validation** — There's also a Quadra > Raw Quadra > Subtype validation by the App Script, which prevents manual entry errors from going live by clearing them automatically. (Requires internet) This behaviour doesn't overwrite publish/edit dates.
