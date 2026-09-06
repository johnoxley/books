# Books

A small public-domain library, hosted as a static site so it's readable from any browser with no app or account needed (https://johnoxley.github.io/books/).

## Contents

- **The Picture of Dorian Gray** — Oscar Wilde. Public domain (author died 1900). Text sourced from [Project Gutenberg](https://www.gutenberg.org/ebooks/174), redistributed under their license with the original header/footer intact.
- **Crime and Punishment** — Fyodor Dostoevsky, translated by Constance Garnett. Public domain. Text sourced from [Project Gutenberg](https://www.gutenberg.org/ebooks/2554), converted from the plain-text edition into a linked table of contents with per-chapter anchors.
- **Great Expectations** — Charles Dickens. Public domain. Text sourced from [Project Gutenberg](https://www.gutenberg.org/ebooks/1400), converted from the plain-text edition into a single-page `index.html` (linked table of contents with per-chapter anchors, matching the other books) for the Pages site. The source is also kept split into 59 per-chapter markdown files under [`great-expectations/`](great-expectations/README.md) for easy individual-chapter reading/editing on GitHub.
- **The Woman in White** — Wilkie Collins. Public domain. Text sourced from [Project Gutenberg](https://www.gutenberg.org/ebooks/583), split into 68 per-chapter markdown files under [`the-woman-in-white/`](the-woman-in-white/README.md), grouped by the book's three "Epochs" and preserving which narrator tells each chapter (the book is narrated by multiple characters). Single-page `index.html` for the Pages site, same convention as the others.

## Adding another book

1. Create a new folder, e.g. `some-book/`
2. Drop an `index.html` in it (a cleaned-up Project Gutenberg HTML export works well)
3. Add a link to it from the root `index.html`

## Hosting

Served via GitHub Pages from the `main` branch. No build step.
