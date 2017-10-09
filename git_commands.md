### Git Commands
#### .gitignore
```
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the root TODO file, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/ 

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .txt files in the doc/ directory
doc/**/*.txt”
```

#### $ git diff
```
$ git diff              # what you've changed but not yet staged
$ git diff --staged     # compare your staged changes to last commit (or --cached)
```
#### $ git log
```
$ git log -p -2             # (-p) show differences in each commit 
                            # (-2) last 2 commits
$ git log -stat             # abbreviated stats for each commit
$ git log --pretty=oneline
$ git log --pretty=format:"%h %s" --graph   # show graph of branches and merge history
$ git log --since=2.weeks
$ git log -S{string}            # replace {string} with text added/removed in commits
$ git log --grep={commit_msg}   # replace {commit_msg} with commit messages
$ git log --author={name}
$ git log --committer={name}
```
#### $ git tag
```
$ git tag
$ git tag -l 'v1.0*'
$ git show v1.4                     # show tag information

$ git tag -a v1.5 -m "new release 1.5"
$ git push origin v1.5              # push specific tag
$ git push origin --tags            # push all tags to remote server

$ git checkout -b version-1.5 v1.5  # check out tags
```

#### $ git merge
```
$ git merge feature-a           # merge current branch with feature-a branch
$ git merge feature-a --no-ff   # when fast-forward merge cannot be done,
provide non fast-foward option to resolve the conflicts and merge manually

```

#### $ git rebase
```
$ git rebase HEAD~2       # change current branch to base with the second commit
before HEAD, enter with specific hash number also works.
$ git rebase b503e59

$ git rebase HEAD~2 -i    # -i means interactive mode where we can squash
(combine), drop, and etc.

# List of interactive mode commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
```

