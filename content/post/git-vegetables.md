---
title: "How a garden in your git patch improves readability"
date: 2020-02-29T14:01:20-06:00
draft: false
author: "Landon Bouma"
cover: "img/git-veggie-patch-garden.png"
description: "Do you sometimes struggle to find the start of a hunk when using `git add -p`?
Add a colorful vegetable garden between interactions, to make patch hunks easier to spot."
---

As I commit changes, I like to double-check my work.

This entails running an interactive patch session with `git add -p` .

But I sometimes struggle to find the start of each hunk.

- A block of text will scroll by, and I'll page up, looking for the start
  of the hunk (marked by "Stage this hunk"), but it can be hard to find.

  It's a different color, for sure, but otherwise it does not stand
  out from surrounding lines.

With a little shell magic, we can inject line breaks between chunks.

Or, more creatively, we can plant a colorful garden between interactions.

Here's an animation to show how it works, before and after making the change:

{{< image src="/assets/post-git-veggie-patch-how-it-works.gif" alt="How it works" position="left" style="border-radius: 8px; margin: 2em 0em; padding: 4px; border: 2px solid white;" >}}

-------

If this feature intrigues you, check out the 
[git-veggie-patch](https://github.com/landonb/git-veggie-patch)
project.

First, install the `diff-filter-garden` Bash script to your user's `PATH`.

- For instance, I install local applications and simple scripts
  to `~/.local/bin`, so I have `export PATH=$HOME/.local/bin:$PATH`
  [in my user's](https://github.com/landonb/home-fries/blob/master/lib/paths_util.sh#L113-L118)
  `~/.bashrc` file.

  One option is to download the script to such a path, e.g.,:

  ```shell
  wget -O ~/.local/bin/diff-filter-garden \
    https://raw.githubusercontent.com/landonb/git-FlU/master/bin/diff-filter-garden
  ```

  Another option (which is what I like to do when developing on a project)
  is to clone the project and figure it out from there. I might make
  a symlink to the file from `~/.local/bin`, or I might update `PATH`
  with the location of the cloned project's `bin/`.

Next, add a few lines to your `~/.gitconfig`.

- If you want to edit the config manually, add two these lines:

  ```ini
  [interactive]
    diffFilter = diff-filter-garden
  ```

  Or, if you want to update the config from the command line, run:

  ```shell
  git config --global interactive.diffFilter diff-filter-garden
  ```

  Or, if you just want to run the filter on a per-command basis, try:

  ```shell
  git -c interactive.diffFilter='diff-filter-garden' add -p
  ```

Note that git will only use `diffFilter` if color is enabled.

- For instance, `git -c ui.color=never add -p` will bypass the filter.

  If you're curious to see how git uses the filter internally,
  [read the source](https://github.com/git/git/blob/de93cc14ab7e8db7645d8dbe4fd2603f76d5851f/add-patch.c#L416-L431).

Many thanks to the ASCII "Flower garden" artist,
[Joan G. Stark](https://www.asciiart.eu/plants/flowers)!
[jgs]

- I swapped ASCII punctuation for Unicode to avoid escaped escapes
  (`\|/` → `╲│╱`); and I added random flower colors.

Lastly, your garden might look different than mine! If you want
the same look as in the images above, try my favorite terminal
and text editor font, [Hack](https://sourcefoundry.org/hack/).

- I use [Hack](https://sourcefoundry.org/hack/) Regular 9 in Vim,
  and size 10 in `mate-terminal` -- though not on this web page,
  which instead uses the equally venerable, albeit quirkier
  [Fira Code](https://github.com/tonsky/FiraCode).

If you have any suggestions or improvements, please submit a
[pull request](https://github.com/landonb/git-veggie-patch/pulls).
Enjoy!

