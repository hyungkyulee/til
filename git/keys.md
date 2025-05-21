

## Role of both Keys : GPG Key vs SSH Key
	•	You need a GPG key if you want to sign your commits so GitHub shows the “Verified” badge.
	•	You need an SSH key if you want to interact with GitHub securely, e.g., cloning, pushing, pulling via SSH.
	•	They serve completely different purposes.
__
Use Case | Requires GPG Key | Requires SSH Key
Sign Git commits | ✅ Yes | ❌ No
Verify commit authorship on GitHub | ✅ Yes | ❌ No
Clone/push over SSH | ❌ No | ✅ Yes
Authenticate with GitHub (e.g., git push) | ❌ No | ✅ Yes
GitHub Actions deploy keys | ❌ No | ✅ Yes (or PAT/OIDC)

## Email accounts
you do not have to use the noreply email when generating a GPG key — but here’s when and why you might want to.

Use noreply email if:
	•	You want to keep your real email private.
	•	You’re using GitHub commit signing but don’t want your email leaked in your commits.
	•	You set “Keep my email addresses private” in your GitHub profile settings.

Use real email if:
	•	You don’t care about privacy.
	•	You want to match the email used in past commits.
	•	You’re signing commits made outside GitHub (e.g., enterprise Git servers, internal tooling).

> Important: Match GPG Email with Git Config
> GitHub will only verify signed commits if:
>   •	The GPG key’s email matches your Git user.email, and
>   •	That email (noreply or real) is linked to your GitHub account

check the current email on git config
```
git config --global user.email
```

## Re-generate GPG keys from the scratch
delete all
```
mv ~/.gnupg ~/.bak_gnupg_$(date +%s)
```

Start GPG (and check version)
```
gpg --version
```

create GPG directory
```
mkdir -p ~/.gnupg
chmod 700 ~/.gnupg
```

restart agent
```
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent
```


```
gpg --full-generate-key                                  х INT 13s 18:53:27
gpg (GnuPG) 2.4.7; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection?
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (4) NIST P-384
   (6) Brainpool P-256
Your selection?
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: Kyu Lee
Email address: [email address]
Comment: GPG Key for kyu@[my compay]
You selected this USER-ID:
    "Kyu Lee (GPG Key for kyu@[company]) <[email address]>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: agent_genkey failed: No such file or directory
Key generation failed: No such file or directory
```

## Add the GPG key to github
check the private / public key
```
gpg --list-secret-keys --keyid-format=long

gpg --list-keys --keyid-format=long
```

export key value to copy it to github GPG section
```
gpg --armor --export XXXXX                              20:27:23
-----BEGIN PGP PUBLIC KEY BLOCK-----

mDME
:
:
:
HW
-----END PGP PUBLIC KEY BLOCK-----
```
> https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account

## Telling Git about your signing key
To sign commits locally, you need to inform Git that there's a GPG, SSH, or X.509 key you'd like to use.


> If you have previously configured Git to use a different key format when signing with --gpg-sign, unset this configuration so the default format of openpgp will be used.
```
git config --global --unset gpg.format
```

From the list of GPG keys, copy the long form of the GPG key ID you'd like to use
```
git config --global user.signingkey [long form key id]
```
> If you use multiple keys and subkeys, then you should append an exclamation mark ! to the key to tell git that this is your preferred key. Sometimes you may need to escape the exclamation mark with a back slash: \!.

Optionally, to configure Git to sign all commits and tags by default, enter the following command:
```
git config --global commit.gpgsign true
git config --global tag.gpgSign true
```

check the registered signing key
```
git config --global user.signingkey
```


## commit with signing key
```
❯ git commit -S -m "xxx - signed commit"               20:45:46
[pplan-permission 183b6b7]   signed commit
 1 file changed, 1 insertion(+), 1 deletion(-)
```
> enter the passphrase on commit


## Disable Commit Signing Temporarily If you want to bypass signing for now:

- Temporal disable
```
git commit --no-gpg-sign
```

Or disable it globally:
```
onfig --global commit.gpgsign false
```

