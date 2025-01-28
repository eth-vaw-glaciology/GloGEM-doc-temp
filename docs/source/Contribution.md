# Guidelines

At the moment the contribution to GloGEM is restricted to **VAW-GL** members, we are working on making it available for everyone.
If you are a **VAW-GL** member, you can contribute following these guidelines.

## Cite GloGEM
If you want to refer to GloGEM in your publications or presentations, please refer to:

**_Huss, M. and Hock, R.: A new model for global glacier change and sea-level rise. Frontiers in Earth Science, 3, 5, https://www.doi.org/10.3389/feart.2015.00054, 2015_**

## Main model code
The main branch on GitHub contains the actual running version of the GloGEM code and is supervised by Dr. Matthias Huss. The repository can be found here:

**_https://github.com/sdrocer/GloGEM.git_**

## How to contribute
All contributions, bug reports, bug fixes, documentation improvements, enhancements and ideas are welcome! The repository of GloGEM is still in an early phase, so most things can be enhanced/corrected/ameliorated by anyone.

The code is hosted on GitHub. To contribute you will need to sign up for a [free GitHub account](https://github.com/signup/free) and get yourself familiar with working with git!

GitHub has instructions for installing git, setting up your SSH key, and configuring git. All these steps need to be completed before you can work seamlessly between your local repository and GitHub. We refer to our page describing the different steps required in [our GitHub workflow page](https://github.com/sdrocer/GloGEM/wiki/Git-workflow). 

For the general usage of Git you can also see some information on [our GitHub workflow page](https://github.com/sdrocer/GloGEM/wiki/Git-workflow).

### General rules
If you want to contribute to GloGEM, please follow these rules: 
* Make a new branch for your development.
* Naming: the branch should be **named by the development** you are working on **not with your name**. Name the discussion channel on our [Discord](https://discord.gg/AgaaJA7z) the exact same way as your branch! The name should be as short as possible but still describe the development as detailed as possible (example: "snow_calibration", or "firnicetemp").
* On the GitHub repository make an issue in the project tracking list, so that everyone knows that this development is in progress. 
* Make sure to regularly synchronize your branch with the main `git push`, especially when you know that changes have been made there. 
* Make sure to test your development (no matter how small or big your change in the code is) with our testing scheme. 
* Once you have tested it, you can create a pull request to merge your branch with the main branch `git push`. Create a Pull Request (PR) to the main branch using the GitHub UI and ask a teammate (GloGEM core group member) to review your changes. After the review is complete, merge the branch in the UI with main.
* Make sure to document your new development in the doc of this repository. 


### Creating a branch
You want your master branch to reflect only production-ready code, so create a feature branch for making your changes. For example:
```
$ git checkout -b <new_branch>
```
This changes your working directory to the <new_branch>. Try to keep any changes in this branch specific to one feature. You can have many <new_branch> and switch in between them using the git checkout command.

To update this branch, you need to retrieve the changes from the master branch:
```
$ git fetch upstream
$ git rebase upstream/master
```
This will replay your commits on top of the latest glogem git master. If this leads to merge conflicts, you must resolve these before submitting your pull request. If you have uncommitted changes, you will need to stash them prior to updating. This will effectively store your changes and they can be reapplied after updating.

### Delete your merged branch
Once your feature/development branch is accepted into upstream, you’ll probably want to get rid of the branch. First, merge upstream master into your branch so git knows it is safe to delete your branch:
```
$ git fetch upstream
$ git checkout master
$ git merge upstream/master
$ git branch -d <new_branch>
```

The branch will still exist on GitHub, so to delete it there too:
```
$ git push origin --delete <new_branch>
```