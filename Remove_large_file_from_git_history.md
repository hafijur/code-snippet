## Remove Large Files from Git History

Since GitHub has rejected the push, the large files must be removed from your Git history. Use the git filter-repo (preferred) or BFG Repo-Cleaner to remove them.

1. **Install Git Filter-Repo**
    ```
    sudo apt install git-filter-repo
    ```
2. **Remove the Large Files: Run the following command to remove campusmanagment.zip**

    ```
    git filter-repo --path campusmanagment.zip --invert-paths
    ```
    This command completely removes campusmanagment.zip from the repository history.
    

3. **Verify Large Files Are Removed: Run:**
    ```
    git log -- campusmanagment.zip
    ```
    If it shows no history, the file has been removed successfully.