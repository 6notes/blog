---
id: zuldcwad3qhu690rvnxeyok
title: fixAddressBarBlock
desc: ''
updated: 1683397577228
created: 1683397001058
---

# Problem

You're building a website with a `100vh` style but when you try it on the mobile
version of Safari, the bottom address bar blocks the rest of your website.

# Solution

- Use `svh` (small viewport height) or `dvh` (dynamic viewport height) instead
  of just `vh`
- [Webkit 15.4 patch notes](https://webkit.org/blog/12445/new-webkit-features-in-safari-15-4/#solving-pain-points)
- ![](/assets/images/newViewportUnits.png)
