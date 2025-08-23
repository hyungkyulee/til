## Development Env Setup

Ensure Java and Maven Are Properly Installed
- Check if Java and Maven are correctly installed:
  ```
  java -version
  mvn -version
  ```

- install openjdk and maven
  ```
  brew install openjdk
  brew install maven
  ```

- set the openjdk path
  ```echo 'export PATH="/opt/homebrew/opt/openjdk/bin:$PATH"' >> ~/.zshrc```
  
- Set the JAVA_HOME environment variable
  ```
  echo "export JAVA_HOME=$(/usr/libexec/java_home)" >> ~/.zshrc
  source ~/.zshrc
  ```
  > if this shows any error OR an error to build java project OR package error to import it on dependencies,
    it's because Homebrew installs OpenJDK in /usr/local/opt/openjdk or /opt/homebrew/opt/openjdk, depending on your architecture. You can manually set JAVA_HOME to the OpenJDK path
    - check the openjdk path
    ```
    brew --prefix openjdk
    ```
    - set path with openjdk
    ```
    echo 'export JAVA_HOME=/opt/homebrew/opt/openjdk/libexec/openjdk.jdk/Contents/Home' >> ~/.zshrc
    source ~/.zshrc
    ```

    - if this is not successful, please use intelliJ and set the Project SDK
    <img width="888" alt="image" src="https://github.com/user-attachments/assets/100333b5-9288-43e6-a9e2-9081a404ba38" />







