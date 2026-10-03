# corporate-devops-sandbox

# Setting Up a Python DevOps Workspace Without Admin Rights

A step-by-step documentation guide on how to engineer a fully automated, isolated Python 3.12 developer environment on a highly restricted corporate Windows device—**without requiring Administrator privileges**.

## 🛑 The Challenge
When trying to set up a workspace for a DevOps course on a locked-down company laptop, several enterprise-level security barriers were encountered:
1. **No Admin Privileges:** Global software installers (`C:\Program Files`) triggered permission locks.
2. **Execution Policy Blocks:** Corporate Windows PowerShell profiles blocked background environment activation scripts.
3. **OneDrive Hijacking:** Enterprise cloud backups seamlessly redirected standard `Documents` paths into a synced OneDrive folder, breaking relative script paths.
4. **VS Code Restricted Mode:** Loose file editing caused VS Code to drop environment preferences on restart.

---

## 🚀 The DevOps Solution

Instead of attempting to modify system-level settings, a complete **User-Space Isolation** strategy was implemented to build a reliable, localized environment.

### 1. User-Space Toolkit Installation
* **Tool:** Miniconda3 (Windows 64-bit).
* **Action:** Installed using the `Just Me (recommended)` flag. 
* **Result:** Bypassed the corporate security guard entirely by installing the package manager strictly within the user's localized `%AppData%` folder.

### 2. Sandbox Room Engineering
* **Action:** Built a dedicated, isolated project room named `devops_course` using a locked-in, stable version of Python.
* **Command Used:**
  ```cmd
  conda config --set plugins.auto_accept_tos yes
  conda create --name devops_course python=3.12 -y
  ```
* **Result:** Guaranteed tool stability for cloud engineering libraries (like `boto3` or `requests`), remaining independent of the device's default system Python.

### 3. Shell Migration & Terminal Profile Automation
To bypass the PowerShell script execution blocks, the VS Code default runtime environment was migrated to the Windows Command Prompt (`cmd`) and automated natively via user-level configurations.

* **File Modified:** `settings.json` (User Profile)
* **Configuration Injected:**
  ```json
  {
      "python.defaultInterpreterPath": "C:\\Users\\<YOUR_USERNAME>\\AppData\\Local\\miniconda3\\envs\\devops_course\\python.exe",
      "terminal.integrated.profiles.windows": {
          "DevOps-CMD": {
              "path": "cmd.exe",
              "args": ["/k", "C:\\Users\\<YOUR_USERNAME>\\AppData\\Local\\miniconda3\\condabin\\conda.bat activate devops_course"]
          }
      },
      "terminal.integrated.defaultProfile.windows": "DevOps-CMD"
  }
  ```
* **Result:** Every single time a new terminal session is opened in VS Code, the `(devops_course)` sandbox environment wakes up and activates itself in **under 0.5 seconds**.

### 4. Overcoming Path Breaks & Guest Closures
* **Local Workspace:** Built a local, unsynced folder directly on the drive: `C:\Users\<YOUR_USERNAME>\devops_projects`.
* **Workspace Trust:** Switched the workspace environment out of VS Code's **Restricted Mode** into **Trusted Mode**.
* **Result:** Locked down the Python interpreter pathway permanently, ensuring the VS Code automated **Play button** works seamlessly on every single run without requiring manual path entry or escaping strings due to OneDrive spacing errors.

---

## 🎯 The Final Workflow
With this infrastructure in place, practicing script execution is down to a simple 3-step routine:
1. Open **VS Code** to the `devops_projects` folder workspace.
2. Open a **New Terminal** (the sandbox room fires up instantly).
3. Hit the **Play** button on any `.py` script.

**Result:** `Hello DevOps! Python version is: 3.12.15 | packaged by Anaconda, Inc.` 🚀
