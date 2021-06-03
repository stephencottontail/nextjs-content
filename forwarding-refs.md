# Forwarding Refs

---
title: Forwarding Refs
date: May 29, 2021 at 9:39 AM
slug: forwarding-refs
category: nextjs
---

The documentation for [`next/link`](https://nextjs.org/docs/api-reference/next/link#if-the-child-is-a-function-component) says that for function components, “you must wrap the [function] component in `React.forwardRef`” for the link to work with NextJS’s cool prefetching features but the example they show doesn’t use TypeScript. So consider this file:

```js
import Link from "next/link";

interface ForwardedProps {
	onClick: React.MouseEventHandler<HTMLAnchorElement>;
	href: "string";
}

const Foo = React.forwardRef((props: ForwardedProps, ref: React.ForwardedRef<HTMLAnchorElement>) => {
	const { onClick, href } = props;

	return (
		<a onClick={onClick} href={href} ref={ref}>Foo</a>
	);
});

const FooLink = () => {
	return (
		<Link href="/" passHref>
			<Foo />
		</Link>
	);
}

export default FooLink;
```

This code will work fine; the `<a>` tag will have the appropriate `href` attribute and you can click on it and NextJS does all the cool prefetching stuff and everything is all sunshine and rainbows. Except that your IDE will have a nice red squiggle underneath `<Foo />` (well, VSCode will, anyway) and if you hover over it, TypeScript will complain that “Type {} is missing the following properties from type ‘ForwardedProps’: onClick, href”. I got TypeScript to shut up about it by making the properties optional:

```js
interface ForwardedProps {
	onClick?: React.MouseEventHandler<HTMLAnchorElement>;
	href?: "string";
}
```

That kinda feels like a bad idea, though. So what’s the best thing to do here?