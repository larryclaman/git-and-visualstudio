# Resolve merge conflicts

When you [merge](pulling.md) one branch into another, file changes from commits in one branch can conflict with the changes the other.
Git attempts to resolve these changes by using the [history](review-history.md) in your repo to determine what the merged files should look like.
When it isn't clear how to merge changes, Git halts the merge and tells you which files conflict. 

In this tutorial you learn how to:

> * Understand merge conflicts
> * Resolve merge conflicts

## Understand merge conflicts

The following image shows a very basic example of how changes conflict in Git. Both the master and bugfix branch make updates to the same lines of source code.

![Master and bugfix branch have changes that conflict](media/merge-conflict.png)    

If you try to merge the bugfix branch into master, Git can't determine which changes to use in the merged version. You may want to keep the changes
in the master branch, the bugfix branch, or some combination of the two. Resolve this conflict with a merge commit on the master branch
that reconciles the conflicting changes between the two branches.

![Create a merge commit to resolve the conflict between the two branches](media/merge-conflict-resolved.png)

The most common merge conflict situation is when you pull updates from a remote branch to your local branch, for example from `origin/bugfix` into your local `bugfix` branch.
Resolve these conflicts in the same way - create a merge commit on your local branch reconciling the changes and complete the merge.

### What does Git do to prevent merge conflicts?

Git keeps an entire history of all changes made in your repo. Git uses this history as well as the relationships between commits to see if it can order the changes and resolve the merge automatically. 
 Conflicts only occur when it's not clear from your history how changes to the same lines in the same files should merge.

### Preventing merge conflicts

Git is very good at automatically merging file changes in most circumstances, provided that the file contents don't change dramatically between commits.
Consider [rebasing]([rebase.md](https://docs.microsoft.com/en-us/azure/devops/repos/git/rebase?view=azure-devops&tabs=visual-studio)) branches before you open up a [pull request](pullrequest.md) if your branch is far behind your main branch.
Rebased branches will merge into your main branch without conflicts.

## Resolve merge conflicts 

#### Lab Exercise 

1. First you'll need to manually force a merge conflict.  From your browsers, navigate to "Repo -> Files", then drill down to the file src/PartsUnlimitedWebsite/Startup.cs, and then press 'Edit'.  Insert a new first line as follows, and commit your change.
   > // Online


2. Next, switch back to Visual Studio and open the same file (make sure you're working in the **master** branch), and insert a new first line that looks like:
   > // Visual Studio

3. Save the file, then stage and commit it (just like in earlier eercises)
3. Press 'Sync', and you'll be informed of the merge conflict(s) when you pull changes or attempt to merge two branches.   
4. The conflict notification appears. Click the **Conflicts** link to start resolve file conflicts.   

   ![Prompt when there is a merge conflict when you pull a change](media/merge_prompt_vs.png)   

5. This will bring up a list of files with conflicts. Selecting a file lets you accept the changes in the source branch you are merging from with the **Take Source** button or accept the changes in the branch you are merging into using **Keep Target**. 
   You can manually merge changes by selecting **Merge**, then entering the changes directly into the merge tool specified in your [Git settings](git-config.md#diff--merge-tools).
6. Use the checkboxes next to the lines modified to select between remote and local changes entirely, or edit the results directly in the **Result** editor under the **Source** and **Target** editor in the diff view.   
7. When done making changes, click **Accept Merge** . Repeat this for all conflicting files.
8. Open the **Changes** view in Team Explorer and commit the changes to create the merge commit and resolve the conflict.

   ![Resolving Merge Conflicts in Visual Studio](media/vsmerge.gif)  

    Compare the conflicting commits as well as the differences between the common history with the options in Visual Studio's merge tool.   

    ![VSMergeTool comparison options](media/vsmergeoptions.png)

#### Command Line -- FYI
Resolve merge conflicts on the command line:   

1. (Optional) Before performing any `pull` or `merge`, make sure that your repo is clean with `git status`. 

    ```
    > git status
    On branch myfeature
    nothing to commit, working directory clean
    ```

2. Perform your `pull` or `merge`. Use `git status` to see exactly which files did not merge properly.

    ```
    > git pull origin myfeature

    Auto-merging serverboot.js
    CONFLICT (content): Merge conflict in serverboot.js
    Automatic merge failed; fix conflicts and then commit the result
    ```

3. (Optional) Check the commit logs to find the commits that conflict with your own using `git log --merge`. 

    ```
    > git log --merge
    commit fac422e78f105ccb44b50a00fc82d6ea89b15513
    Merge: 9b28b1e 1dd2603
    Author: Francis Totten frank@fabrikam.com

        merging new api endpoint
    ```

4. Update the conflicted files listed in `git status`. Git adds markers to files that have conflicts. These markers look like:   

    ```
    <<<<<<< HEAD
    console.log("Writing changes to dev console");
    =======
    debug("Writing changes to debug module);
    >>>>>>> dev-updates
    ```

    The `<<<<<<<` section are the changes from one commit, the `=======` separates the changes, and `>>>>>>>` for the other conflicting commit.   

5. Edit the files so that they look exactly how they should, removing the markers. Use `git add` to stage the resolved changes.
6. Resolve file deleting conflicts with `git add` (keep the file) or `git rm` (remove the file).
7. If performing a merge (such as in a `pull`), commit the changes. If performing a rebase, use `git rebase --continue` to proceed.

    ```
    > git add serverboot.js
    > git commit -m "Resolved both new api endpoints"
    ```

* * *
