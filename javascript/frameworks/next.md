# Next

Next.js is a React framework ideal for SSL sites.

## Create an app

`npx create-next-app {app name}`

## Serving Content

`npm run dev`

By default content is served on port 3000.

## Dir Structure

The root dir simply contains the package.json

`styles/` contains CSS, CSS modules
`pages/` contains individually crafted pages
`posts/` contans blog style posts
`public/` contains static assets

## Built In Components

Next includes some React components you get for free. You can include them via import:

`import {Component} from 'next/{component}';`

The `Link` component can be used to handle client-side nevigation in a performant way.
