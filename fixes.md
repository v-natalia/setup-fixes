Hey teachers, during the setup for Windows, one step is not working as it should:

## VS Code connection to Ubuntu

### [Connecting VS Code to Ubuntu](https://github.com/lewagon/setup/blob/master/windows.md#connecting-vs-code-to-ubuntu)

It should connect actomatically to WSL-Ubuntu but it doesn't. To fix this you need to install manually the extension WSL.
For that, open VS Code and go to the small square on the left that allows to install extensions,
![extensions](images/extensions.png 'Install extensions').

There, search for Remote-WSL and install it (image below for reference).

![install WSL extension](images/installremote-wsl.png 'Install WSL extension')

After that, you need to click on the computer (menu on the left) to connect Ubuntu to VS Code
![connect Ubuntu](images/connectubuntu.png 'Connect Ubuntu').
Where it says WSL targets, you need to click on the button with a '+' that appears on the right of the name of the Ubuntu distro.
![add Ubuntu](images/ubuntu.png 'Connect Ubuntu').

## Issues with ZSH configuration

Also, I have encountered that after the [Dotfiles Configuration](https://github.com/lewagon/setup/blob/master/windows.md#dotfiles-standard-configuration), students get this error while trying to open VS Code.
![Unknown FileSystemError: Error: ELOOP: too many symbolic links encountered, open '\wsl$\Ubuntu\home\dushveer.zshrc'](images/toomanylinks.png 'Too Many Symbolic Links')

Or they can come after finished the setup with screen like this:
![Zshell Config issue- This is the Z Shell configuration function for new users, zsh-newuser-install. You are seeing this message because you have no zsh startup files the files .zshenv, .zprofile, .zshrc, .zlogin in the directory ~. This function can help you with a few settings that should make your use of the shell easier.](images/zshellconfig.png 'Zshell Config issue')
to escape this screen tap '0' because afterwards we will get rid of this file.

TO remove the "too many links issue, we need to run the following in the terminal (Ubuntu):

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

<details>
  <summary>gh auth issues. </summary>
  <br>
  To fix the issues with `gh auth`:
</details>

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
git remote rm upstream
```

and then run the git installer

```
cd ~/code/\$GITHUB_USERNAME/dotfiles && zsh git_setup.sh
```

This will ask you for your name (FirstName LastName) and your email.
*Please now quit all your opened terminal windows.*

If you open the terminal again and is not looking like it should you need to redo all the steps again. And again, until it looks pretty as we use it in Le Wagon.
