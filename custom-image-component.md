# Custom image component

---
title: Custom Image Component
date: Jun 2, 2021 at 10:42 PM
updated: Jun 5, 2021 at 3:03 PM
slug: custom-image-component
category: nextjs
---

This started out with a simple goal: use Next.JS’s cool `Image` component to take advantage of all the cool image optimizations it does behind the scenes. Unfortunately, the [sample Markdown-powered blog](https://github.com/vercel/next.js/tree/canary/examples/blog-starter) doesn’t have any images in the Markdown itself, probably because it turned out to be obnoxiously convoluted. **Update**: I just remembered that the sample blog uses `dangerouslySetInnerHTML` instead of converting to JSX, which is probably another reason they don’t use the `Image` component.

I’m using the `remark` and `rehype` ecosystems to handle Markdown, and one cool thing that `retype-react` can do is map custom React components to HTML tags. The first problem I ran into is that Next.JS’s `Image` component requires that you pass in the height and width of the image (unless you use `layout="fill"`, which wouldn’t have worked for my particular use case) and by default, you don’t have that information. I found `rehype-image-size`, which adds height and width attributes to the `<img>` tag, but since it runs in a Node environment, it has to be run in `getStaticProps`, which meant that I had to rewrite a bit of my Markdown-processing code:

```js
import fs from "fs";
import { join } from "path";

import remark from "remark";
import frontmatter from "remark-frontmatter";
import extract from "remark-extract-frontmatter";
import yaml from "yaml";
Import unwrap from "remark-unwrap-images";
import toRehype from "remark-rehype";
import toString from "rehype-stringify";
import size from "rehype-image-size";

// code snipped for clarity

export const getStaticProps = async (context) => {
	const { slug } = context.params;
	const contents = await remark()
		.use(frontmatter, [{ type: yaml, anywhere: true, marker: '-' }])
		.use(extract, { yaml: yaml.parse })
		.use(unwrap)
		.use(toRehype)
		.use(size)
		.use(toString)
		.process(fs.readFileSync(join(process.cwd(), "public", slug + ".md")));

	return {
		props: {
			slug: slug
			contents: contents.contents // lol
		}
	}
};
```

(Note: I removed the TypeScript-specific stuff from this example, but it’s there in the actual repository.)

(Another note: `rehype-image-size` doesn’t have types so VSCode complains. I don’t care.)

Notice how cool this code is: I read in the Markdown, extract the frontmatter so `rehype` doesn’t get confused, remove the `<p>` tags surrounding the images so we don’t end up with a `<div>` tag inside a `<p>` tag later, convert everything to HTML, add `height` and `width` attributes to the `<img>` tag, and finally convert it to a string so `getStaticProps` can pass it to the page component. Then the page component has to parse the string again, convert it all to JSX including my custom image component, and finally display everything on the page:

```js
import unified from "unified";
import rehype-parse from "rehype-parse";
import { createElement, Fragment } from "react";
import Prose from "../../components/Prose";
import MyImage from "../../components/Image";

// code snipped for clarity

const Slug = (props) => {
	const { slug, contents } = props;
	const content = unified()
		.use(rehype-parse, {
			fragment: true
		})
		.use(toReact, {
			createElement: createElement,
			Fragment: Fragment,
			components: {
				img: MyImage
			}
		})
		.processSync(contents);

	return (
		{ /* code snipped for clarity */ }
		<Prose>{contents.result}</Prose>
		{ /* code snipped for clarity */ }
	);
};

// code snipped for clarity

export default Slug;
```

You might be asking, “Hey Steve, why did you import `unified` and `rehype-parse` instead of just using the standard `rehype` import?” and if I was there with you, I might reply, “Well, reader, it’s because if you use `rehype` on it’s own, it runs `rehype-parse` with the default options, which includes parsing HTML in document mode instead of fragment mode, which means it automatically adds `<html>`, `<head>`, and `<body>` tags as appropriate. Don’t worry, though; I didn’t know that either, and if it hadn’t been for a few lucky clicks on the GitHub page for `rehype` , I wouldn’t have figured it out.”

The actual custom component itself was the easiest part of the whole thing, mostly because I managed to see the final iceberg waiting in my path by sheer luck. You see, the URL of the image in Markdown has to include `public` so `rehype-image-size` can find it properly, but it shouldn’t be in the URL passed to the `Image` component:

```js
// components/Image.tsx

import Image from "next/image";

const MyImage = (props) => {
	const { src, alt = "", title = "", height, width } = props;
	const realSrc = src.replace("public", "");

	return (
		<Image
			src={realSrc}
			alt={alt}
			title={title}
			height={height}
			width={width}
		/>
	);
};

export default MyImage;

```

(Because `height` and `width` were added to the `<img>` tag way back when the Markdown was converted to HTML, they’re available as props to this custom component now.)

Pretty clear why the sample blog doesn’t include images in the Markdown itself now, huh?