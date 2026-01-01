# Github SSH Configuration
We can have separate `Git` credentials for handling multiple Git repositories in our local system. This can be done using Git SSH config file.

## What is SSH config file

The SSH config file (`~/.ssh/config`) is a configuration file that allows you to define custom SSH connection settings for different hosts. It simplifies SSH connections by allowing you to use aliases and preset configurations instead of typing long commands with multiple parameters.

### Benefits
- Manage multiple GitHub accounts on the same machine
- Use different SSH keys for different repositories
- Simplify SSH commands with aliases
- Separate personal and work GitHub accounts

---

## Prerequisites

Before configuring SSH, ensure you have:
- Git installed on your system
- SSH key pairs generated
- Access to your GitHub account(s)

---

## Step 1: Generate SSH Keys

Generate separate SSH keys for each GitHub account you want to manage.

### For Personal Account
```bash
ssh-keygen -t ed25519 -C "personal@example.com" -f ~/.ssh/id_ed25519_personal
```

### For Work Account
```bash
ssh-keygen -t ed25519 -C "work@company.com" -f ~/.ssh/id_ed25519_work
```

<div style="background-color: #e7f3ff; border-left: 4px solid #2196F3; padding: 10px; margin: 10px 0;">
💡 <strong>TIP:</strong> Press Enter when prompted for a passphrase, or set one for added security.
</div>

---

## Step 2: Add SSH Keys to SSH Agent

Start the SSH agent and add your keys:

```bash
# Start SSH agent
eval "$(ssh-agent -s)"

# Add personal key
ssh-add ~/.ssh/id_ed25519_personal

# Add work key
ssh-add ~/.ssh/id_ed25519_work
```

### Verify Added Keys
```bash
ssh-add -l
```

---

## Step 3: Add SSH Keys to GitHub

Copy your public key to clipboard:

```bash
# For personal account
cat ~/.ssh/id_ed25519_personal.pub | pbcopy

# For work account
cat ~/.ssh/id_ed25519_work.pub | pbcopy
```

Then add to GitHub:
1. Go to GitHub Settings → SSH and GPG keys
2. Click "New SSH key"
3. Paste the public key
4. Give it a descriptive title (e.g., "Personal MacBook" or "Work Laptop")

---

## Step 4: Configure SSH Config File

Create or edit the SSH config file:

```bash
touch ~/.ssh/config
chmod 600 ~/.ssh/config
nano ~/.ssh/config
```

### Example Configuration

```ssh-config
# Personal GitHub Account
Host github.com-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
    IdentitiesOnly yes

# Work GitHub Account
Host github.com-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
    IdentitiesOnly yes

# Default GitHub (optional)
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
    IdentitiesOnly yes
```

### Configuration Breakdown

| Parameter | Description |
|-----------|-------------|
| `Host` | Alias you'll use in Git commands |
| `HostName` | Actual hostname (github.com) |
| `User` | Always `git` for GitHub |
| `IdentityFile` | Path to your private SSH key |
| `IdentitiesOnly` | Use only specified identity file |

---

## Step 5: Clone Repositories

### Using Personal Account
```bash
git clone git@github.com-personal:username/repo.git
```

### Using Work Account
```bash
git clone git@github.com-work:company/repo.git
```

---

## Step 6: Configure Git User for Repositories

Set the correct Git user for each repository:

### For Personal Repository
```bash
cd personal-repo
git config user.name "Your Name"
git config user.email "personal@example.com"
```

### For Work Repository
```bash
cd work-repo
git config user.name "Your Work Name"
git config user.email "work@company.com"
```

<div style="background-color: #fff3cd; border-left: 4px solid #ffc107; padding: 10px; margin: 10px 0;">
⚠️ <strong>IMPORTANT:</strong> Use <code>git config</code> without <code>--global</code> to set user info per repository.
</div>

---

## Step 7: Update Existing Repositories

If you have existing repositories, update their remote URLs:

### Check Current Remote
```bash
git remote -v
```

### Update Remote URL
```bash
# For personal account
git remote set-url origin git@github.com-personal:username/repo.git

# For work account
git remote set-url origin git@github.com-work:company/repo.git
```

---

## Testing SSH Connection

Test your SSH connections to verify the setup:

```bash
# Test personal account
ssh -T git@github.com-personal

# Test work account
ssh -T git@github.com-work
```

### Expected Output
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## Troubleshooting

### Permission Denied Error
```bash
# Check SSH agent
ssh-add -l

# Re-add keys if needed
ssh-add ~/.ssh/id_ed25519_personal
ssh-add ~/.ssh/id_ed25519_work
```

### Wrong Account Being Used
```bash
# Check which key is being used
ssh -vT git@github.com-personal

# Verify git config
git config user.email
```

### SSH Key Not Found
```bash
# Verify key files exist
ls -la ~/.ssh/

# Check file permissions
chmod 600 ~/.ssh/id_ed25519_*
chmod 644 ~/.ssh/id_ed25519_*.pub
```

---

## Common Workflows

### Starting Fresh on New Machine
```bash
# 1. Generate keys
ssh-keygen -t ed25519 -C "email@example.com" -f ~/.ssh/id_ed25519_personal

# 2. Add to agent
ssh-add ~/.ssh/id_ed25519_personal

# 3. Copy public key
cat ~/.ssh/id_ed25519_personal.pub | pbcopy

# 4. Add to GitHub (via web interface)

# 5. Create config file
nano ~/.ssh/config

# 6. Test connection
ssh -T git@github.com-personal
```

### Switching Between Accounts
```bash
# Clone with specific account
git clone git@github.com-work:company/repo.git

# Set user info
cd repo
git config user.name "Work Name"
git config user.email "work@company.com"

# Verify
git config --list
```

---

## Advanced Configuration

### Using Different Ports
```ssh-config
Host custom-github
    HostName github.com
    User git
    Port 443
    IdentityFile ~/.ssh/id_ed25519_custom
```

### SSH Key with Passphrase Auto-unlock (macOS)
Add to `~/.ssh/config`:
```ssh-config
Host *
    AddKeysToAgent yes
    UseKeychain yes
```

### Multiple Organizations
```ssh-config
Host github-org1
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_org1

Host github-org2
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_org2
```

---

## Quick Reference

| Command | Purpose |
|---------|---------|
| `ssh-keygen -t ed25519 -C "email" -f ~/.ssh/keyname` | Generate new SSH key |
| `ssh-add ~/.ssh/keyname` | Add key to SSH agent |
| `ssh-add -l` | List loaded keys |
| `ssh -T git@github.com-alias` | Test SSH connection |
| `git remote set-url origin git@alias:user/repo.git` | Update remote URL |
| `git config user.email` | Check current Git email |
| `cat ~/.ssh/config` | View SSH config |

---

## Resources

- [GitHub SSH Documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [SSH Config Man Page](https://man.openbsd.org/ssh_config)
- [Git Configuration](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)

---

<div align="center">
<em>Keep your SSH keys secure and never share your private keys!</em>
</div>
