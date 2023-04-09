---
id: riei26e1qagamub98t0zjhj
title: Tips
desc: ""
updated: 1681019784575
created: 1681018685698
---

# Summary

- These are notes to remind me what my workflow is when writing my blog.

# Workflow

- Create new topic/blog post in the blog vault under the blog node.
- Add the post to the root page.

  - Format should be:

  ```{markdown}
  ## YYYY.MM.DD - Title

  ![[dendron.path#firstHeader,1:#*]]
  ```

  - Example:

  ```{markdown}
  ## 2023.04.08 - Twine setup (WIP)

  ![[blog.guides.twine.setup#setup,1:#*]]
  ```

  - I think the `#firstHeader,1:#*` part means from the first header to the next
    header.
  - This was used for
    [the user guide page in Dendron's docs](https://github.com/dendronhq/dendron-site/blob/master/vault/dendron.user-guide.md#basics).

- Push to repo.

# Todo

- Add workflow for mobile.
- For mobile, use
  [the frontmatter generator](https://github.com/6notes/dendronFrontmatterGen)
  to add the frontmatter to the top of the note.
- When we have more notes, we should consider:
  - Separating the notes by year then month with headers
    - Not sure how this works with the right sidebar. I think it's just a flat
      list of all headers.
    - Can also adding year category notes into the sidebar. For example, we
      could just have a `2022` node that's just a list of preview links like the
      homepage.
  - Hiding the blog posts older than the recent ten by using `<details>`
  - Removing older blog posts from the homepage, maybe only show the most recent
    10 blog posts
    - Not a big fan of this idea, should probably try the other methods first.
