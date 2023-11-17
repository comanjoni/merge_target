# Merge two repositories
This repository, along with [merge_source](https://github.com/comanjoni/merge_source), serve as an illustrative example of merging two repositories into one.

## Steps
```
# clone repos
git clone https://github.com/comanjoni/merge_target.git
git clone https://github.com/comanjoni/merge_source.git

# setup source repo
cd merge_source
vim source.txt
git add .
git commit -m "source - init commit"
git push
vim common.txt
git add .
git commit -m "source - pre merge - common"
git push
git branch --all

# setup target repo
cd ../merge_target
vim target.txt
git add .
git commit -m "target - init commit"
git push
vim common.txt
git add .
git commit -m "target - pre merge - common"
git push

# prepare source merging repo if it need some extra modifications that we do not want in master
cd ../merge_source
git checkout -b for_merging
git push --set-upstream origin for_merging
git branch

# prepare target repo for merging before bringing result in target master
cd ../merge_target
git checkout -b merging
git push --set-upstream origin merging
git remote add --fetch merge_source https://github.com/comanjoni/merge_source.git
git merge merge_source/for_merging --allow-unrelated-histories
git diff
vim common.txt
git diff
git add .
git commit
git log
git push

# new features appear in source repo
cd ../merge_source
git checkout master
vim source.txt
vim post_merge_source.txt
git add .
git commit -m "source - post merge 1"
git push
git checkout for_merging
git merge master
git push

# update target repo with new changes from source
cd ../merge_target
git checkout merging
git fetch merge_source for_merging
git merge merge_source/for_merging --allow-unrelated-histories
git log

# merge is complete, remove merge_source remote
git branch
git branch --all
git remote
git remote rm merge_source
git remote
git branch --all
```
