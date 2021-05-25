# More Dynamic Routing

---
title: More Dynamic Routing
date: May 24, 2021 at 4:25 PM
slug: more-dynamic-routing
category: nextjs
---

I figured out what was wrong with [my code from last time](https://stephencottontail.dev/nextjs/dynamic-routing): I was returning an array of strings from `getStaticPaths`, but you actually have to return an array of objects:

```js
// pages/[category]/index.tsx

export const getStaticPaths: GetStaticPaths = async () => {
	// code that fetches all the categories

	return {
		paths: [
			{ params: { category: "foo" } },
			{ params: { category: "bar" } },
			{ params: { category: "baz" } },
			// ...so on and so forth
		],
		fallback: false
	}
}
```

From there, I was also able to figure out that you can nest dynamic routes:

```js
// pages/[category]/[slug].tsx

export const getStaticPaths: GetStaticPaths = async () => {
	// code that fetches all the slugs and the matching category

	return {
		paths: [
			// because there are two dynamic routes for this page,
			// you need to return an array of objects that each
			// have both placeholders filled
			{ params: { category: "foo", slug: "bar" } },
			{ params: { category: "baz", slug: "qux" } },
			// ...so on and so forth
		],
		fallback: false
	}
}
```

From the above code, each object maps to a specific URL: `https://domain.tld/foo/bar` will work, but `https://domain.tld/foo/qux` will return a 404 error.
