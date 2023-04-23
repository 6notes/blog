---
id: jkoetciw8gx345vhbipiijy
title: Setup
desc: ""
updated: 1682217491976
created: 1681014788105
---

# Demo

- [Repo](https://github.com/6notes/tweeExample)
- [Website](https://6notes.github.io/tweeExample/)

# Setup

I will be following
[Emilia's tutorial](https://dev.to/lazerwalker/a-modern-developer-s-workflow-for-twine-4imp)
on how to set up the Twine workflow. I will likely add some modifications as
well.

# Assumptions

- You already have git installed and know how to use git.

# Rough steps for Twine/Tweego and GitHub Pages setup

## Testing locally

- Download the Tweego binary
- Try the Tweego binary on an example `.twee` file

## Setting up a repo with GitHub Pages

- Create the new git repo.
- Make the passage `.twee` files.
- Copy the story formats to your repo from your local Tweego install.
- Set up the workflow action.

# Local Tweego installation

- Install
  [Tweego with Homebrew](https://github.com/potatodiet/homebrew-tweego#how-to-use).
  ```{bash}
  brew tap potato-diet/tweego
  brew install tweego
  ```
- Use `tweego -v` to check that it's installed properly.
- It should be installed in `/usr/local/Cellar`.
  - This is where you can find the story formats.

# Tweego usage

- Example `.twee` file from Emilia's A Modern Developer's Workflow For Twine

  ```{twee}
  ::StoryData
  {
      "ifid": "F2277A49-95C9-4B14-AE66-62526089F861",
      "format": "Harlowe",
      "format-version": "3.1.0",
      "start": "Start"
  }

  ::StoryTitle
  My test story!

  :: Start
  This is the first passage in a Twine game!

  [[This is a link|Next Passage]]

  :: Next Passage
  The player just clicked a link to get here!
  ```

- Testing Tweego output:
  - Change directories to where you want to output the html file.
  - Create the example `.twee` file. Let's say we name it as `test.twee`.
  - Add the example contents above into the `test.twee` file.
  - Create the html output by using `tweego test.twee -o example.html`.
    - The `-o` argument is for the output file name according to
      [the docs](http://www.motoslave.net/tweego/docs/).
      - `-o FILE`
- According to the docs, it looks like Tweego will use all `.twee` files within
  a specified directory. You can use this to split the passages into different
  files.

# CI/CD Github Actions

- I think the `${{ secrets.GITHUB_TOKEN }}` is supplied by the GitHub Actions
  bot so you don't need to add any repository secrets.
- I added the `dist/index.html` output path into repo variables.
  - [Repo variables are in the `vars` context according to `mbaum0`](https://github.com/orgs/community/discussions/42133#discussioncomment-4501954).
- Just like in [[blog.coding.github.multiSSH]], if you're getting an issue with
  GitHub Actions being denied, then see
  [`tdy` on checking permissions if GitHub Actions is being denied](https://stackoverflow.com/a/75308228).
  - I think checking the
    `Allow GitHub Actions to create and approve pull requests` checkbox helps
    the most.

## Notes

- I've been having trouble with trying Go v1.17.x. I think it's because the
  Tweego repo hasn't been updated to be compatible with that version of Go;
  trying the simple suggested fixes like choosing the latest version or
  specifying a version, or specifying the version listed in
  [`go.mod`](https://github.com/tmedwards/tweego/blob/master/go.mod). I think
  it's best to just stick with v1.13.x.

# Resources

- [Tutorial on multiple `.twee` file compiling with Tweego](https://idrellegames.tumblr.com/post/674326647526260736/a-quick-guide-to-using-tweego)
- [A Modern Developer's Workflow For Twine by Em Lazer-Walker](https://dev.to/lazerwalker/a-modern-developer-s-workflow-for-twine-4imp)
- [Using Tweego and VSCode by `JoshuaGrams`](https://github.com/JoshuaGrams/tiny-qbn/blob/master/doc/tweego.md)

# Further areas to explore

- TypeScript
- Importing functions between passages?
- UI
  - Adding UI like health bars or tabs
  - Adding background images
    - AI generated background images based on description text? Could just be
      low resolution if too intensive.
- Adding music/sound effects
- Creating an inventory system; I expect it to be an Object.
  - Story passage that is gated by items.
- Story passage transitions. Like maybe smoke effects before revealing the text.
