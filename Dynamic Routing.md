# Dynamic Routing

---
title: Dynamic Routing
date: May 22, 2021 at 8:41 AM
slug: dynamic-routing
category: nextjs
---

I don’t know how to do what I want. Hopefully someone out there does.

So what I wanted to do was have `https://domain.tld/:category` go to the category archive page and `https://domain.tld/:category/:slug` go to the single page (to borrow some terminology from WordPress). I thought it would work to have a folder structure like this:

```
/
|---- pages
      |---- [cat]
            |---- [slug].js
            |---- index.tsx
```

(By the way, is there a “standard” way to indicate folder structure in ASCII art? I mean, what I did makes sense to everyone, right? Right?)

Anyway. That turned out not to work exactly like I’d hoped. I was able to get `https://domain.tld/nextjs` to work, but so did things like `https://domain.tld/fake` and `https://domain.tld/doesntexist`. Not good.

I thought it would work to use `getStaticPaths` in `[cat]/index.tsx` like this:

```js
export const getStaticPaths = async () => {
    // boring code that fetches all categories
    const arrayOfCategoryNames = [];

    return {
        paths: [
            { params: { cat: arrayOfCategoryNames } }
        ],
        fallback: false
    }
}
```

That also turned out not to work exactly like I’d hoped; I got this error: `Error: A required parameter (cat) was not provided as a string in getStaticPaths for /[cat]`.

The final thing I tried was to use a catch-all route, renaming `[cat]` to `[...cat]`. That kinda worked, in the sense that `next` seemed to accept an array of strings as the parameter `(cat)`. It also did not work, in the sense that every page returned a 404 error, even `:categories` that matched a string in the array of category names.

It was then that I gave up and ate nuggets. Oh, and wrote this thing. So if you’re an expert at NextJS and you know what I did wrong, [please tell me](https://twitter.com/s_cottontail24).
