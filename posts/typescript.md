--- 
title: 'Foray into Typescript'
date: '2023-02-19' 
---
### Typescript 

Typescript can be installed as a 'devDependency', which is a library your project depends on for development purposes only. This can be done as follows: 

```bash
npm install --save-dev typescript
```

Something I noticed that caused me lots of problems intially was how Typescript doesn't automatically add the '.js' suffix for imported modules. 

When you compile your '.ts' source files to Javascript with 'npx tsc', any imports without an extension will also not have an extension in the emitted Javascript files. 

For example: 

```typescript 
// source file
import Grid from "./Grid" 

// In the emitted javascript file,
// you might expect 'import Grid from "./Grid.js"' but you actually get
import Grid from "./Grid"
```

One solution is to import other Typescript modules with the '.js' extension. 

So if you needed to import everything from Square.ts in App.ts, then you would write 

```typescript 
// App.ts
import * as Square from "./Square.js"
``` 

and Typescript will recognise that you are importing a Typescript file despite the '.js' file extension. 

This workaround works but you will have to get used to the inconsistency in how your source files are named compared to how they are imported. 