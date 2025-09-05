# Resolve git merge conflicts

## Question

Sarah and Max were working on writting some stories which they have pushed to the repository. Max has recently added some new changes and is trying to push them to the repository but he is facing some issues. Below you can find more details:


SSH into storage server using user max and password Max_pass123. Under /home/max you will find the story-blog repository. Try to push the changes to the origin repo and fix the issues. The story-index.txt must have titles for all 4 stories. Additionally, there is a typo in The Lion and the Mooose line where Mooose should be Mouse.


Click on the Gitea UI button on the top bar. You should be able to access the Gitea page. You can login to Gitea server from UI using username sarah and password Sarah_pass123 or username max and password Max_pass123.

## Answer

- SSH into the storage server
- Change the desired location
```bash
ssh max@ststor01

cd story-blog
```

- Pull the latest changes from remote server
```bash
git pull
```

- Resolve the conflicts(Changes that's not match with my local repo with pulled remote repo. One line of sentence different in 2 files.)
```bash
vi story-index.txt
  > remove "<<<<<<< HEAD" line
  > remove "======" line
  > remove ">>>>>>> <branch-name>"
  > remove line with wrong word "Mooose"
  > remove duplicate lines 
  > after all of those we need to have 04 topics 
```

- Then add the file to commit and commit changes
- push to `origin/master`
```bash
git add story-index.txt

git commit -am "conflicts resolved"

git push origin master
```

## Brief Description About the Environment

**When conflicts happens?**

A Git conflict happens when Git cannot automatically reconcile differences between changes made to the same area of a repository. This most commonly occurs during merges, rebases, or cherry-picks.  

Key situations that cause conflicts:
- **Two branches modify the same lines in the same file** and Git can't decide which change to keep.  
- **One branch edits a file while another branch deletes it.**  
- **During a rebase, when your local commits conflict with upstream commits being replayed.**  
- **When applying patches or cherry-picking commits that touch the same lines as existing code.**

How to detect and resolve:
1. Run the command that triggered the conflict (e.g., git merge feature). Git will stop and mark conflicted files.  
2. Inspect conflicted files — Git marks conflict sections with >>>>>>>, =======, <<<<<<<.  
3. Manually edit files to choose or combine changes, then git add the resolved files.  
4. Finish the operation: for merge use git commit; for rebase use git rebase --continue; for cherry-pick use git cherry-pick --continue.

Quick commands:
- See conflict status: git status  
- List unmerged files: git diff --name-only --diff-filter=U  
- Abort merge/rebase: git merge --abort or git rebase --abort

If you want, tell me the exact command you ran and the conflict markers you see and I’ll give step-by-step resolution for that case.
