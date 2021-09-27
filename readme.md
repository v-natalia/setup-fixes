
# Le Wagon Setup Fixes

Hey teachers, during the setup for Windows, one step is not working as it should:

## [Connecting VS Code to Ubuntu](https://github.com/lewagon/setup/blob/master/windows.md#connecting-vs-code-to-ubuntu)
It should connect actomatically to WSL-Ubuntu but it doesn't. To fix this you need to install manually the extension WSL.
For that, open VS Code and go to the small square on the left that allows to install extensions,
![extensions](images/extensions.png 'Install extensions').

There, search for Remote-WSL and install it (image below for reference).

![install WSL extension](images/installremote-wsl.png 'Install WSL extension')

After that, you need to click on the computer (menu on the left) to connect Ubuntu to VS Code
![connect Ubuntu](images/connectubuntu.png 'Connect Ubuntu')

Also, I have encountered that after the [Dotfiles Configuration](https://github.com/lewagon/setup/blob/master/windows.md#dotfiles-standard-configuration), students get this error while trying to open VS Code.
![too many symbolic links](images/toomanylinks.png 'Too Many Symbolic Links')
Or they can come after finished the setup with screen like this:
![Zshell Config issue](images/zshellconfig.png 'Zshell Config issue')
