---
id: iyrjekgnm084f8hjabat7eh
title: dendronFrontmatter
desc: ""
updated: 1681158677085
created: 1681110151127
---

# Summary

I explore how I figured out Dendron's frontmatter format to make valid Dendron
notes on mobile. I also go over some roadbumps I encountered along the way. The
resulting
[Dendron Frontmatter Generator app](https://6notes.github.io/dendronFrontmatterGen/)
can be used offline.

# Outline

- [Summary](#summary)
- [Outline](#outline)
- [Dendron's frontmatter `id`](#dendrons-frontmatter-id)
- [Dendron's frontmatter `created`/`updated`](#dendrons-frontmatter-createdupdated)
- [Creating an app to use on mobile](#creating-an-app-to-use-on-mobile)
  - [Images not showing up on GitHub Pages](#images-not-showing-up-on-github-pages)
  - [Favicon not showing up on mobile](#favicon-not-showing-up-on-mobile)

# Dendron's frontmatter `id`

This took a while to find. I think the crucial hint was finding
[Dendron Doctor's `fixFrontmatter` command](https://wiki.dendron.so/notes/ZeC74FYVECsf9bpyngVMU/#fixfrontmatter).
In VSCode, if you pull up the Dendron commands, you can pick `Doctor` which
leads to a new set of commands. You'll find the `fixFrontmatter` doctor command.

From here you search `fixFrontmatter` on the
[Dendron GitHub Repo](https://github.com/dendronhq/dendron/search?q=fixFrontmatter),
which leads you to the `FIX_FRONTMATTER` enum key, which leads to
[the Doctor.ts file](https://github.com/dendronhq/dendron/blob/8c7322ce5cea9e31eae057c08a90b4e788e8c542/packages/plugin-core/src/commands/Doctor.ts),
then to
[the Backfill line](https://github.com/dendronhq/dendron/blob/8c7322ce5cea9e31eae057c08a90b4e788e8c542/packages/plugin-core/src/commands/Doctor.ts#L511).
In the
[BackfillService file](https://github.com/dendronhq/dendron/blob/e3dbc9eac5872ef6811b73a1f16baae5fd77dc0a/packages/engine-server/src/backfillV2/service.ts#L15),
you'll find that it edits the frontmatter id with `genUUID`.
[Searching for `genUUID`](https://github.com/dendronhq/dendron/search?q=genUUID)
eventually leads you to
[the uuid file](https://github.com/dendronhq/dendron/blob/9e93cb79448867bbe915a90069dfdd8c1357807d/packages/common-all/src/uuid.ts)
which shows that nanoid is used to generate the id.

# Dendron's frontmatter `created`/`updated`

This was mostly trial and error. I had prior knowledge that dates are stored as
X amount of seconds since a past date, and this format looked similar. It ended
up being `new Date().valueOf()`. I wasn't able to find this in the repo to
verify, but they do look the same and are in the same ballpark. If you find the
difference between the `updated` value when you save now, and
`new Date().valueOf()` in your browser console, the values will be pretty close.

# Creating an app to use on mobile

Going into this, I wanted these features:

- Offline capability which likely meant making a progressive web app (PWA).
- Easy and quick to use, which means being able to load the app and just copy
  the frontmatter to the clipboard.

The main benefit I wanted from using a progressive web app was
[network independence](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)
as I wanted to be able to use the app without being connected to wifi.

I used both
[Melvin George's How to make a quick PWA tutorial](https://melvingeorge.me/blog/nextjs-pwa)
and
[Bigyan Poudel's How to make a quick PWA tutorial](https://articles.wesionary.team/pwa-with-next-js-6073f07a0b26).
The tutorial required favicons so I used
[DALL-E 2](https://openai.com/product/dall-e-2) to generate a generic icon.
Afterwards, I generated favicons with
[Real Favicon Generator](https://realfavicongenerator.net/) to make a NextJS
PWA. To host the website, I used
[Rizèl Scarlett's tutorial on how to host a Hugo or Next.js site on GitHub Pages](https://dev.to/github/how-to-host-a-static-nextjs-site-on-github-pages-4pe0).

## Images not showing up on GitHub Pages

- To make my images show up properly on GitHub Pages, I had to change the image
  paths to be relative.
- For example: `src="/images/next.svg"` to `src="./images/next.svg"`.

## Favicon not showing up on mobile

- The main fix I did was renaming the `manifest.json` file to
  `site.webmanifest`.
