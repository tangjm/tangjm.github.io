--- 
title: 'The purpose of module bundlers' 
date: '2023-11-14' 
---
What problem does a module bundler like Webpack solve? 

It enables us to organise our front-end javascript into modules and use import/require and export statements. Without a module bundler, javascript code written in this way won't be able to run in the browser because it involves constructs like import/export statements that are only available⁬ with respect to a file system. This is because when importing an object, we need to specify the file that contains the object we need and this is done by providing a file path, for example: `import 'Dog' from 'src/models/Dog.js'`. But a file path would only have any significance in the context of a file system - 'src/models/Dog.js' has meaning only when understood as a relative file path. Now as the browser does not have access to your file system for security reasons, it won't be able to interpret and execute javascript code that contains import statements and file paths. This is why we cannot organise front-end javascript code with import/export statements without a module bundler. 

A module bundler will take our collection of javascript files organised into modules and resolve all the import/export statements found in them by replacing them with the file contents they reference. The end result is a single javascript file with no occurrences of import/export statements that can be linked in your HTML and run in the browser. 

The module bundler introduces a build step in your development workflow, which allows it to access your file system through NodeJS. It locates files referenced by import statements to perform the necessary replacements of import statements with their imported contents. And so, interestingly, NodeJS is also used in front-end applications nowadays, albeit as a development tool to help with code organisation. It has also been argued that it comes with performance benefits too. 

The advantages of organising code into modules is that it becomes easier to see the exact dependencies for each javascript file. Getting the order of dependencies right becomes much easier too. And unused dependencies become easier to spot.
