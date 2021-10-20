# WordPress Inline Images

---
title: WordPress Inline Images
date: Aug 24, 2021 at 12:36 PM
slug: wordpress-inline-images
category: gatsby
---

Building on the [code from last time](/gatsby/using-images-from-wordpress-in-gatsby), I discovered that you can set default options for  how Gatsby will process your images:

```js
// gatsby-config.js

module.exports = {
	plugins: [
		{
			resolve: 'gatsby-plugin-sharp',
			options: {
				defaults: {
					placeholder: 'blurred'
				}
			}
		}
		// other plugins snipped for clarity
	]
};
```

There are [a lot of options](https://www.gatsbyjs.com/docs/reference/built-in-components/gatsby-plugin-image/#all-options) here, but not all of them seem all that useful or necessary or documented.

A caveat, though; these options only apply to images sourced via `childImageSharp` in GraphQL and used via `StaticImage` or `GatsbyImage`, not to inline images in WordPress content. You can fetch the featured image for a WordPress post via GraphQL, but any images in the content itself (e.g., ones added via Gutenberg’s Image block) get transformed by some other magic and these options don’t apply. I briefly looked at all the code for `gatsby-source wordpress` and I couldn’t figure out how the images get transformed so I couldn’t figure out a way to fix this. If you know, [please tell me](https://twitter.com/s_cottontail24).