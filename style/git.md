## Git & Github Best Practice

---
### Merge Proposals

* You may merge pull requests for branches you have authored to master after a +1 from a teammate.
* Avoid merging branches you do not own, unless explicitly requested.

#### Rebasing Pull Requests

Please rebase your pull requests against master and squash your changes to a single commit.

This can be achieved with:

```
$ git rebase -i master
```

This will open your text editor with a file listing the commits in your branch that differ from master, e.g.

```
pick 4d1423e neat stuff
pick 5a5353f some seriously snazzy things
pick 1fe3cf2 fixed things
```

Replace "pick" with the word "squash" on every line ***except the first*** (Your editor may provide a nicer interface with keyboard shortcuts for this). The file should now look something like:

```
pick 4d1423e neat stuff
squash 5a5353f some seriously snazzy things
squash 1fe3cf2 fixed things
```

Save and close the file.

You should be presented with a new editor window listing a series of commit messages for these commits. Replace these with a descriptive commit message which explains all of the changes.

As we have changed history, you'll need to force push the branch to update your pull request:

```
$ git push --force
```


