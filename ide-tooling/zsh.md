# zsh customisation

## oh-my-zsh installtion
https://ohmyz.sh/
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

echo $SHELL
```

## p10k theme installation
1) install the font : https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k
  - Download these four ttf files:
    MesloLGS NF Regular.ttf
    MesloLGS NF Bold.ttf
    MesloLGS NF Italic.ttf
    MesloLGS NF Bold Italic.ttf
  - Double-click on each file and click "Install". This will make MesloLGS NF font available to all applications on your system. Configure your terminal to use this font:
  > set the font on the item text
  > ![image](https://user-images.githubusercontent.com/59367560/120310606-795b2500-c2ce-11eb-9630-976e9082c707.png)

3) p10k theme configuration
```zsh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
```
> relaunch the iterm

![image](https://user-images.githubusercontent.com/59367560/120108830-bd2f1c80-c15e-11eb-9083-315e1230e737.png)

> https://github.com/romkatv/powerlevel10k#manual


