# Update local list of remote branches
`git remote update origin --prune`

From <https://stackoverflow.com/questions/36358265/when-does-git-refresh-the-list-of-remote-branches> 


# Add "upstream" remote
```
git remote -v
git remote add upstream git@github.com:paretech/notes.git
git remote -v
```

From <https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/configuring-a-remote-repository-for-a-fork>


# Fix fork sync issue
```
git checkout main
git fetch upstream
git reset --hard upstream/main
git push --force
```

From <https://github.com/orgs/community/discussions/22440> 


# Working with Upstream Repo
Complete section "Add Upstream Remote" first

After merging a pull request from fork to upstream you will want to update your local main.

```
git fetch upstream
git checkout main 
git rebase upstream/main
```

From <https://www.atlassian.com/git/tutorials/git-forks-and-upstreams> 


# Securely remove file
See GitHub documentation for [Removing sensitive data from a repository](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository).

Be warned, [`git-filter-repo` will remove remotes](https://github.com/newren/git-filter-repo/issues/46) in the process.

If running on Windows, becareful with `PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA`. It may fail silently. Always confirm files actually removed.

```
python git-filter-repo.py --invert-paths --path PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA
git remote add origin git@github.com:paretech/notes.git && git fetch origin
git push origin --force --all
```


# Submodules

- https://git-scm.com/book/en/v2/Git-Tools-Submodules
- https://www.atlassian.com/git/articles/core-concept-workflows-and-tips
- https://www.atlassian.com/git/tutorials/git-submodule
- https://gchp.readthedocs.io/en/latest/reference/git-submodules.html

# Subtrees
