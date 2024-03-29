# Self-signed Certificate Issue
$ git pull
fatal: unable to access 'https://github.com/abcde/[project].git/': SSL certificate problem: self signed certificate in certificate chain

> ref: https://confluence.atlassian.com/kkkkk/resolving-ssl-self-signed-certificate-errors-xxxxx.html
Step 2: Configure git to use the certificate in the windows Trust store
When using Windows, the problem resides that git by default uses the "Linux" crypto backend. Starting with Git for Windows 2.14, you can configure Git to use SChannel, the built-in Windows networking layer as the crypto backend. To do that, just run the following command in the GIT client:

git config --global http.sslbackend schannel
This means that it will use the Windows certificate storage mechanism and you don't need to explicitly configure the curl CA storage (http.sslCAInfo) mechanism. Once you have updated the git config, Git will use the Certificate in the Windows certificate store and should not require http.sslCAInfo setting.



# SSH Configuration
When you set up SSH, you create a key pair that contains a private key (saved to your local computer) and a public key (uploaded to Bitbucket). Bitbucket uses the key pair to authenticate anything the associated account can access. This two-way mechanism prevents man-in-the-middle attacks.

This first key pair is your default SSH identity. If you need more than a default identity, you can set up additional keys.

 * reference: https://confluence.atlassian.com/bitbucket/set-up-an-ssh-key-xxxxx.html 

***

## Setup on Mac / Windows(bash)
### 1. Set up your default identity

Generate the key pair and Enter to accept the default key
```$ ssh-keygen -t ed25519 -C "your_email@example.com"```
> Enter and re-enter to set a passphrase (password). This will ask you to check your credential whenever you do git-related activities
> check the key files in the .ssh folder ```$ ~/.ssh/id_ed25519.pub```

[legacy system not supporting ed25519]
```
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/emmap1/.ssh/id_rsa):
Created directory '/c/Users/emmap1/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/emmap1/.ssh/id_rsa.
Your public key has been saved in /c/Users/emmap1/.ssh/id_rsa.pub.
The key fingerprint is: e7:94:d1:a3:02:ee:38:6e:a4:5e:26:a3:a9:f4:95:d4 emmap1@EMMA-PC
```

```
$ dir .ssh
id_rsa id_rsa.pub
```
> If you already had the key pair, please copy them to the .ssh folder

### 2. Add the key to the ssh-agent
Not asking passcode again in the git commends (pull/push)

Run the agent
```
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

Enter ssh-add command to add the key to the agent
```
$ ssh-add ~/.ssh/id_ed25519

[legacy system]
$ ssh-add ~/.ssh/id_rsa
Enter passphrase for /c/Users/hyungkyu.lee/.ssh/id_rsa:
Identity added: /c/Users/hyungkyu.lee/.ssh/id_rsa (hyungkyu@deepeyes.co.uk)
```

## Add SSH Key to Services
Copy the SSH public key to your clipboard
```
$ pbcopy < ~/.ssh/id_rsa.pub
```

### Github
Paste the key to Avatar > Settings > SSH and GPG Keys > New SSH Key

### Bitbucket
Paste the key to personal settings > SSH Keys > Add Key


## git Alias
Edit ~/.gitconfigure
```
[alias]
  # Show verbose output about tags, branches or remotes
  ci = commit
  st = status
  co = checkout
  br = branch
  tags = tag -l
  branches = branch -a
  remotes = remote -v
  # Pretty log output
  hist = log --graph --pretty=format:'%Cred%h%Creset %s%C(yellow)%d%Creset %Cgreen(%cr)%Creset [%an]' --abbrev-commit --date=relative
```

# Personal Access Token
Background : remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.

## Personal access tokens over https
> ref: https://docs.github.com/en/enterprise-cloud@latest/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token
> ref: https://help.knapsack.cloud/article/83-github-personal-access-tokens-local-dev#:~:text=Under%20the%20System%20Properties%2C%20navigate%20to%20the%20Advanced,button%20For%20%22Variable%20name%22%20use%20%22GITHUB_TOKEN%22%20%28no%20quotes%29
#### Create a personal access token
Developer settings > Developer settings > Personal access tokens > Generate new token 

#### Set details
Description : Organisation Developer Token
Token : [created token]

#### Configures SSO
Configure SSO on the list items which have been invited from the repository admin

## Clone Repository
```
git clone [https repository path]
username : hyungkyulee
password : [personal token]
```
