+----------------------------------------+
| GIT, GITHUB, GITLAB       (20.12.2023) |
+----------------------------------------+

***************************************
# Instructions to Use GitHub
***************************************

Assume that we have created a directory /Documents/GitHub/. 
This directory must contain all tracked projects.

Variables:
- "name_project": The name of the project and the directory. 
For example can be "nmlib".
- "file": A file created or modified inside name_project.

0. Installation.
The installation is done by an installer (a Git environment).


1. Download a Project From GitHub.

1.1 Clone the project from GitHub, use the command:
	%git clone https://github.com/KenyGeovany/name_project

1.2 In the cloned local directory, create a new file named "file" 
or modify an existing one.

1.3 Use the add command to stage the file for upload.
Use the commit command to add a comment for the staged file. 
Finally, use the push command to upload the changes and the comments.
	%cd GitHub/name_project
	%git add file
	%git commit -m “Name of the change”
	%git push

1.4 Before updating the project, you can check the changes made
in your local project with:
	%git status
This command returns: 
	- The name of the brach we are working(?).
	- The changes per file.
	- The files that are not tracked (e.g., new files).

1.5 It is possible to update all files in he project using the commands:
	%git add .
	%git commit -m “Name of the change”
	%git push

2. First time updating changes with git 

2.1 When you first update your changes using git ad, git commit (and git push?),
You will be prompted to enter your password to access GitHub.
However, entering your GitHub password may result in an error because Git
no longer accepts passwords directly for authentication. 
To solve this issue, you need to create a Personal Access Token (PAT), 
which serves as an alternative to using your GitHub password. 
This token is a unique string of letters and numbers, 
and is created in the webpage of GitHub with following commands:

Settings → Developer Settings → Personal Access Token 
→ Generate New Token (Give your password) → Fillup the form 
→ click Generate token → Copy the generated Token

2.2 To avoid entering the access key each time you update, follow these steps:
	$ git config --global user.name "your_github_username"
	$ git config --global user.email "your_github_email"

2.3 (WARNING: Try to avoid this step if it is possible)
- The following commands may not be necessary:
$ git config -l
$ git config --global credential.helper cache
- The following commands are necessary (TESTED):
	%git config --local --unset credential.helper
	%git config --global --unset credential.helper
- Run nano ~/.git-credentials. Remove the GitHub line and save it.
	%git config --global credential.helper store
(Risky as physically the token is saved in file ~/.git-credentials.
This method saves the credentials in plaintext on your PC's disk. 
Everyone on your computer can access it, e.g. malicious NPM modules.)
- Run git pull and provide the username and password only once.
- Update the changes, will no longer ask us for the access key.

2.4 Create a personal access token to enable actions like git push, cloning, etc.
- Settings > Developer settings > Personal access tokens.
- Create a new password and save it in a secure file. 
The file name could be something like "personal_access_token_2024."
- Once you refresh the page, the token will no longer be accessible.
- Perform a git push. When prompted for the username (MaximoCanul) and password, 
use the generated token as the password. Copy and paste the token when asked.
- This procedure only needs to be done once, until the token expires.

3. Create a new repository in GitHub

3.1 Suggested way:
- Create a new repository in GitHub.
- Fill in the repository name, description, 
- Choose the visibility (public or private).
- Choose a license.
- Follow the steps suggested by the GitHub template. In local project:
	%git init
	%git add .
	%git commit -m "first commit" 
	%git branch -M main
	%git remote add origin <url_github_repository>
	%git push -u origin main

The fifth line adds what is called a "remote" (in this case, origin), 
which allows you to pull and push to the GitHub URL (see section 8.1). 
Note: Can this remote have a deferent name for each project?

If you are working on a local project with both GitHub and GitLab, 
you can avoid interference by using a different remote name for GitLab.
To do this, add a new remote control, for example gitlab or origin_gitlab, with
	%git remote add gitlab <url_github_repository>

3.2 Infalible way:
- Create a new repository in GitHub.
- Fill repository name, description, public, GPL license.
- Clone the remote project to the local repository.
- Copy all files from our project to the local repository /GitHub/name_project.
(At least one file)
- Update using add, commit and push, as in 1.

4. Edit in the GitHub remote project.

4.1 Open the file on GitHub.

4.2 Try to write something. Nothing will happen immediately, 
but an option to open the file in an online editor will appear.	

4.3 Edit the file and press "cmd + S" to save. In the pop-up window,
write your commit message and save the changes.

4.4 Note: The changes will not be updated in your local project. To synchronize, use:
	%git pull origin branch

5. Create and merge a branch

5.1 By default branch, the repository uses a primary branch called "main" or "master". 
We will create a new branch named test:
- Create the branch (locally)
	%git branch test
- Switch to the new branch (locally)
	%git checkout test
- Check the current branch
	%git branch
- Push the branch to the repository (even if there is no changes)
	%git push -f origin test:test_remote
Changing the branch with checkout all files are reloaded.

5.2 Merge the branch with the main branch:
- Switch to the main branch
- Merge the test branch into main
	%git checkout main
	%git merge test
- Delete the branch locally
	% git branch -d test

6. Conflicts. These occur when branches are modified in parallel, 
and you attempt to merge them. For example:
---------------------------------
USR 1:
%git branch hard_branch
%git checkout hard_branch
A file is modified
%git add .
%git commit -m "change 1"
---------------------------------
USR 2:
Some file is modified
%git add .
%git commit -m "change 2"
---------------------------------
USR 1:
%git checkout main
%git merge hard_branch
Appear a conflict asking for
a solution to the user.
Resolve by using a branch
To show the status
%git status -s
Finally comment
%git add .
%git commit -m "resolver conflicto"
---------------------------------

7. Archivos especiales

7.1 (.gitkeep) Is a placeholder file used to ensure that empty directories 
are tracked by Git, as Git does not track empty directories by default.
	%touch dir_empty/.gitkeep

7.2 (.gitignore) This file goes inside a directory 
that we do not want to copy in GitHub.
This solves the problem of the limit size for a file (100MB) 
and a total project (100GB), including all branches.
If you modify the .gitignore file to add untracked directories,
it is possible that does not work (the files are upload to GitHub)
	%git rm -r --cached dir/heavyfiles/
This line will not delete your files, but it is recommended do a backup.
Update the .gitignore:
	dir/heavyfiles/*
	!dir/heavyfiles/.gitkeep
This code add an empty directory "heavyfiles" to the remote repository.
Finally track the .gitignore:
	git add .gitignore
	git commit -m "Stop tracking files in dir/heavyfiles"
	git push %origin test
If arise a conflict with the remote repository, then forces the push (see 8.6)
	%git push --force-with-lease
And then upload the changes:
	%git commit -am "Add dir/heavyfiles"
	%git push

8. Useful Commands

8.1 If you are using both GitLab and GitHub, it’s preferable to assign 
different names to the remote commands. You can do this with:
	%git remote add <remote_name> <url_github_repository>
For example, <remote_name> could be origin for GitHub and origin_glab for GitLab.
View the defined remote:
	%git remote
Delete a remote command:
	%git remote rm <remote_name>

8.2 Show the history of commits:
	%git log --oneline
The alpha-numeric code at the left of the list are the identification code (ID)
of the corresponding commits.

8.3 Restore to a old version of a file:
	%git reset --hard 7bef94c
Where "7bef94c" is the ID of the file.
WARNING: This command deletes all changes made after the restored version.

8.4 Stage and commit changes at the same time using:
	%git commit -am "comment"

8.5 Create a branch and switch to it at the same time using:
	%git checkout -b test

8.6 Force a push with
	%git push --force-with-lease
Keep in mind that forcing the push can affect other collaborators if you're working on a shared branch, so use it with caution.

8.7 If you want to make a modification and include it in the last add/commit 
(without creating a new commit), you can do the following:
	%git add . # To stage the updated changes (verify with git status)
	%git commit --amend --no-edit	
Now you can push.

8.8 To modify the las commit use (NO TESTED)
	%git add --amend
To modify a different commit (not the last) follow the following steps:
- Let b0853b0 be the ID of the commit
	%git rebase -i b0853b0
- A file will open. Change the word "pick" by edit", save and exit. E.g.
	pick b0853b0 Commit message 1 -> edit b0853b0 Commit message 1
- Modify the file(s) you need, then use:
	%git add "file"
	%git commit --amend
Edit the commit, save and exit.
- Continue the rebase process:
	%git rebase --continue
Git will apply the remaining commits on top of the modified commit.
- Update the remote repository
	%git push --force-with-lease		
This will overwrite the history on the remote repository with the new history you've created 
WARNING: (see section 8.6)

8.9 Create Tags (Control Point for finished or tested versions of the project)
To make a tag use the command:
	%git tag 10.08.2024v1.0 -m "Version 1.0 of the project."
Upload the changes to the remote repository
	%git push --tags
If you encounter the following error:
Fatal:tag "10.08.2024v1.0" already exists means other tag with the same name yet exits.
It means a tag with the same name already exists. 
This error can occur when you attempt to modify the message of an existing tag.
To resolve this issue, first delete the tag:
	%git tag -d 10.08.2024v1.0
Create a tag with the same name and push it.

8.10 Rename a project in GitHub.
Go to settings → rename the project. To update the local repository:
	%git remote set-url origin

8.11 Use of fork (Only GitHub)
- Do a fork: fork -> clone -> modify the file -> add, commit, push
  If you are accesing to another count but in the same PC, then first delete the
  credentials, do push and then re-enter with your acount and pasword.
- Pull request: Pull request -> New pull request -> create pull requests ->
  -> rellenar el mensaje -> create pull request.
- Into the owner profile: Ir al pull request -> files changed,
  to see the changes added, then use:
  review changes -> Comment (Stand by)
  		    Approve (Merge)
  		    Request Changes
  -> submit review -> Merge pull request.

***************************************
# Issues, Errors and Questions.
***************************************

1. Updating from MacOS from Big Sur to Monterrey does not update git.
Sol. To solve this problem simply reinstall Xcode-select:
	% xcode-select --install
And later reinit the Git:
	% git init
	% git status
Credits: 
https://stackoverflow.com/questions/52522565/git-is-not-working-after-macos-update-xcrun-error-invalid-active-developer-p (08.08.2024)

0.2.38 Mostrar los commit con la fecha en que se agregó:
git log --oneline  --pretty=format:'%h %ad %s’ 
Mas opciones en https://moraguex.gitbooks.io/trabajando-con-git/content/listar_el_historial_de_commits.html

0.4.49 Problem cloning a project from GitHub: En escénica este problema surge cuando la velocidad de subida de internet es pequeña, por ejemplo 1Mb/s
git clone https://github.com/user/project
Cloning into ‘project’…
remote: Enumerating objects: 1845, done.
remote: Counting objects: 100% (265/265), done.
remote: Compressing objects: 100% (197/197), donelin.
error: RPC failed; curl 92 HTTP/2 stream 0 was not closed cleanly: CANCEL (err 8)
error: 6705 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
Sol. Execute 
	git config --global http.postBuffer 157286400
For an explanation see: https://stackoverflow.com/questions/66366582/github-unexpected-disconnect-while-reading-sideband-packet

0.4.51 Checar cambios entre el último commit y la versión actual previo a hacer el siguiente commit:
Sol. 
	% git diff file.py
Algunos commando de vim funcionan, por ejemplo shift + g. Con
% git diff --stat (guiones dobles)
Se obtiene el número de  lineas agregadas  y eliminadas antes del git add. Si queremos ver el tamaño de los archivos modificados después del git add
Usar: 
	% git diff --cached --name-only | xargs -I{} du -h {}

git ls-files -s | awk '{print $4}' | xargs stat --format="%s" | awk '{total += $1} END {print total, "bytes"}' (No funciona, debería obtener el total)
