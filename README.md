# pickles-venture
## local development Preview ?
1. `npm i docsify-cli -g  `
2. `npm start`

## Add new documentation section
1. Add menu links ? add filename in `_sidebar.md`
2. Example you make a new section `./doc/your-own-doc-section.md`
3. Link it inside `_sidebar.md`

## Automatically make diagram with textcode?
1. Go mermaid playground website - google it
2. Example we have is  `mermaidexample.md`

## Serve process on github pages once master is merged
1. This serve is using github pipeline to serve docsify as github pages
2. Do note we need `.nojekyll` to allow `_sidebar.md ` and `_pages.md`, anything with begin `_` to render as filename correctly.
3. Read official docsify.org documentation  
