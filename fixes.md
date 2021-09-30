Hey teachers, during the setup for Windows, some steps are not working as they should:

## VS Code connection to Ubuntu

### [Connecting VS Code to Ubuntu](https://github.com/lewagon/setup/blob/master/windows.md#connecting-vs-code-to-ubuntu)

It should connect actomatically to WSL-Ubuntu but it doesn't. To fix this, install manually the extension WSL.
For that, open VS Code and go to the small square on the left that allows to install extensions.
![extensions](images/extensions.png 'Install extensions').


There, search for Remote-WSL and install it (image below for reference).


![install WSL extension](images/installremote-wsl.png 'Install WSL extension')


After that, click on the small computer (menu on the left) to connect Ubuntu to VS Code
![connect Ubuntu](images/connectubuntu.png 'Connect Ubuntu').

Where it says WSL targets, click on the button with a '+' that appears on the right of the name of the Ubuntu distro.
![add Ubuntu](images/ubuntu.png 'Connect Ubuntu').

Then, the magic operates and VS Code will be connected to Ubuntu.



## Issues with ZSH configuration

Also, I have encountered that after the [Dotfiles Configuration](https://github.com/lewagon/setup/blob/master/windows.md#dotfiles-standard-configuration), students get this error while trying to open VS Code (*"Unknown FileSystemError: Error: ELOOP: too many symbolic links encountered"*).
![Unknown FileSystemError: Error: ELOOP: too many symbolic links encountered, open '\wsl$\Ubuntu\home\name.zshrc'](images/toomanylinks.png 'Too Many Symbolic Links')

Or they can come after finished the setup with a screen like this (*"Zshell Config issue- This is the Z Shell configuration function for new users, zsh-newuser-install. You are seeing this message because you have no zsh startup files the files .zshenv, .zprofile, .zshrc, .zlogin in the directory ~."*):


![Zshell Config issue- This is the Z Shell configuration function for new users, zsh-newuser-install. You are seeing this message because you have no zsh startup files the files .zshenv, .zprofile, .zshrc, .zlogin in the directory ~. This function can help you with a few settings that should make your use of the shell easier.](images/zshellconfig.png 'Zshell Config issue')


To escape this screen tap '0' because afterwards we will get rid of this file.

To remove the "too many links issue, they need to run the following in the terminal (Ubuntu (at this point should be the default if entering from Windows Terminal)):

```
rm ~/.zshrc
rm ~/.aliases
rm ~/.gitconfig
rm ~/.irbrc
rm ~/.zprofile
rm ~/.rspec
rm ~/.vscode-server/data/Machine/settings.json
rm ~/.Package\ Control.sublime-settings
```

and then repeat the following commands from the setup.

```
export GITHUB_USERNAME=`gh api user | jq -r '.login'`
echo \$GITHUB_USERNAME
```

You should see your GitHub username printed. *If not, redo the `gh auth` step.* (see below)

Then, check if the student already cloned dotfiles, in that case you can continue with the instalation.

```
cd ~/code/\$GITHUB_USERNAME/dotfiles
zsh install.sh
```

After, run

```
gh api user/emails | jq -r '.[].email'
```

*Before the next step run*

```
git remote -v
```

 if it shows:
`upstream git@github.com:lewagon/dotfiles.git (fetch)`
`upstream git@github.com:lewagon/dotfiles.git (push)`

it is necessary to remove the upstream with:

```
git remote rm upstream
```

and then run the git installer

```
cd ~/code/\$GITHUB_USERNAME/dotfiles && zsh git_setup.sh
```

This will ask you for the student's name (FirstName LastName, without any special characters) and the student's email and the passphrase.

*Please now quit all the opened terminal windows.*

If they open the terminal again and is not looking like it should (the classic terminal of students of Le Wagon) they need to redo all the steps from this section again. And again, and again, until it looks as we use it in Le Wagon.


## gh auth issues.

To fix the issues with "gh auth", for example when the student has this error after running: ` gh api user/emails | jq -r '.[].email' `

error:
` gh: Not Found (HTTP 404)`
` jq: error (at <stdin>:0): Cannot index string with string "email" `

They need to run:

```
gh auth logout
```

Say yes to the prompt asking to confirm that they want to logout.
Then, copy this line

```
gh auth login -s 'user:email' -w
```

without ANY editing.

They need to press ENTER and it will open the browser where they need to put the code that appears in the terminal,
then authorize on the browser that Github have access to the things that it says there, and
go back to the terminal and press ENTER again.

Check with:

```
gh auth status
```

Is ok, if it says Logged in and it says that is using SSH protocol.
Like in the image below.

![Unknown FileSystemError: Error: ELOOP: too many symbolic links encountered, open '\wsl$\Ubuntu\home\name.zshrc'](images/toomanylinks.png 'Too Many Symbolic Links')
