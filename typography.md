# Typography

---
title: Typography
date: May 12, 2021 at 10:54 PM
slug: typography
category: testing
---

## Typography Is Important

I’m making my own version of the `prose` class from Tailwind, mostly for my own edification, so it’s important that I have a good testing base to work from. If you randomly put **bold text** in the middle of a sentence, people will think it means something important.

### Here’s a third-level heading

Here’s some more text following a heading.

- Here’s a list
- It has items
	- Some of them
	- Are nested
- And some of them
- Are not

1. Here’s another list
2. It also has items
	1. Some of them
	2. Are nested
	3. Just like the last one
3. Some of them are not
4. Just like the last one

This paragraph contains a [link to Apple’s website](https://www.apple.com "Apple"). Isn’t that cool? Aren’t links cool? It should have hover, focus, and active states; don’t forget it. Also, I’ve changed my mind and I generally prefer underlined links. Just looks better and it helps links stand out when I use similar link and text colors, which seems to happen a lot with my design sensibilities.

![I drew this](../public/snowy-window.png)

There’s an image. Isn’t it neat? I won’t finish the reference to *The Little Mermaid* because I don’t feel like it.

### A Blockquote

> After looking around for a solution to this coupling, I started finding more and more recommendations towards adding more classes to your markup so you could target them directly; keeping selector specificity low and making your CSS less dependent on your particular DOM structure.
> 
> _Adam Wathan_

## Advanced Typography

Like all great JavaScript developers, I use `console.log()` as my primary debugging tool. I also use one tab (four spaces) for indentation:

```js
import tw, { styled } from "twin.macro";

const Prose = styled.article({
	...tw`p-4 bg-gray-100 text-gray-700`,
	h1: tw`mb-4 font-extrabold text-4xl`,
	h2: tw`mb-4 font-bold text-2xl`,
	h3: tw`mb-4 font-bold text-xl`,
	p: tw`mb-4`,
	a: tw`font-bold text-blue-700 border-blue-700 border-b-2 focus:border-b-4 hover:border-b-4 active:border-b-4 motion-safe:transition-all`
	// remainder of code snipped to save space
});

export default Prose;
```

Incidentally, that’s a snippet of the actual code used to create the `Prose` component that I just created. I don’t know why the triple dots were required before the first use of `tw`, but that’s how it was done in all the `twin.macro` examples so I just went with it. That’s the beauty of being a JavaScript developer in 2021; you don’t need to know what you’re doing, you just copy-and-paste and you’re good. It’s the same reason why things like `create-react-app` and `create-next-app` are popular: you don’t need to understand how to configure `babel` or worse, `webpack`; these tools do it for you.
