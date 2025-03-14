
## version management
### pyenv
install version manager
```
brew install pyenv
```

path configuration
```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

install/see the installed versions, and use one of them
```
pyenv install [version]

pyenv versions

pyenv global (or local or shell) [version]
```
> PIP will be binded together with pyenv automatically

