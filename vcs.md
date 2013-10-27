---
coding: UTF-8
layout: default
title: VCS notes
---

# Selective commit #
* `git commit --patch [-p]`
* `bzr shelve && bzr commit; bzr unshelve`  # Select changes _not_ to commit
    for _shelve_
* `hg record`

# Amend commit #
* `git commit --amend`
* `bzr uncommit && bzr commit`  # Log entry etc are lost; using _gcommit_ or
    _qcommit_ will populate the old log entry

# Author #
* Git: _name_ &lt;_email_&gt;; _email_ must not be blank
* Bazaar: _name_ &lt;_email_&gt;. Uses some sort of check of formatting of
    _email_, otherwise it is just treated as part of _name_.
* Mercurial: `~/.hgrc`: [ui]: username = . . .

# Remote repositories #
* Git: SSH: `USER@HOST:PATH` or “ssh:” URL protocol
* Bazaar: “bzr+ssh:” URL protocol
* Mercurial: “ssh:” URL protocol

# Clone remote repository #
* `git clone [-o origin] USER@HOST:PATH`
* `bzr branch [--standalone] bzr+ssh://USER@HOST/~/PATH`

# Import remote changes #
* `git fetch REPOSITORY --all | SOURCE-REVISION:LOCAL-BRANCH`
* `hg pull URL`

# GUI #
* `gitk --all HEAD -n10000`
* `bzr qlog`
* `bzr visualise`
* `hg view`  # Requires enabling _hgk_ extension
* `hgview`  # QT
