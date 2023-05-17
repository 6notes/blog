
# Summary

You have a private repo and you want to publish a portion of it without having
to manually copy files between repos.

# Outline

- Set up the repos and accounts
- Set up multiple ssh
- Set up the Github pushing with rsync

# Using git with multiple ssh keys and multiple accounts

- Situation: You want to commit to two different repos with two different
  accounts.
- You can do this by:
  - Generating SSH keys for both accounts.
    - Make sure you don't have a `*` host because it will override the others.
  - Register the public keys for their respective GitHub accounts.
  - [Setup `~/.ssh/config`](https://stackoverflow.com/a/69580744) so that each
    account/GitHub host has its own ssh file.
  - Setup the git origin to have the modified host so that it uses your
    corresponding ssh key.
  - Clone the repos.
  - In each repo, use `git config`s to set the user name and email.

# SSH keys

- If you have
  [a generic `*` host](https://gist.github.com/jexchan/2351996?permalink_comment_id=4286595#gistcomment-4286595),
  you'll need to make it un-generic. This is because regardless of all of your
  settings, having a generic host will supercede everything.
- For notation, I will use `{variable}` to denote variable names.
  - Say you have `url = git@github.com:{workplaceName}/{workRepo}.git` in
    `{workRepo}/.git/config`. If `workplaceName = 'bar'; workRepo = 'chair'`,
    then the url would look like `url = git@github.com:bar/chair.git` and it
    would be in `bar/.git/config`)
- Say you have a cloned work repo with
  `url = git@github.com:{workplaceName}/{workRepo}.git` in
  `{workRepo}/.git/config`, you have another cloned personal repo with
  `url = git@github.com:{personal}/{personalRepo}.git` in
  `{personalRepo}/.git/config`, and a `~/.ssh/config` file with:

  ```{ssh config}
  # Work/All purpose
  Host *
    AddKeysToAgent yes
    UseKeychain yes
    IdentityFile ~/.ssh/id_rsa

  # Personal
  Host github.com-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_personal
  ```

  The default ssh keys for both the work and personal repos will be the work ssh
  key because the `*` host setting takes priority over all of the other hosts.

- Here's what you can do to fix this:

  - Modify the `*` host:
    - Change the host to `work`.
    - Add `User git`.
    - Possibly optional: Add `IdentitiesOnly yes`
  - Modify `workRepo/.git/config`:
    - Change the url to `work:workplaceName/workplaceName.git`.
  - Possibly optional: Modify the `github.com-personal` host:
    - Add `IdentitiesOnly yes`
  - Modify `personalRepo/.git/config`:
    - Change the url to `git@github.com-personal:personalName/personalRepo.git`
  - The updated config file should look like this:

  ```{ssh config}
  Host work
    HostName github.com
    User git
    AddKeysToAgent yes
    UseKeychain yes
    IdentityFile ~/.ssh/id_rsa
    IdentitiesOnly yes

  Host github.com-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_personal
    IdentitiesOnly yes
  ```

- You can verify that the ssh keys are set up properly by doing the following:
  - Use `ssh -T {Host}` for both hosts.
    - You can use `ssh -vT {Host}` instead for a more verbose message for
      debugging.
    - You should get something like: `Hi {sshName}! You've successfully ...`,
      and the ssh names should be different between the repos.

# Git configuration

- Once you're done fixing the ssh keys, you'll have to configure the remote URL
  for git and the user name and email for the repo you're working with. By
  default you will be making commits with the user/email in your global
  `gitconfig`, but you can override this in a specific repo with the following:
  - `git config user.name "Your Name Here"`
  - `git config user.email your@email.com`
  - Use the following to update your git origin remote URL:
    - `git remote add origin git@{personalHost}:{personal}/{personalRepo}.git`
    - `git remote -v`
  - Make local commit to both repos then use `git log` to verify:
    - Make a small change to a file.
    - Do a `git add`.
    - Do a `git commit`.
    - Do `git log` and check that the author name.
    - Do `git reset HEAD~1` to undo the local commit you just made.
    - Undo the small change you made to the file.
    - You should get different author names between the two repos.
- In the future, you can set the `url` in the repo's git config file by
  modifying the git clone text that GitHub provides.
  - Say the text is `git@github.com:{personal}/{personalRepo}.git`. You can
    modify the text to `git@github.com-personal:{personal}/{personalRepo}.git`
    where `github.com-personal` corresponds to the `host` of the ssh key you
    want to use from your `~/.ssh/config` file.
    - This will set the `personalRepo/.git/config` file to use the url
      `git@github.com-personal:{personal}/{personalRepo}.git`.

# Additional notes

- Converting a local vault into a private vault with the Dendron command in
  Visual Studio didn't entirely work for me, so I think it's best to just
  convert it manually.
  - Development of new Dendron features has stopped so it'll probably stay in
    the same state.
- Steps I took for configuring which git account to use for commits in a new git
  repo:
  - `git init`
  - `git remote add origin git@{personalHost}:{personal}/{personalRepo}.git`
  - `git remote -v`
  - `ssh -T {personalHost}`
  - `git config user.name "Your Name Here"`
  - `git config user.email your@email.com`
- [Manual migration of vaults to make them self contained](https://wiki.dendron.so/notes/aikv0yamnfkcowlol7qeldy/#manual-migration-instructions)
  - I used this to figure out what the `.yml` and workspace files should look
    like in the vault that I'm publishing.
  - If you're getting an issue with GitHub Actions being denied, then see
    [`tdy` on checking permissions if GitHub Actions is being denied](https://stackoverflow.com/a/75308228).

# Resources

- [`k-o-d-r` discussing how to use an alias for git cloning](https://gist.github.com/jexchan/2351996?permalink_comment_id=3495142#gistcomment-3495142)
- [`troccoli` discussing how the generic `*` host supercedes all of the other ssh keys](https://gist.github.com/jexchan/2351996?permalink_comment_id=4286595#gistcomment-4286595)
- [`alejandro-martin` discussing setting up two ssh's with the same website (GitHub) ](https://gist.github.com/alejandro-martin/aabe88cf15871121e076f66b65306610#using-two-accounts-from-the-same-server-website-optional)
- [`oanhnn` discussing how to test your ssh key connection](https://gist.github.com/oanhnn/80a89405ab9023894df7)
- [`Sharadh` discussing how to use the verbose version of `ssh -T`](https://stackoverflow.com/a/23730614)
- [`mikemaccana` on setting git remote](https://stackoverflow.com/a/11251797)
- [`Peter Mortensen` on how to list git remote URLs](https://stackoverflow.com/a/10183740)
- [`tdy` on checking permissions if GitHub Actions is being denied](https://stackoverflow.com/a/75308228)
- [Guide to deploying to GitHub Pages normally](https://wiki.dendron.so/notes/FnK2ws6w1uaS1YzBUY3BR/)
