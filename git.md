# Git

Much of this was taken from:

* [Git Style Guide](https://github.com/agis-/git-style-guide)
* [How to write a commit message](http://chris.beams.io/posts/git-commit/)
* [Pro Git](http://git-scm.com/book/en/v2)

### Branches

* Every new feature should be build on a separate branch

* Choose *short* and *descriptive* names:

  ```shell
  # good
  $ git checkout -b oauth-migration

  # bad - too vague
  $ git checkout -b login_fix
  ```

* Use *dashes* to separate words.

* Delete your branch from the upstream repository after it's merged (unless
  there is a specific reason not to). 

Tip: Use the following command while being on "master", to list merged
  branches:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

### Commit

* Avoid merge commits, instead use git rebase when possible

* Each commit should be a single *logical change*. Don't make several
  *logical changes* in one commit. For example, if a patch fixes a bug and
  optimizes the performance of a feature, split it into two separate commits.

* Don't split a single *logical change* into several commits. For example,
  the implementation of a feature and the corresponding tests should be in the
  same commit.

* Commit *early* and *often*. Small, self-contained commits are easier to
  understand and revert when something goes wrong.

* Commits should be ordered *logically*. For example, if *commit X* depends
  on changes done in *commit Y*, then *commit Y* should come before *commit X*.

### Messages


* It is generally a good idea to use the editor and not the terminal, when writing a commit message:

  ```shell
  # good
  $ git commit

  # bad
  $ git commit -m "Quick fix"
  ```

  Committing from the terminal encourages a mindset of having to fit everything
  in a single line which usually results in non-informative, ambiguous commit
  messages.

* The summary line (ie. the first line of the message) should be
  *descriptive* yet *succinct*. Ideally, it should be no longer than
  *50 characters*. 

* The subject should be capitalized and written in imperative present
  tense.

* The subject should not end with a period since it is effectively the commit
  *title* (besides space is precious when you're trying to keep them to 50 chars or less):

  ```shell
  # good - imperative present tense, capitalized, fewer than 50 characters
  Mark huge records as obsolete when clearing hinting faults

  # bad
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* Separate subject from body with a blank line

* After that should a more thorough description. It should be wrapped to *72 characters* and explain *why* the change is needed, *how* it addresses the issue and what *side-effects* it might have.

  It should also provide any pointers to related resources (eg. link to the
  corresponding issue in a bug tracker):

  ```shell
  Short (50 chars or fewer) summary of changes

  More detailed explanatory text, if necessary. Wrap it to
  72 characters. In some contexts, the first
  line is treated as the subject of an email and the rest of
  the text as the body.  The blank line separating the
  summary from the body is critical (unless you omit the body
  entirely); tools like rebase can get confused if you run
  the two together.

  Further paragraphs come after blank lines.

  - Bullet points are okay, too

  - Use a hyphen or an asterisk for the bullet,
    followed by a single space, with blank lines in
    between

  Source http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```

  Ultimately, when writing a commit message, **think about what you would need
  to know if you run across the commit in a year from now.**

* If a *commit A* depends on another *commit B*, the dependency should be
  stated in the message of *commit A*. Use the commit's hash when referring to
  commits.

## Merging

* **Never ever rewrite the history of the "master" branch** or any other
  special branches (ie. used by production or CI servers). The repository's history is valuable in its own right and it is very important to be able to tell *what actually
  happened*.

* The following are the *only* cases where rewriting history is legitimate:

  * You are the only one working on the branch and it is not being reviewed.

  * You want to tidy up your branch (eg. squash commits) and/or rebase it onto
    the "master" in order to merge it later.

  That said, 

* Keep the history *clean* and *simple*. *Just before you merge* your branch:

    1. Make sure it conforms to the style guide and perform any needed actions
       if it doesn't (squash/reorder commits, reword messages etc.)

    2. Rebase it onto the branch it's going to be merged to:

       ```shell
       [my-branch] $ git fetch
       [my-branch] $ git rebase master
       # then merge
       ```

       This results in a branch that can be applied directly to the end of the
       "master" branch and results in a very simple history.

       *(Note: This strategy is better suited for projects with similar branches. Otherwise it might be better to occassionally merge the "master" branch instead of rebasing onto it.)*

