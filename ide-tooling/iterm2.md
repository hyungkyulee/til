# iterm2 setup

## install
https://iterm2.com/downloads/stable/iTerm2-3_4_8.zip (or download the latest version from https://iterm2.com/downloads.html)
move the unzipped iterm app to Application folder

## theme configuration
### make it a default theme
iTerm2 -> make iTerm2 default

### install shell integration
iTerm2 -> install shell integration
![image](https://user-images.githubusercontent.com/59367560/120106566-640ebb00-c155-11eb-8c8f-054ff6576dda.png)

### theme applied
1) download dracular theme
```bash
git clone https://github.com/dracula/iterm.git
```
2) iTerm2 > Preferences > Profiles > Colors Tab
3) Open the Color Presets... drop-down in the bottom right corner
4) Select Import the the Dracula.itermcolors file from the downloaded git folder
5) Select the Dracula from Color Presets...
6) other actions > set a default
![image](https://user-images.githubusercontent.com/59367560/120106933-df24a100-c156-11eb-9c55-6fa7a0b51ad7.png)

### font customisation
Manual font installation
Download these four ttf files:

MesloLGS NF Regular.ttf
MesloLGS NF Bold.ttf
MesloLGS NF Italic.ttf
MesloLGS NF Bold Italic.ttf
Double-click on each file and click "Install". This will make MesloLGS NF font available to all applications on your system. Configure your terminal to use this font:

iTerm2: Type p10k configure and answer Yes when asked whether to install Meslo Nerd Font. Alternatively, open iTerm2 → Preferences → Profiles → Text and set Font to MesloLGS NF.
