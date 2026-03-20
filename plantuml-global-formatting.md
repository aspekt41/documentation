# PlantUML Global Formatting Cheat Sheet

This cheat sheet covers formatting and presentation features that work across all PlantUML diagram types — class, sequence, and others. These are the "meta" controls: how text is laid out, how diagrams are titled and scaled, and how to make your diagrams more readable without changing their content.

---

## Line Breaks in Labels

This is one of the most useful things to know. Any text label in PlantUML — arrow messages, notes, class members — can contain a newline using `\n`:

```plantuml
User -> OrderService : placeOrder(userId, items,\nshippingAddress, paymentMethod)
```

Use `\n` anywhere you want text to wrap to the next line.

### Newlines in Notes

```plantuml
note right : This check validates\nthe payment token\nbefore proceeding
```

### Newlines in Class Members

```plantuml
class OrderRequest {
  + userId: int
  + items: List<Item>
  + shippingAddress:\n  Address
}
```

**Why:** Long labels on arrows or notes will stretch the diagram horizontally and become hard to read. Breaking them with `\n` keeps diagrams compact and scannable.

---

## Creole Markup (Text Formatting Inside Labels)

PlantUML supports a lightweight markup language called Creole for styling text inside labels, notes, and titles.

### Bold, Italic, Monospace

```plantuml
note right : **Bold text**\n//Italic text//\n""monospace text""
```

| Syntax        | Result      | Use for...                              |
|---------------|-------------|------------------------------------------|
| `**text**`    | **bold**    | emphasizing key terms                    |
| `//text//`    | *italic*    | variable names, slight emphasis          |
| `""text""`    | `monospace` | code values, type names, identifiers     |
| `--text--`    | ~~strikethrough~~ | marking deprecated or removed things |
| `__text__`    | underline   | additional emphasis                      |

### Colored Text

```plantuml
note right : <color:red>Error path</color>\n<color:green>Success path</color>
```

### Lists Inside Notes

```plantuml
note over Service
  Steps:
  * Validate token
  * Look up user
  * Return profile
end note
```

**Why:** Creole markup lets you add visual hierarchy inside notes and labels, which is especially useful for documenting rules, options, or multi-step logic.

---

## Title, Header, and Footer

### Title

```plantuml
title Place Order — Happy Path
```

Or multi-line:

```plantuml
title
  Place Order
  Happy Path
  Version 1.2
end title
```

**Why:** Always add a title. It makes diagrams self-documenting when shared out of context (in a wiki, PR, or email).

### Header and Footer

```plantuml
header
  Internal Draft — Not for Distribution
end header

footer
  Page %page% of %lastpage% — Generated: %date%
end footer
```

Available variables: `%page%`, `%lastpage%`, `%date%`, `%time%`, `%filename%`

**Why:** Headers and footers are useful for versioning or marking confidentiality when exporting diagrams as images or PDFs.

---

## Scale and Output Size

```plantuml
scale 1.5
```

Scales the entire diagram up or down. Values > 1 make it larger, < 1 make it smaller.

```plantuml
scale 800 width
scale 600 height
scale 800*600
```

**Why:** Useful when exporting to fit a specific document layout, slide, or wiki page width.

---

## Direction

By default, most diagrams flow top-to-bottom. You can change this:

```plantuml
left to right direction
```

Or explicitly set the default:

```plantuml
top to bottom direction
```

**Why:** Wide diagrams with many participants often read more naturally left-to-right. Class diagrams with deep inheritance hierarchies can look cleaner top-to-bottom.

---

## Skinparam — Global Styling

`skinparam` controls the visual appearance of diagrams. It works for both class and sequence diagrams (and others). Settings can be applied globally or scoped to a specific element type.

### Global Font

```plantuml
skinparam defaultFontName "Helvetica"
skinparam defaultFontSize 13
skinparam defaultFontColor #333333
```

### Background and Borders

```plantuml
skinparam backgroundColor #FAFAFA
skinparam roundCorner 8
```

### Monochrome Mode

```plantuml
skinparam monochrome true
```

**Why:** Useful for printing or for contexts where color adds no meaning. Forces a clean black-and-white output.

### Shadowing

```plantuml
skinparam shadowing false
```

**Why:** PlantUML adds drop shadows by default. Many people find `shadowing false` produces cleaner, flatter diagrams.

### Common Skinparam Targets

| Target                         | What it controls                      |
|--------------------------------|----------------------------------------|
| `classBackgroundColor`         | fill color of class boxes              |
| `classBorderColor`             | border color of class boxes            |
| `sequenceArrowThickness`       | thickness of sequence arrows           |
| `sequenceParticipantBackgroundColor` | fill of sequence participant boxes |
| `sequenceLifeLineBorderColor`  | color of lifeline dashes               |
| `sequenceGroupBorderColor`     | border of alt/loop/opt groups          |
| `noteBorderColor`              | border of note boxes                   |
| `noteBackgroundColor`          | fill of note boxes                     |

### Colors

PlantUML accepts standard HTML color names (`LightBlue`, `Tomato`, `DarkGray`) and hex values (`#4A90D9`).

---

## Legends

```plantuml
legend right
  Solid arrow = synchronous call
  Dashed arrow = response
  Diamond = composition
end legend
```

Or just `legend` for auto-positioned:

```plantuml
legend
  |= Symbol |= Meaning |
  | --> | HTTP Response |
  | ->> | Async Event |
end legend
```

**Why:** Legends help readers who aren't familiar with UML notation understand your diagram without prior knowledge. Use them when sharing with non-technical stakeholders.

---

## Tables in Notes and Legends

PlantUML supports simple tables using `|` syntax inside notes and legends:

```plantuml
note right
  |= Field       |= Type   |
  | orderId      | int     |
  | createdAt    | Date    |
  | status       | String  |
end note
```

`|=` creates a header cell. `|` creates a regular cell.

**Why:** Tables are useful for documenting data shapes, enumerating options, or summarizing rules inline on the diagram.

---

## Multiline Notes (Block Syntax)

For notes with more than a line or two, the block form is much more readable than chaining `\n`:

```plantuml
note over OrderService
  This service is responsible for:
  * Validating cart contents
  * Charging payment
  * Sending confirmation emails
end note
```

The block form (`note over ... end note`) supports the full Creole syntax including lists, tables, and markup.

---

## Comments

PlantUML line comments use a single quote `'`:

```plantuml
' This line is ignored by the renderer
participant User  ' inline comments also work
```

Block comments:

```plantuml
/'
  This is a multi-line comment.
  Useful for temporarily hiding a section.
'/
```

**Why:** Block comments are handy for disabling parts of a diagram during iteration without deleting them.

---

## Preprocessing — Variables and Conditions

PlantUML includes a basic preprocessor for reuse and conditional rendering:

```plantuml
!define SERVICE_COLOR LightSteelBlue

skinparam participantBackgroundColor SERVICE_COLOR
```

```plantuml
!ifdef SHOW_INTERNALS
  participant InternalCache
!endif
```

**Why:** Variables let you define colors or names once and reuse them. Conditionals let you produce different versions of the same diagram (e.g., simplified vs. detailed) from one source file.

---

## Including Other Files

```plantuml
!include common-styles.iuml
!include_many diagrams/*.iuml
```

**Why:** Shared style definitions (skinparam blocks, color variables) can live in a single file and be included across all your diagrams to keep them visually consistent.

---

## Quick Reference Card

```
Newlines in labels     \n
Text styling           **bold**  //italic//  ""mono""  <color:red>text</color>
Title                  title My Diagram / title ... end title
Header/footer          header ... end header / footer ... end footer
Scale                  scale 1.5 / scale 800 width
Direction              left to right direction / top to bottom direction
Skinparam              skinparam shadowing false
                       skinparam monochrome true
                       skinparam defaultFontName "Helvetica"
Legend                 legend right ... end legend
Tables (in notes)      |= Header | / | Cell |
Block notes            note over P1, P2 ... end note
Comments               ' line comment   /' block comment '/
Variables              !define NAME value
Conditionals           !ifdef NAME ... !endif
Includes               !include filename.iuml
```
