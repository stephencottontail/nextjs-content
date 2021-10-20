# Using Images from WordPress in Gatsby

---
title: Using Images from WordPress in Gatsby
date: Aug 19, 2021 at 5:19 PM
slug: using-images-from-wordpress-in-gatsby
category: gatsby
---

This is confusing on purpose, I swear. You need three plugins: `gatsby-plugin-sharp`, `gatsby-image-sharp`, and `gatsby-transformer-sharp`. I think some of those are listed as dependencies for `gatsby-source-wordpress`, but I’m not 100% sure.

```js
// gatsby-config.js

module.exports = {
	plugins: [
		{
			resolve: "gatsby-source-wordpress",
			options: {
				html: {
					useGatsbyImage: true
				}
				// other options snipped for clarity
			}
		},
		{
			resolve: "gatsby-plugin-sharp"
		},
		{
			resolve: "gatsby-image-sharp"
		},
		{
			// this is needed for `gatsby-source-wordpress` to
			// return the correct data
			resolve: "gatsby-transformer-sharp"
		},
		// other plugins snipped for clarity
	]
}
```

`gatsby-transformer-sharp` is one of the key items here; it adds a `childImageSharp` (with a [boatload of options](https://www.gatsbyjs.com/docs/reference/built-in-components/gatsby-plugin-image#all-options), by the way) node to wpPost \> nodes \> featuredImage \> node \> localFile:

```js
export const query = graphql`
	{
		allWpPost {
			nodes {
				featuredImage {
					node {
						localFile {
							childImageSharp
						}
					}
				}
			}
		}
	}
`;

// as always, some fields are omitted for clarity
```

The other key item is the `useGatsbyImage` option under `gatsby-source-wordpress`: that option tells Gatsby to automatically transform any inline image (such as ones added via Gutenberg’s Image block) into the special Gatsby image.