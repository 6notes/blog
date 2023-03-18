---
id: 3z5di4v1qfglionxzng5nwm
title: pushSettings
desc: ''
updated: 1677486470947
created: 1677486458905
---

# (WIP) Changing `git push` settings to be less tedious

- I think the default `git push` settings are annoying because you always have
  to specify the origin or upstream stuff.
  - I think if you set `default = current` under `[push]` in your global
    `.gitconfig` file, you can just git push to whatever your current repo's
    branch is. I think it's more convenient and haven't run into any problems
    yet.

# Resources

- [`git push` documentation that explains the different `push.default` settings](https://git-scm.com/docs/git-push#_configuration)
