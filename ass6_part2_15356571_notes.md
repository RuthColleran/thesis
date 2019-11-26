***Installing Git***

- $ sudo apt -get update 
- $ sudo apt -get install git

***Create account***
- $ cd ../thesis

- $ git init
- $ git config --global user.name "RuthColleran"
- $ git config --global user.email "ruth1colleran@hotmail.com"
- $ git status _To follow what Git is doing as you record the initial version of your files_

On branch master

Initial commit

Untracked files:

    (use "git add <file>…" to include in what will be committed)

        analyze.R

        clean.py

        process.sh

nothing added to commit but untracked files present (use "git add" to track)

- $ git add process.sh _Start tracking the file process.sh._
- $ git status _check its new status._


On branch master

Initial commit

Changes to be committed:

    (use "git rm --cached <file>…" to unstage)

        new file: process.sh

Untracked files:

    (use "git add <file>…" to include in what will be committed)

        analyze.R

        clean.py




- $ git add clean.py analyze.R _use git add to begin tracking the other two files and add their changes to the staging area as well._ 
- $ git commit -m "Add initial version of thesis code." _Then create the first commit using the command git commit, flag -m was used to pass a message for the commit. This message describes the changes that have been made to the code and is required.__


[master (root-commit) ae04ecd] Add initial version of thesis code.

3 files changed, 154 insertions(+)

create mode 100644 analyze.R

create mode 100644 clean.py

create mode 100644 process.sh



- $ git log _To view the record of your commits, use the command git log. For each commit, it lists the unique identifier for that revision, author, date, and commit message._


commit ae04ecd4b9f5021e4ebfa57638411d6ac86e5a2c (HEAD -> master, origin/master)
Author: RuthColleran <ruth1colleran@hotmail.com>
Date:   Tue Nov 26 11:30:31 2019 +0000

    Add initial version of thesis code
    
    
- $ tail clean.py 


# Filter based on fold-change over control sample
fc_cutoff = 10
epithelial = epithelial.filter(filter_fold_change, fc = fc_cutoff).saveas()
proximal_tube = proximal_tube.filter(filter_fold_change, fc = fc_cutoff).saveas()
kidney = kidney.filter(filter_fold_change, fc = fc_cutoff).saveas()
# Identify only those sites that are peaks in all three tissue types
combined = pybedtools.BedTool().multi_intersect(
           i = [epithelial.fn, proximal_tube.fn, kidney.fn])
union = combined.filter(lambda x: int(x[3]) == 3).saveas()
union.cut(range(3)).saveas(data + "/sites-union.bed")


- $ nano clean.py _edit clean.py so that the fold change cutoff for filtering peaks is more stringent.(increase the fold change cutoff from 10 to 20)_
- $ tail clean.py


# Filter based on fold-change over control sample
fc_cutoff = 20
epithelial = epithelial.filter(filter_fold_change, fc = fc_cutoff).saveas()
proximal_tube = proximal_tube.filter(filter_fold_change, fc = fc_cutoff).saveas()
kidney = kidney.filter(filter_fold_change, fc = fc_cutoff).saveas()
# Identify only those sites that are peaks in all three tissue types
combined = pybedtools.BedTool().multi_intersect(
           i = [epithelial.fn, proximal_tube.fn, kidney.fn])
union = combined.filter(lambda x: int(x[3]) == 3).saveas()
union.cut(range(3)).saveas(data + "/sites-union.bed")

- $ git status _Because Git is tracking clean.py, it recognizes that the file has been changed since the last commit.The report from git status indicates that the changes to clean.py are not staged, i.e., they are in the working directory._ 


On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   clean.py

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	README.md

no changes added to commit (use "git add" and/or "git commit -a")

- $ git diff  _To view the unstaged changes, run the command git diff.Any lines of text that have been added to the script are indicated with a +, and any lines that have been removed with a -. Here, we altered the line of code that sets the value of fc_cutoff. git diff displays this change as the previous line being removed and a new line being added with our update incorporated_


diff --git a/clean.py b/clean.py
index 9f76681..7f7ccf1 100644
--- a/clean.py
+++ b/clean.py
@@ -28,7 +28,7 @@ def filter_fold_change(feature, fc = 1):
         return False
 
 # Filter based on fold-change over control sample
-fc_cutoff = 10
+fc_cutoff = 20
 epithelial = epithelial.filter(filter_fold_change, fc = fc_cutoff).saveas()
 proximal_tube = proximal_tube.filter(filter_fold_change, fc = fc_cutoff).saveas()
 kidney = kidney.filter(filter_fold_change, fc = fc_cutoff).saveas()

- $ git checkout -- clean.py _undo the edit by following the directions from the output of git status to “discard changes in the working directory” using the command git checkout._
- $ git diff _returns no output, because git checkout undid the unstaged edit you had made to clean.py_
- $ git log 



commit ae04ecd4b9f5021e4ebfa57638411d6ac86e5a2c (HEAD -> master, origin/master)
Author: RuthColleran <ruth1colleran@hotmail.com>
Date:   Tue Nov 26 11:35:42 2019 +0000

    Add initial version of thesis code
    
- $ git checkout ae04ecd clean.py __by default, git checkout restores the most recent version of the file from the staging area (if you haven’t staged any changes to this file, as is the case here, the version of the file in the staging area is identical to the version in the last commit). Instead of using the entire commit identifier, use only the first seven characters__

- $ git remote add origin https://github.com/RuthColleran/thesis _link the local thesis repository on your computer to the remote repository you just created_ 
- $ git push origin master _Send your code to GitHub using the command git push_

- $ git clone https://github.com/RuthColleran/thesis _download the Git repository using the command git clone._
- $ git pull origin master _This pulls in all the commits that you had previously pushed to the GitHub remote repository from your home computer_


Cloning into 'thesis'...
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 5 (delta 0), reused 5 (delta 0), pack-reused 0
Unpacking objects: 100% (5/5), done.

Link to github: https://github.com/RuthColleran/thesis
