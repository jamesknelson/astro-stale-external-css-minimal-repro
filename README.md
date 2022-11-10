# Astro Bug Report Minimal Repro

Astro version: 1.6.3
Package manager: pnpm 

## Stale CSS is displayed after modifying .astro files located outside the project root

This repository is set up as a monorepo, with two packages:

- packages/theme (exports .astro that would theoretically be shared by multiple projects)
- demo (a package that loads and displays the theme components for development purposes)

The repository initially works after setup:

```
pnpm i
cd demo
pnpm run dev
```

However, the styles break after modifying Layout.astro:

- If the styles are modified (e.g. changing one of the background colors), then the changes are not reflected in the app
- If the markup is modified, *all scoped styles* will no longer be displayed

Upon some investigation:

- Modifying the markup will update the displayed markup, causing a new scope style hash to be rendered.
- The CSS rendered for the file will never update. It continues to use the initial styles and scope hash.

## Motivation

Separating shared .astro files into a separate repository would allow for standard "theme" packages that can be shared between blogs (or other types of websites).

It's already possible to develop these themes by reloading the webserver after each change, but having HMR working for external packages would markedly improve development velocity.