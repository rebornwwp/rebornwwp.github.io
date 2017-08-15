---
layout: post
title:  "Setup SSH Login With Keygen In OSX"
date:   2016-09-17 15:46:02 +0800
comments: true
tags:
- programming
---
I'm quite tired with copy-pasting passwords from my Evernote every time I  login to my company's remote server. Especially my company uses a proxy server, which means I have to type two passwords to eventually login.


（;≧皿≦）。゜°。


Then I found Keygen tool, which can help us login without typing password.


＼（○＾ω＾○）／


SSH Keygen is to generate a pair of key, putting the public key in your remote server, secret key in your local machine.
If you are also using a Mac, `~/.ssh/id_ras` is the default option of storing secret key, `~/.ssh/id_ras` for public key. You can create many key pairs, and specify which pair to use by adding `IdentityFile` option in your `~/.ssh/config` file.


### Configurations With Single SSH Connection

#### 1. generate keys in your local machine

```
~ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/user/.ssh/id_rsa): [press Enter]
Enter passphrase (empty for no passphrase): [press Enter]
Enter same passphrase again: [press Enter]
Your identification has been saved in /Users/user/.ssh/id_rsa.
Your public key has been saved in /Users/user/.ssh/id_rsa.pub.
The key fingerprint is:
**fingerprint**
The key's randomart image is:
+---[RSA 2048]----+
|    ...          |
|     o . .       |
|    o o + o      |
|     + B B +     |
| .   .= S B o    |
|=o...o.o * o     |
|Bo+oE ... +      |
|=.++ ..  .       |
|o++oooo.         |
+----[SHA256]-----+
```


#### 2. copy public key to your server

If you haven't installed `ssh-copy-id` yet,  use homebrew or Macports to install it.
Then, copy your public key to your remote server, this step needs you type password.

```
~ ssh-copy-id -i ~/.ssh/id_rsa.pub username@servername
```


#### 3. Try login without password
Finally, make sure you can now login without password, and it's done.


ヘ(・、ヘ)ホイホイ(ノ、・)ノホイホイ

```
~ ssh username@servername
Last login: Sat Sep 17 15:00:45 2016 from XXX.XXX.XXX.XXX
```


<hr>
### Configurations for Multiple SSH connections

#### 1. change ssh configuration

If you use a proxy server like my company does, edit `.ssh/config` file to change your ssh configuration (If there is no file named `config` in OSX, create a new one).

```
Host [servername]
ServerAliveInterval 90
ServerAliveCountMax 8
Hostname [username@servername]
User [username]
ProxyCommand ssh [username@proxyname] nc %h %p
```

#### 2. generate key pairs
Same as Step 1 in Single SSH connection do.

#### 3. copy your public key to both your proxy server and your real server
Same as Step 2 in Single SSH connection do.

#### 3. Try login without password
Make sure you can login your real server password, and it's done.


ヘ(・、ヘ)ホイホイ(ノ、・)ノホイホイ
<hr>


#### My Environment
OSX: 10.11.4


iTerm2: 3.0.9
