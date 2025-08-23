# React-Native with Typescript and Tailwind
## Dev Environment
### Configure basic toolings
```
# install homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# install node and watchman
brew install node
brew install watchman
```

### Configure Android Dev Kit
```
# jdk 8.x
brew install --cask adoptopenjdk/openjdk/adoptopenjdk8

# install android sdk or android studio

# configure env variables
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

### Creat a new RN project with typescript template
```
npx react-native init AwesomeTSProject --template react-native-template-typescript
```
> if there is an error to install the template, react-native-cli version might be out-dated.
> ![image](https://user-images.githubusercontent.com/59367560/147587140-f5820099-db53-407a-9a95-41658c811551.png)
> remove react-native-cli

> ![image](https://user-images.githubusercontent.com/59367560/147587334-7901bc62-3116-4677-b49f-d679e3e64eae.png)
> install a new CLI globally
> ![image](https://user-images.githubusercontent.com/59367560/147587444-c29ec013-7285-43c2-996b-ee58c161050a.png)
> ```
> npm uninstall -g react-native-cli
> yarn global add @react-native-community/cli
> ```

![image](https://user-images.githubusercontent.com/59367560/147661449-d900b3f0-44d0-495c-8119-98f681ca7180.png)
