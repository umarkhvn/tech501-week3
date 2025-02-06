- [Using SSH Instead of HTTP for GitHub](#using-ssh-instead-of-http-for-github)
  - [Overview](#overview)
- [Steps to Use SSH for GitHub Authentication](#steps-to-use-ssh-for-github-authentication)
  - [1. Generate SSH Key Pair](#1-generate-ssh-key-pair)
  - [2. Register Public Key on GitHub](#2-register-public-key-on-github)
    - [**On GitHub:**](#on-github)
    - [**In the Local Terminal:**](#in-the-local-terminal)
  - [3. Register Private Key with SSH Agent](#3-register-private-key-with-ssh-agent)
- [Creating a Test Repository (on GitHub and Locally)](#creating-a-test-repository-on-github-and-locally)
  - [**On GitHub:**](#on-github-1)
  - [**In the Local Terminal:**](#in-the-local-terminal-1)


# Using SSH Instead of HTTP for GitHub

## Overview
By default, developers often use **HTTPS** to authenticate and push a local repository to GitHub.  
However, using **SSH** provides a more secure and seamless way to authenticate and push changes.

---

# Steps to Use SSH for GitHub Authentication  

## 1. Generate SSH Key Pair  
An SSH key pair consists of a **public key** and a **private key**.  

Run the following command to generate a key pair:  

```sh
ssh-keygen -t rsa -b 4096 -C "umarfarooq001@gmail.com"
```

When prompted for a path, enter:  

```sh
/c/Users/umar/.ssh/umar-github-key
```

This will be the name of your key.

---

## 2. Register Public Key on GitHub  

### **On GitHub:**
1. Go to **Settings**  
2. Navigate to **SSH and GPG keys**  
3. Click **New SSH key**  
4. Name the key: **umar-github-key** (same as the key pair created)  

### **In the Local Terminal:**
Run the following command to display your public key:  

```sh
cat umar-github-key.pub
```

Copy and paste the key’s content into the GitHub SSH key box.  
The key will be used for all repositories in your GitHub account.  

---

## 3. Register Private Key with SSH Agent  

Start the SSH agent:  

```sh
eval $(ssh-agent -s)
```

This should output a **process ID**, indicating that the SSH agent is running.  

Add your private key to the agent:  

```sh
ssh-add umar-github-key
```

Verify authentication with GitHub:  

```sh
ssh -T git@github.com
```

If successful, you should see a confirmation message from GitHub.

---

# Creating a Test Repository (on GitHub and Locally)  

## **On GitHub:**
1. Create a new repository  
2. Name it: **tech501-test-ssh**  
3. Set it to **Public**  

## **In the Local Terminal:**  
Navigate to your working directory:  

```sh
cd /documents/github
```

Create a new repository with the same name:  

```sh
mkdir tech501-test-ssh
cd tech501-test-ssh
git init
echo "# tech501-test-ssh" > README.md
```

Add the remote repository using SSH:  

```sh
git remote add origin git@github.com:umarx78/tech501-test-ssh.git
```

Push changes:  

```sh
git add .
git branch -M main
git commit -m "test"
git push -u origin main
```

> **Note:** The `git push -u origin main` command needs to be run only the first time to establish the upstream branch.

---

This should now set up your repository with **SSH authentication** instead of **HTTPS**. 