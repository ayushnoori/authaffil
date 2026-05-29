# authaffil

![License: LPPL 1.3c](https://img.shields.io/badge/license-LPPL%201.3c-blue.svg)
![Requires: expl3](https://img.shields.io/badge/requires-expl3-orange.svg)
![Engines: pdfLaTeX | XeLaTeX | LuaLaTeX](https://img.shields.io/badge/engines-pdfLaTeX%20%7C%20XeLaTeX%20%7C%20LuaLaTeX-green.svg)

First-appearance numbering of author affiliations for LaTeX.

Hand-numbered affiliation superscripts are tedious and fragile: add or reorder
one author and you renumber everything by hand, often introducing errors. With
`authaffil` you define each affiliation once under a short code, tag each author
with codes, and the package assigns the numbers in the order the codes are
**first cited** across the author list — then prints the affiliation block in
that same order. Add, remove, or reorder authors and every number updates
automatically. There is never a numeral to maintain by hand.

## Features

- Define affiliations once, keyed by a short code (e.g. `HMS`), in any order.
- Numbers assigned by first citation; the printed list follows the same order.
- Footnote symbols (`*`, `\dagger`, `\ddagger`, …) handled per author.
- Per-author numbers shown ascending by default (`1,5,30`); citation order optional.
- Build warnings for codes cited-but-undefined and defined-but-uncited.
- Layout-agnostic: drop the print commands into any journal class's `\author{}`.
- Pure `expl3`; no dependencies beyond a modern TeX distribution.

## Installation

Copy `authaffil.sty` next to your `.tex` file (or anywhere on your TeX path),
then load it:

```latex
\usepackage{authaffil}            % default: per-author numbers sorted ascending
\usepackage[nosort]{authaffil}    % per-author numbers in citation order
```

On Overleaf, upload `authaffil.sty` into your project. It works with pdfLaTeX,
XeLaTeX, and LuaLaTeX.

## Quick start

```latex
% 1. Define affiliations (the order of these lines is irrelevant).
\DefineAffiliation{HMS}{Dept. of Biomedical Informatics, HMS, Boston, MA, USA}
\DefineAffiliation{OXE}{Dept. of Engineering Science, University of Oxford, UK}
\DefineAffiliation{ETH}{Dept. of Computer Science, ETH Zurich, Switzerland}

% 2. List authors, tagging each with codes (+ optional footnote symbols).
\AddAuthor{Ayush~Noori}{HMS,OXE}
\AddAuthor[*]{Manuel~Burger}{ETH}
\AddAuthor[\dagger,\ddagger]{Marinka~Zitnik}{HMS}

% 3. (Optional) legend lines.
\AddAffilNote{*}{Equal contribution.}
\AddAffilNote{\dagger}{Jointly supervised this work.}

% 4. Typeset — e.g. inside \author{...} of your template.
\author{{\small\begin{center}
    \PrintAuthors      \\[2mm]
    \PrintAffiliations \\[1mm]
    \PrintAffilNotes
\end{center}}}
```

This renders Noori as `1,2`, Burger as `3,*`, Zitnik as `1,†,‡`, followed by the
three affiliations numbered `1`–`3` and the two legend lines.

## Command reference

| Command | Purpose |
| --- | --- |
| `\DefineAffiliation{CODE}{address}` | Register one affiliation. `CODE` is any short string; `address` may contain `\\` for forced line breaks. Call once per affiliation, in any order. |
| `\AddAuthor[symbols]{Name}{CODE,CODE,…}` | Append one author. Each new code gets the next number on first sight. Optional `symbols` (e.g. `*`, `\dagger`) are appended after the numbers; write them pre-separated, e.g. `[\dagger,\ddagger]`. Use `~` in names to prevent line breaks. |
| `\AddAffilNote{symbol}{text}` | Register one legend line, typeset as a superscript symbol followed by text. Lines print in the order added. |
| `\PrintAuthors` | Typeset the author names, comma-separated, each with its computed superscript. Sets no font size — wrap it yourself. |
| `\PrintAffiliations` | Typeset the numbered affiliation list, one per line, in first-appearance order. Switches to `\footnotesize`. |
| `\PrintAffilNotes` | Typeset the legend lines, one per line. Switches to `\footnotesize`. |

## Options

| Option | Effect |
| --- | --- |
| `sort` | *(default)* Numbers within a single author are shown ascending: `1,5,30`, regardless of citation order. |
| `nosort` | Numbers within a single author are shown in the order the codes were cited. |

## How the numbering works

A code receives its number the first time *any* author cites it. So for an
author who introduces several brand-new affiliations at once, the left-to-right
order of that author's code list fixes those numbers. To force a particular
global ordering, control the order of first citation — the simplest lever is the
first author who uses each code. Everything downstream (the per-author
superscripts and the printed affiliation list) is derived from that single
source, so there are never two places to keep in sync.

## Diagnostics

The package writes a warning to the log and terminal in two situations. Neither
is fatal; both exist to catch copy-paste slips before submission.

- **`unknown-code`** — an author cites a code that was never defined with
  `\DefineAffiliation`. A number is still allocated, but no address exists for it.
- **`unused-affil`** — a code was defined but never cited by any author, so it
  does not appear in the printed list. Checked once, at `\end{document}`.

## Repository contents

| File | Description |
| --- | --- |
| `authaffil.sty` | The package. |
| `authaffil-doc.tex` / `authaffil-doc.pdf` | Manual source and compiled PDF. |
| `example.tex` | A complete multi-author, multi-affiliation worked example. |

To rebuild the manual:

```sh
pdflatex authaffil-doc.tex
pdflatex authaffil-doc.tex   # second pass resolves cross-references
```

## License

Released under the [LaTeX Project Public License](https://www.latex-project.org/lppl/),
version 1.3c or later.

## Author

Ayush Noori

## Contributing

Issues and pull requests are welcome.
