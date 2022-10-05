# MacOS Reset and Dev Settings

## Factory Reset
### Create a backup
backup your data and photo to other location or machine

### Sign out of iCloud and iMessage
> Sign out of iTunes in macOS Mojave or earlier
- Account > Authorisations > Deauthorise This Computer. 
Then enter your Apple ID and password and click Deauthorise.

- Apple menu  > System Preferences, then click Apple ID. Select Overview in the sidebar, then click Sign Out.
- Messages > Preferences from the menu bar. Click iMessage, then click Sign Out.

### Reset NVRAM
This will clear user settings from the memory and restore certain security features that may have been altered previously.

1. Shut down your Mac, 
2. turn it on and immediately press and hold these four keys together: 
   > Option, Command, P and R. Release the keys after about 20 seconds.

### Erase your hard drive and reinstall macOS
> ref: https://support.apple.com/en-gb/HT208496

1. turn on your Mac, 
2. then immediately press and hold Command (⌘) and R until you see an Apple logo or other image. 
  > If asked, select a user you know the password for, then enter their administrator password. 
3. From the utilities window, select Disk Utility and click Continue.
4. Select Macintosh HD in the sidebar of Disk Utility.
5. Click the Erase button in the toolbar, then enter the requested details:
  - Name: Macintosh HD
  - Format: APFS or Mac OS Extended (Journaled), as recommended by Disk Utility

6. Click Erase Volume Group. If you can't see this button, click Erase instead.
7. If asked, enter your Apple ID.
8. After the erase process has been completed, select any other internal volumes in the sidebar, then click the delete volume (–) button in the toolbar to delete that volume.
9. Quit Disk Utility to return to the utilities window
10. select Reinstall macOS in the utilities window
11. click Continue and follow the onscreen instructions to reinstall macOS.

## Basic Dev Env
### Add Accounts
 > System Preferences > Internet Accounts > + > Add email accounts

### Install Browsers
Chrome

### Install IDEs
#### Rider
#### VS Code
#### VS for Mac

### Brew
> ref: https://brew.sh/

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

....


echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/kyu/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/kyu/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
...

$ brew --version
Homebrew 3.x.x
Homebrew/homebrew-core ..........
```

### iterm2
https://github.com/hyungkyulee/til/blob/main/ide-tooling/iterm2.md

### on-my-zsh
https://github.com/hyungkyulee/til/blob/main/ide-tooling/zsh.md

### git
https://github.com/hyungkyulee/til/blob/main/git/setup.md

```
brew install git
```

### node & package manager(e.g. npm/yarn)
```
brew install node
npm i -g yarn
```

## Mobile Development
#### XCode
#### Android Studio

