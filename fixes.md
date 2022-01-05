Hey teachers, during the setup for Windows, some steps are not working as they should:


## Not able to enable Hyper-V on Windows Home
The packages required for WSl are not automatically installed.

To download them, please copy the following lines in a text file on desktop and save it with the name `hv.bat`

```
     pushd "%~dp0"
     dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hv.txt
     for /f %%i in ('findstr /i . hv.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
     del hv.txt
     Dism /online /enable-feature /featurename:Microsoft-Hyper-V -All /LimitAccess /ALL
     pause
```
After saving, open the file manager (File Explorer) and right mouse button, click the file and choose run as administrator,
it will open a Terminal window and it will install many packages to enable the features needed.
Once complete it will reboot the machine and then it will be possible to turn on Hyper-v from Programs and Features.



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

You should see your GitHub username printed. **If not, redo the `gh auth` step.** (see below)

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

If they open the terminal again and is not looking like it should (the classic terminal of students of Le Wagon) they need to redo all the steps from this section again. And again, and again, until it looks as we use it in Le Wagon (Actually, I **just** have to re-do many times this with 1 student, for the others it was ok with only 1 time.).


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

![✓ Logged in to github.com as v-natalia (/home/natalia/.config/gh/hosts.yml) ✓ Git operations for github.com configured to use ssh protocol. ✓ Token: ********\*\*\*********](images/loggedin.png 'Logged-in')

## Issues with the support for password authentication.

If a student gets this message:

` “Support for password authentication was removed on August 13, 2021. Please use a personal access token instead. remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information. fatal: Authentication failed for github.com/username/...’” `

The fix is:

```
gh auth status
```

If the result says: ` Git operation for github.com configured to use HTTPS protocol `
Then change it with:

```
gh config set git_protocol ssh
```

Verify with:

```
gh auth status
```
The result should be: ` configured to use ssh protocol `


## Could not access starting directory \\wsl\$\Ubuntu\etc error 0x8007010b

If a student has a black terminal with the message `Could not access starting directory \\wsl\$\Ubuntu\etc error 0x8007010b`

It seems this is a bug on wsl [See here](https://github.com/microsoft/WSL/issues/6995#issuecomment-856286100), but first verify that the student didn't make a mistake during the setup and putting other name on the starting directory.

To fix it, go to the ubuntu terminal, tap `Ctrl+,` and then open the Settings JSON file, then locate the line `"Starting Directory"` as the picture shows, and comment it.

![Starting directory](images/startingdir.png 'Starting Directory')


Then add the line:

```
"commandline": "wsl.exe ~"
```
and save it. It should be fixed the next time they launch the terminal.
