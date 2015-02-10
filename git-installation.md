---
layout: page
title: Using Git with RStudio -- Installation instructions
---

## Which operating system are you using?

### Linux?

Easy:

```
sudo apt-get install git-core
```

(to be adapted depending on the Linux flavor you use)

### Mac?

If you have Maverick (10.9 or higher), it should already be installed.

If you have an older version, download it from here:
http://git-scm.com/downloads

### Windows?

Download it from here: http://git-scm.com/downloads


## Make sure git is installed, and introduce yourself to git

### Linux & Mac

In a terminal type:

```
which git
```

If it doesn't say: `/usr/bin/git` or something close to this, or it doesn't
return anything, git isn't installed properly.

In your terminal, type:

```
git config --global user.name 'Your Name'
git config --global user.email 'your@email.com'
```

**Be sure to use the same email address as the one you used to sign up to
  GitHub**

Finally, create a SSH key pair if you don't already have one.

To test if you have a key, type in your terminal:

```
cat ~/.ssh/id_rsa.pub
```

if it says `No such file or directory`, we'll create it in RStudio.

It seems that using SSH on Mac doesn't work out of the box, so we are going to
ask git to store our credentials so we don't have to enter them every time. To
do so, follow [these instructions](https://help.github.com/articles/caching-your-github-password-in-git/#platform-mac)

### Windows

Launch "git bash" from your list of programs.

In your terminal, type:

```
git config --global user.name 'Your Name'
git config --global user.email 'your@email.com'
```

You now need to add the path where git is installed.

1. First, double check where git is installed on your computer. In the file
	manager, look for `C:\Program Files (x86)\Git\` (or `C:\Program Files\Git`), and
	locate the `bin\` within.

2. Go to the "Control Panel", and look for "Advanced System Settings". Click on
   the "Environment Variable" button, and in the bottom window, scroll to find
   the "System variable" named "Path". Click on it and push the button
   "Edit". Then edit the "Variable value" by adding a ; and the path of the bin
   folder within Git (e.g., `C:\Program Files (x86)\Git\bin`).

## Let's configure RStudio

Start RStudio, and click on Tools > Global Options. Click on the "Git/SVN" icon
on the right. Make sure that the paths are correct. Try to create the SSH RSA
key.

It's likely that it won't work at first on Windows. Check the path where RStudio
is trying to create it (e.g., `C:\Users\yourname`). Then launch git bash and
type:

```
cd C:\Users\yourname
mkdir .ssh
```

and then try again.

Click on "View key", and upload it on github. See instructions
[here](https://help.github.com/articles/generating-ssh-keys/#step-3-add-your-ssh-key-to-your-account).
