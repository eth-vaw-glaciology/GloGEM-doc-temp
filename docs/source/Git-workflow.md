# Git workflow
 
## Get a local copy of the repository

This step creates a local copy of the repository in the folder where you are. 
```
$ git clone https://github.com/sdrocer/GloGEM.git
```
## Set the settings correct to contribute and be able to push from any machine

This step gives you the permission to push changes to Github. 

### Option 1 
```
$ git config --global credential.helper store
$ git config --global user.name "[user name from GitHub]"
$ git config --global user.email "[email-adress from GitHub]"
```
- Go to your GitHub account: [Personal Access Token Page](https://github.com/settings/tokens).
- Click "Generate new token".
- Select the scopes your token needs (e.g., repo for repository access).
- Save the token securely (you won't see it again after creation
```
$ git config --global user.password "[token created in the above workflow]"
```

### Option 2
On your machine, set up a private and public ssh key by [following the instructions](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) specific to your operating system.

Under your user settings on the GitHub webpage, create a new SSH key and add the content of the public SSH key you generated on your machine.  

Clone the Git repository using SSH link (instead of HTTPS)

## Editing code

- Change the code
- Write comments
- Upload files

## Create a branch for your work
Get an overview of the changes you made.
```bash
$ git status
```
Create the branch in a specific directory
```bash
$ cd <path_to_repository>
$ git checkout -b <branch_name> 
```

### Configure the location of the branch to be visible in the terminal automatically
```bash
$ XXX
```
List all the different branches
```bash
$ git branch -r
```
Crank away and work! When it is time to commit some changes.
```bash
$ git add path/to/your/file.ext  
$ git commit -m "commit message"
$ git push origin <branch_name>  
``` 
Create a Pull Request (PR) to the main branch using the GitHub UI and ask a teammate to review your changes. After the review is complete, merge the branch in the UI with main. A good practice is to also delete the branch after the merge is complete in the GitHub UI.

## Pull down the merged PR to your local main
```bash
$ git checkout main
$ git pull
```

### Delete the local branch
```bash
$ git branch -d <branch_name>
```

## Rinse and repeat
Repeat the above steps from creating a branch, pushing to master, making a PR, pulling down to your local master, and deleting the branch for every chunk of work your are planning to do.

## More infos on working with git

If you want to get more information on how to work with git, check out this [tutorial](https://github.com/ICESAT-2HackWeek/intro-jupyter-git/blob/master/03-Git-Tutorial.ipynb) by Fernando Perez (UC Berkeley)

## Pulling Changes from Main into a Branch

Follow these steps to ensure your branch is up-to-date with the latest changes from the `main` branch.

### Switch to Your Branch
First, ensure you are working on the branch where you want to pull the changes.
```bash
$ git checkout <branch_name>
```

### Pull changes from the main
```bash
$ git pull origin main
```

### Resolve merge conflicts (if any)
1. Open the conflicting files in your editor.
2. Look for conflict markers like <<<<<<<, =======, and >>>>>>>.
3. Edit the file to resolve the conflict.
4. Mark the file as resolved:
```bash
$ git add <file_name>
```

**Note:** The conflict resolution tool in Visual Studio Code is very helpful for resolving conflicts while maintaining a clear overview.

### Verify the merge
```bash
$ git status
```

### Push your updated branch
```bash
git push origin <branch_name>
```