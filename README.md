# Books

A small public-domain library, hosted as a static site so it's readable from any browser with no app or account needed (https://johnoxley.github.io/books/).

## Contents

- **The Woman in White** — Wilkie Collins. Public domain. Text sourced from [Project Gutenberg](https://www.gutenberg.org/ebooks/583), split into 68 per-chapter markdown files under [`the-woman-in-white/`](the-woman-in-white/README.md), grouped by the book's three "Epochs" and preserving which narrator tells each chapter (the book is narrated by multiple characters). Single-page `index.html` for the Pages site, same convention as the others.
- **2026 reading list** — see [`reading-list.md`](reading-list.md).

## Reference

- **RFC 4301 — Security Architecture for IP** — original technical summary/analysis under [`reference/rfc4301-ipsec-architecture.md`](reference/rfc4301-ipsec-architecture.md). Not a reproduction of the RFC text.

## Adding another book

1. Create a new folder, e.g. `some-book/`
2. Drop an `index.html` in it (a cleaned-up Project Gutenberg HTML export works well)
3. Add a link to it from the root `index.html`

## Hosting

Served via GitHub Pages from the `main` branch. No build step.
