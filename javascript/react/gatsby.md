# Gatsby

Gatsby is a static site generator that enables sever-side rendering for your React apps.

## Gatsby CLI

`gatsby new [<site-name> [<starter-url>]]` creates new site

`gatsby develop` starts development build

`gatsby build` starts production build

`gatsby serve` serves the production build for testing

`gatsby clean` wipes out cache

## Gatsby Site Layout

Gatsby will somewhat magically create URL routes based on JS files with valid React components in `src/pages/`. So typically you will have your pages located there, and the smaller components in `src/components/`

## Built in Components

Gatsby has a number of built-in components that can be used. You can import them via `import { ComponentName } from 'gatsby';`

### Link

`<Link to="/route/">Link</Link>`
