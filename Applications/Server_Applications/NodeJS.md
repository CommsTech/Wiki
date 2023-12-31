---
title: 
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# Error: cannot find module [Node npm Error Solved]

![Kolade Chris](https://www.freecodecamp.org/news/content/images/size/w60/2023/01/kolade-recent.jpg)

[Kolade Chris](https://www.freecodecamp.org/news/author/kolade/)

![Error: cannot find module [Node npm Error Solved]](https://www.freecodecamp.org/news/content/images/size/w2000/2022/11/factory-4757647_1280.jpg)

If you’re a developer that works with Node JS and JavaScript libraries and frameworks like React, Vue, and Angular, then you might have encountered the "Error: cannot find module" error.

In this article, I’m going to show you how to fix the error.

## Why the "Error: cannot find module" Occurs

This error occurs because of the following reasons:

- you’re trying to import an item from a module you don’t have installed in your project directory
- you’re importing some things from an outdated package
- you’re pointing to a file that does not exist

In the screenshot below, you can see that I’m getting the error:

![ss1](https://www.freecodecamp.org/news/content/images/2022/11/ss1.png)

I’m getting the error because I’m trying to import the freeCodeCamp icon from the react-icons package, which I don’t have installed.

```js
import { FaFreeCodeCamp } from "react-icons/fa";
```

## How to Fix the "cannot find module" Error

If you get this error, the solution is always in the error. The module (package) not found is always specified in the format "Module not found: Error: Can't resolve 'package name' in 'project directory".

In my case, I got it like this "Module not found: Error: Can't resolve 'react-icons/fa' in 'C:\Users\user\Desktop\Projects\Address Locator\address-locator\src'".

To fix the error, you need to install the package that is absent in your project directory – `npm install package-name` or `yarn add package-name`.

In my case, I need to install the `react-icons` package so the freeCodeCamp icon can be resolved. I’ll do that by running `yarn add react-icons`.

Once I install the package and run the app, everything should successfully compile:

![ss2](https://www.freecodecamp.org/news/content/images/2022/11/ss2.png)

If you install the package but you still get the error, then follow the steps below:

- delete the node modules folder by running `rm -rf node_modules`
- delete package.lock.json file by running `rm -f package-lock.json`
- clean up the NPM cache by running `npm cache clean --force`
- install all packages again by running `npm install`

That should fix the error for you.

## Conclusion

When you get the “cannot find module” error, or “module not found”, it means you’ve not installed the package you’re trying to use.

If the error occurs even if you have the package installed, then the fixes suggested in this article can help you out.

Thank you for reading.