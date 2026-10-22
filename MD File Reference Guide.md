

Here is a reference guide explaining the standard Markdown (`.md`) formatting symbols and what they do:



1. `#` (Hashtag / Pound Sign)

- **Function:** Defines Headings.
- **Syntax:** Place 1 to 6 `#` symbols at the start of a line followed by a space.
- **Usage:**
    - `# H1 Header` (Main Document Title)
    - `## H2 Header` (Major Section)
    - `### H3 Header` (Subsection)

---

2. `*` (Asterisk) & `_` (Underscore)

- **Function:** Formats text emphasis or creates bullet points.
- **Syntax:**
    - **Single (*****text*** **or** **_text_****):** _Italic_ text.
    - **Double (******text**** **or** **__text__****):** **Bold** text.
    - **Triple (*******text***** **or** **___text___****):** **Bold and Italic** text.
    - **Horizontal Rule:** `***` or `---` on a blank line creates a full-width divider line.

---

3. `-` (Hyphen / Dash) & `+` (Plus)

- **Function:** Creates unordered list items, task lists, or section dividers.
- **Syntax:**
    - **Bullet List:** Start a line with `-` or `+` .
    - **Checklist:** `- [ ]` creates an empty checkbox; `- [x]` creates a completed checkbox.
    - **Horizontal Rule:** Three hyphens `---` on a line by themselves create a divider line.

---

4. `>` (Greater-Than / Right Angle Bracket)

- **Function:** Creates a Blockquote.
- **Syntax:** Place `>` at the start of a line to indent and highlight quoted text. Use `>>` for nested blockquotes.

---

5. `` ` `` (Backtick)

- **Function:** Highlights code or fixed-width text.
- **Syntax:**
    - **Inline Code:** Wrap words in single backticks (`` `code` ``).
    - **Code Block:** Enclose multiple lines between triple backticks (` ``` `). Add a language label immediately after the first set (e.g. ` ```javascript `) for syntax highlighting.

---

6. `[` `]` (Square Brackets) & `(` `)` (Parentheses)

- **Function:** Constructs Hyperlinks, Reference Links, and Images.
- **Syntax:**
    - **Link:** `[Display Text](https://example.com)`.
    - **Image:** `![Alt Text](image.png)` (preceded by an exclamation mark).

---

7. `!` (Exclamation Mark)

- **Function:** Distinguishes an embedded Image from a standard text link when placed immediately before square brackets (`![Alt Text](URL)`).

---

8. `|` (Pipe)

- **Function:** Separates columns in Markdown Tables.
- **Syntax:** `| Column 1 | Column 2 |`. The second row uses hyphens and colons (`|:---|:---:|---:|`) to align text left, center, or right.

---

9. `\` (Backslash)

- **Function:** Serves as an Escape character.
- **Syntax:** Prefix any Markdown symbol with `\` (e.g., `\#` or `\*`) to render it as literal plain text without applying formatting. Adding `\` at the end of a line forces a soft line break.

---

10. `~` (Tilde)

- **Function:** Applies Strikethrough formatting.
- **Syntax:** Wrap text in double tildes `~~crossed out text~~`.

---

11. `:` (Colon)

- **Function:** Sets table alignment (`:---` or `---:`) or renders emoji graphics (`:smile:`) on platforms like GitHub and Azure DevOps.

---

12. `$` (Dollar Sign)

- **Function:** Renders KaTeX/LaTeX mathematical formulas inline (`$E=mc^2$`) or as block equations (`$$...$$`).


_Last Updated: October 2026 | Maintainer: QA Automation Team_