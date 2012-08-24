---
coding: UTF-8
layout: default
title: Bazaar
---

# Bazaar glossary #
http://doc.bazaar.canonical.com/latest/en/user-guide/core_concepts.html
Bazaar wiki: http://wiki.bazaar.canonical.com/[CategoryTerm](http://wiki.bazaar.canonical.com/CategoryTerm)

                             │ No separate repository │ Shared repository   
    ─────────────────────────┼────────────────────────┼─────────────────────
    Local branch only:       │                        │                     
      • without tree         │ standalone branch      │ repository tree     
      • with tree            │ standalone tree        │ repository branch   
    ─────────────────────────┼────────────────────────┼─────────────────────
    Bound remote branch:     │                        │                     
      • with local branch    │ heavyweight checkout   │ repository checkout 
      • without local branch │ lightweight checkout   │ –                   
    ─────────────────────────┼────────────────────────┼─────────────────────
    With ancestor branch     │ stacked branch         │ ?                   
    ─────────────────────────┴────────────────────────┴─────────────────────

## repository ##

Stores revisions of history

* Revisions ids have a random component by default. See _bzrlib.generate_ids.gen_revision_id_ <https://bazaar.launchpad.net/~bzr-pqm/bzr/bzr.dev/view/head:/bzrlib/generate_ids.py>. So the revision id would need to be saved in order to reconstruct it exactly.

## branch ##

Records history leading up to single point (?)

* Has the revision numbers corresponding to development history
* History is stored in a _repository_
* **standalone branch:** Repository and branch stored in same place
* **shared repository** (also \[sometimes?\] simply **repository**): Stores revisions of multiple branches
* **repository branch** (also **sharing branch**): If repository shared with other branches. Lives within a repository. May be nested.
* **stacked branch:** Only stores revisions additional to the branch it is stacked _over_

Bazaar wiki:
[Branch](http://wiki.bazaar.canonical.com/Branch)
[StandaloneBranch](http://wiki.bazaar.canonical.com/StandaloneBranch)
[SharedRepository](http://wiki.bazaar.canonical.com/SharedRepository)
[RepositoryBranch](http://wiki.bazaar.canonical.com/RepositoryBranch)
[StackedBranch](http://wiki.bazaar.canonical.com/StackedBranch)

## working tree ##

User files (for inspecting and generating revisions?)

* Associated with a branch. In general may be stored in a different place to the branch.
* **standalone tree:** The working tree stored with a standalone branch. Repository, branch and working tree all stored in the same place. 
* **repository tree:** Repository branch containing a working tree

Bazaar wiki:
[WorkingTree](http://wiki.bazaar.canonical.com/WorkingTree)
[StandaloneTree](http://wiki.bazaar.canonical.com/StandaloneTree)
[RepositoryTree](http://wiki.bazaar.canonical.com/RepositoryTree)

## checkout ##

Working tree (non-strictly?) _bound_ to a remote branch

* Checkout _of_ the remote branch
* Have local branch(es?)
* (Both local and remote?) branches updated by commits
* Remote branch may be accessed through network
* Commit within a checkout is like commit and push without the checkout status

Bazaar wiki:
[Checkout](http://wiki.bazaar.canonical.com/Checkout)

### lightweight checkout ###

Does not have the local branch(es?)

* Distinguished by calling other checkouts **heavyweight**
* Does not contain a copy of the history
* Most operations are online to remote branch
