# Books

A small public-domain library, hosted as a static site so it's readable from any browser with no app or account needed (https://johnoxley.github.io/books/).

## Contents

- **The Picture of Dorian Gray** — Oscar Wilde. Public domain (author died 1900). Text sourced from [Project Gutenberg](https://www.gutenberg.org/ebooks/174), redistributed under their license with the original header/footer intact.
- **Crime and Punishment** — Fyodor Dostoevsky, translated by Constance Garnett. Public domain. Text sourced from [Project Gutenberg](https://www.gutenberg.org/ebooks/2554), converted from the plain-text edition into a linked table of contents with per-chapter anchors.
- **Great Expectations** — Charles Dickens. Public domain. Text sourced from [Project Gutenberg](https://www.gutenberg.org/ebooks/1400), split into one markdown file per chapter (59 chapters) under [`great-expectations/`](great-expectations/README.md). Note: unlike the other two books, this one is plain markdown per chapter rather than a single `index.html`, so it isn't currently wired into the GitHub Pages site — browse it directly on GitHub for now.

## Adding another book

1. Create a new folder, e.g. `some-book/`
2. Drop an `index.html` in it (a cleaned-up Project Gutenberg HTML export works well)
3. Add a link to it from the root `index.html`

## Hosting

Served via GitHub Pages from the `main` branch. No build step.
