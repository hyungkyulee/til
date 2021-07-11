# Dev Environment
When you set up SSH, you create a key pair that contains a private key (saved to your local computer) and a public key (uploaded to Bitbucket). Bitbucket uses the key pair to authenticate anything the associated account can access. This two-way mechanism prevents man-in-the-middle attacks.

This first key pair is your default SSH identity. If you need more than a default identity, you can set up additional keys.

 * reference: https://confluence.atlassian.com/bitbucket/set-up-an-ssh-key-728138079.html 

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
