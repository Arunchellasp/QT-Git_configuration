


#!/bin/bash

# GitHub SSH Key Setup Script
echo "🔑 Checking for existing SSH keys..."
if [ -f ~/.ssh/id_rsa.pub ]; then
    echo "✅ SSH key already exists."
else
    echo "🚀 Generating a new SSH key..."
    read -p "Enter your GitHub email: " email
    ssh-keygen -t rsa -b 4096 -C "$email"
    echo "✅ SSH key generated successfully."
fi

# Start the SSH agent
echo "🔄 Starting the SSH agent..."
eval "$(ssh-agent -s)"

# Add SSH key to the agent
echo "🔗 Adding SSH key to the agent..."
ssh-add ~/.ssh/id_rsa

# Copy SSH key to clipboard (Linux/macOS: xclip/pbcopy, Windows: clip)
echo "📋 Copying SSH key to clipboard..."
if command -v xclip &>/dev/null; then
    cat ~/.ssh/id_rsa.pub | xclip -selection clipboard
    echo "✅ SSH key copied! Paste it in GitHub SSH settings."
elif command -v pbcopy &>/dev/null; then
    cat ~/.ssh/id_rsa.pub | pbcopy
    echo "✅ SSH key copied! Paste it in GitHub SSH settings."
elif [[ "$OSTYPE" == "msys" ]]; then
    cat ~/.ssh/id_rsa.pub | clip
    echo "✅ SSH key copied! Paste it in GitHub SSH settings."
else
    echo "⚠️ Could not copy SSH key automatically. Please run:"
    echo "cat ~/.ssh/id_rsa.pub"
fi

# Test SSH connection
echo "🔍 Testing SSH connection with GitHub..."
ssh -T git@github.com

echo "🎉 Setup complete! Now add the SSH key to GitHub."
echo "Go to GitHub → Settings → SSH and GPG keys → New SSH Key, and paste it."


Here’s a step-by-step guide you can use on GitHub for setting up Git with SSH authorization on Windows and ensuring it works with both Git Bash and Windows Command Prompt:

---

### **Step 1: Configure Git Username and Email**

Before committing any changes, you need to configure your Git username and email to track who made changes to the repository.

#### **1.1 Set Global Identity (Applies to All Repositories)**
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

#### **1.2 Set Identity for a Specific Repository**
If you want to set your identity only for the current repository, omit the `--global` flag:
```bash
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

*Replace "Your Name" with your actual name and "your.email@example.com" with your actual email address.*

---

### **Step 2: Generate SSH Keys (If Not Already Done)**

If you haven't generated SSH keys yet, follow these steps:

#### **2.1 Open Git Bash**
Open Git Bash on your Windows system.

#### **2.2 Generate a New SSH Key Pair**
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

*Press Enter to accept the default location (`/c/Users/YourUsername/.ssh/id_rsa`). Set a passphrase if desired, or press Enter to skip it.*

---

### **Step 3: Add SSH Key to the SSH Agent**

#### **3.1 Start the SSH Agent**
```bash
eval "$(ssh-agent -s)"
```

#### **3.2 Add Your SSH Private Key to the Agent**
```bash
ssh-add ~/.ssh/id_rsa
```

---

### **Step 4: Configure the HOME Environment Variable for Windows Command Prompt**

To ensure that Git uses the correct SSH keys when running from a Windows Command Prompt:

#### **4.1 Set HOME Environment Variable in Windows**
- Open the Start Menu, and search for **Environment Variables**.
- Click on **Edit the system environment variables**.
- In the **System Properties** window, click on **Environment Variables**.
- Under **User variables** (for your account), click **New**.
- Set the variable name as `HOME` and the variable value as the path to your user directory, e.g., `C:\Users\YourUsername`.
- Click **OK** to save the variable.

---

### **Step 5: Test SSH Connection**

#### **5.1 Open Git Bash or Windows Command Prompt**
Test the SSH connection with GitHub or your Git server:

```bash
ssh -T git@github.com
```

*If successful, you should see a message like "Hi username! You've successfully authenticated...".*

---

### **Step 6: Using Git with SSH in Windows Command Prompt**

After setting the `HOME` environment variable, you should be able to use Git with SSH from both Git Bash and Windows Command Prompt without any issues.

---

### **Summary**

- **Git Bash**: Automatically sets the `HOME` environment variable to your user directory, so SSH keys are found without additional configuration.
- **Windows Command Prompt**: By setting the `HOME` environment variable manually, you ensure that Git finds your SSH keys in the correct directory.

---

Once you've completed these steps, you should be able to commit your changes without encountering any issues.
