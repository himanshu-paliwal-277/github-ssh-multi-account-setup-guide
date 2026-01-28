# Multiple GitHub Accounts SSH Setup (Windows)

This document explains **how to permanently set up multiple GitHub accounts using SSH on Windows**.
It avoids Windows Credential Manager issues and allows you to use **personal, company, and client GitHub accounts side-by-side**.

---

## Why This Setup?

Problems with HTTPS + Credential Manager:
- GitHub account keeps switching
- Wrong account used for push/commit
- Frequent re-login required

✅ SSH-based setup solves all of this permanently.

---

## System Used

- OS: Windows 10 / 11
- Terminal: Git Bash
- GitHub Accounts:
  - Personal
  - Company
  - Client

---

## Folder Used

All SSH keys live here:

```
C:\Users\<YOUR_USERNAME>\.ssh
```

> ⚠️ Important: **Never delete this folder**
> Backup it if you clean your system.

---

## Step 1: Check / Create `.ssh` Folder

Open **Git Bash**:

```bash
cd ~
ls -a
```

If `.ssh` exists → good  
If not:

```bash
mkdir ~/.ssh
```

---

## Step 2: Generate SSH Keys (One Per Account)

### Personal GitHub
```bash
ssh-keygen -t ed25519 -C "personal-github"
```
When asked:
```
Enter file in which to save the key:
```
Enter:
```
id_ed25519_personal
```

---

### Company GitHub
```bash
ssh-keygen -t ed25519 -C "company-github"
```
Save as:
```
id_ed25519_company
```

---

### Client GitHub
```bash
ssh-keygen -t ed25519 -C "client-github"
```
Save as:
```
id_ed25519_client
```

You will now have:

```
id_ed25519_personal
id_ed25519_personal.pub
id_ed25519_company
id_ed25519_company.pub
id_ed25519_client
id_ed25519_client.pub
```

---

## Step 3: Add Public Keys to GitHub

For each account:

```bash
cat ~/.ssh/id_ed25519_<name>.pub
```

Example:
```bash
cat ~/.ssh/id_ed25519_personal.pub
```

Copy output → GitHub →  
**Settings → SSH and GPG Keys → New SSH Key**

- Key type: Authentication Key
- Title: Any meaningful name
- Paste key → Save

Repeat for all accounts.

---

## Step 4: Create SSH Config File

Create config file:

```bash
touch ~/.ssh/config
```

Edit it:

```bash
nano ~/.ssh/config
```

Paste this:

```ssh
# Personal GitHub
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal

# Company GitHub
Host company-github
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_company

# Client GitHub
Host client-github
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_client
```

Save and exit.

---

## Step 5: Test SSH Connections

```bash
ssh -T git@github-personal
ssh -T git@company-github
ssh -T git@client-github
```

Expected output:

```
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

✅ This means SSH is working.

---

## Step 6: Clone Repositories Correctly (IMPORTANT)

❌ Do NOT use:
```
git@github.com:username/repo.git
```

✅ Always use host alias:

### Personal
```bash
git clone git@github-personal:USERNAME/repo.git
```

### Company
```bash
git clone git@company-github:ORG/repo.git
```

### Client
```bash
git clone git@client-github:ORG/repo.git
```

---

## Step 7: Set Git Identity Per Repository

Inside each repo:

### Personal
```bash
git config user.name "Himanshu Paliwal"
git config user.email "personal@email.com"
```

### Company / Client
```bash
git config user.name "Company Name"
git config user.email "work@email.com"
```

Verify:
```bash
git config --list
```

---

## Edge Cases & Fixes

### 1. SSH key file not created
✔ This is normal if you entered filename without path  
Keys are created inside `.ssh` automatically

---

### 2. `.ssh` folder deleted during cleanup
❌ SSH will break  
✔ Restore backup or regenerate keys

---

### 3. Wrong GitHub account used
✔ Check:
- Clone URL uses correct host
- Repo-level `user.email`

---

### 4. Still asking for password
✔ Ensure:
```bash
ssh -T git@host-name
```
works before cloning

---

## Optional: Backup Recommendation

Backup this folder:

```
C:\Users\<YOUR_USERNAME>\.ssh
```

---

## Final Result

- Multiple GitHub accounts
- No credential switching
- No password prompts
- Clean & professional workflow

---

## Author

Himanshu Paliwal  
Multi-GitHub SSH Setup (Windows)

