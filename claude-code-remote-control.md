# Remote Coding Setup Guide

**Code from your iPhone while AI runs on your Windows PC**

**Tailored for Windows + Git Bash + Claude Code + iPhone**

---

## Table of Contents

- [Section 0: Overview & Prerequisites](#section-0-overview--prerequisites)
- [Section 1: Claude Code Remote Control](#section-1-claude-code-remote-control)
- [Why Go Beyond Native Remote Control?](#why-go-beyond-native-remote-control)
- [Section 2: Tailscale — Access localhost from your phone](#section-2-tailscale--access-localhost-from-your-phone)
- [Section 3: Termius + Tmux — Full terminal sync from your phone](#section-3-termius--tmux--full-terminal-sync-from-your-phone)
  - [Part A: Enable SSH on Windows](#part-a-enable-ssh-on-windows)
  - [Part B: Install Termius on iPhone](#part-b-install-termius-on-iphone)
  - [Part C: Install Tmux (Two Paths)](#part-c-install-tmux-two-paths)
  - [Part D: Configure SSH Default Shell](#part-d-configure-ssh-default-shell)
  - [Part E: The Tmux Workflow](#part-e-the-tmux-workflow-the-actual-magic)
- [Section 4: Putting It All Together](#section-4-putting-it-all-together)
- [Section 5: Troubleshooting](#section-5-troubleshooting)

---

## Section 0: Overview & Prerequisites

### What this guide achieves

By the end of this guide, you will be able to:

1. **Send prompts to Claude Code from your iPhone** while Claude Code runs on your Windows PC (using Remote Control)
2. **See your localhost web app on your iPhone** — test what Claude Code builds without being at your desk (using Tailscale)
3. **Control your full terminal from your iPhone** — works with Claude Code, Codex, or any CLI tool, with sessions that stay alive even if you disconnect (using Termius + Tmux)

### What you need before starting

| Requirement | Details |
|---|---|
| **Windows PC** | Windows 10 (build 19041 or later) or Windows 11 |
| **Git Bash** | Already installed and working (you have this) |
| **Claude Code** | Already installed and working in Git Bash (you have this) |
| **iPhone** | Any iPhone running iOS 15 or later |
| **Internet** | Both your PC and iPhone need internet access |
| **Apple ID** | For downloading apps from the App Store |

### Tool summary

| Tool | What it does | Cost |
|---|---|---|
| **Claude Code Remote Control** | Syncs your Claude Code terminal session to the Claude iOS app | Free (built into Claude Code) |
| **Tailscale** | Creates a secure private network between your PC and iPhone so they can talk to each other | Free for personal use |
| **Termius** | SSH client app for iPhone — lets you open a terminal on your PC from your phone | Free tier is sufficient |
| **Tmux** | Terminal multiplexer — keeps terminal sessions alive and syncs them between devices | Free and open source |

### Which sections do you need?

- **Just want to send prompts from your phone?** Section 1 only.
- **Want to also see localhost?** Sections 1 + 2.
- **Want the full setup (terminal sync, works with any tool)?** Sections 1 + 2 + 3.

---

## Section 1: Claude Code Remote Control

### What it is

Claude Code Remote Control syncs your local Claude Code session to the Claude web/mobile app. Anything you type on your phone appears in your terminal, and vice versa. It requires zero extra software — it is built into Claude Code.

### What it gives you

- Send prompts to Claude Code from your iPhone
- See Claude Code's output in real time on your phone
- Commit code, run commands, and manage your project from anywhere

### What it does NOT give you

- You **cannot** see localhost (your web app preview) on your phone
- You **cannot** use it with other tools like Codex or Open Code

### Step 1.1: Start Claude Code in Git Bash

Open Git Bash on your Windows PC. Navigate to your project folder and start Claude Code:

```bash
# In Git Bash
cd ~/your-project-folder
claude
```

You should see Claude Code running in your terminal.

### Step 1.2: Launch Remote Control

Inside the running Claude Code session, type:

```
/remote-control
```

Press Enter. Claude Code will connect to Anthropic's secure server and display a URL. It looks something like:

```
Remote control URL: https://claude.ai/code/session/abc123...
```

### Step 1.3: Connect from your iPhone

**Option A: Using the Claude iOS app (recommended)**

1. Open the **Claude** app on your iPhone (download from App Store if you don't have it)
2. Log in with your Anthropic account (the same account you use for Claude Code)
3. Tap the **Code** tab at the bottom of the app
4. You should see an **Interactive session** listed — tap on it
5. Your phone is now synced with your Claude Code terminal

**Option B: Using Safari**

1. Copy the URL that Claude Code displayed in Step 1.2
2. Send it to your iPhone (AirDrop, iMessage, email, or any way you prefer)
3. Open Safari on your iPhone and paste the URL
4. Log in with your Anthropic account if prompted

### Step 1.4: Verify the sync

1. On your iPhone, type a message like: `list all files in this project`
2. Look at your Windows PC terminal — you should see the same message appear
3. Claude Code's response will appear on both your phone and your PC simultaneously

### Step 1.5: When you're done

To stop the remote control session:
- In your Claude Code terminal on your PC, press `Ctrl+C` or type `/quit`
- Or simply close the Claude Code session

The remote control session ends when Claude Code stops.

---

## Why Go Beyond Native Remote Control?

Claude Code's built-in `/remote-control` is great for sending text prompts from your phone — but it has a fundamental blind spot: **you can't see what you're building.**

### The problem: vibe coding blind

Consider this scenario. You're building a web app — a game, a dashboard, a landing page — and Claude Code is making changes to your code. On your PC, you'd open `localhost:5173` in your browser to see the result. But from your phone using Remote Control, all you see is Claude Code's terminal output: file diffs, tool calls, and text responses.

You're coding blind. You can tell Claude Code "make the sidebar collapsible" and it will do it, but you have no way to verify that the sidebar actually looks right, that the animation is smooth, or that it didn't break the layout. You'd have to walk back to your PC to check.

This is exactly the problem chongdashu encountered. He was building an isometric game inspired by Final Fantasy Tactics — a project with a 3D map, 2D character sprites, combat animations, and UI panels — all running in the browser on `localhost`. From his phone, he could tell Claude Code to "make the character preview panel collapsible," and Claude Code would do it. But he couldn't see the game. He couldn't verify that the collapse animation worked, that the panel didn't overlap the game map, or that the sprites still rendered correctly. For a visual project like a game, this is a dealbreaker.

### What the Tmux + Termius + Tailscale stack solves

This setup fills every gap that native Remote Control leaves open:

#### 1. See localhost on your phone (Tailscale)

Tailscale creates a secure network between your PC and phone. Once connected, you open Safari on your iPhone and type `http://<tailscale-ip>:5173` — and your web app appears, live, on your phone. You can interact with it, test touch gestures, check responsive layouts, and verify that Claude Code's changes actually look correct.

This is the single biggest upgrade over native Remote Control. **You complete the feedback loop**: prompt Claude Code to make a change, then immediately see the result on your phone without walking back to your desk.

#### 2. Works with any terminal tool, not just Claude Code (Tmux + Termius)

Native Remote Control is locked to Claude Code. If you want to use OpenAI's Codex, Open Code, Aider, or any other CLI tool, you're out of luck — they don't have a "remote control" feature.

Tmux + Termius solves this at the infrastructure level. You SSH into your PC from your phone and attach to a tmux session. Whatever is running in that session — Claude Code, Codex, a plain bash shell — you see it and control it. You can even run multiple sessions simultaneously: one for Claude Code, one for Codex, and switch between them on your phone.

Chongdashu demonstrated this directly: he had Claude Code running in one tmux session and Codex in another, and switched between them from his iPhone. You're not dependent on any single AI vendor adding mobile support.

#### 3. Sessions survive disconnects (Tmux)

When you use native Remote Control and your phone goes to sleep or loses signal, you simply stop seeing updates until you reconnect. That's manageable.

But with Termius alone (without tmux), if your SSH connection drops — which it will, because iOS aggressively kills background apps after about 3 minutes — your terminal session is gone. Any running process that was attached to that session gets killed.

Tmux fixes this completely. Your session runs on the PC inside tmux, independent of any SSH connection. Your phone can connect and disconnect freely. When you reconnect, you `tmux attach` and everything is exactly where you left it — the full scrollback history, the running processes, the cursor position. Nothing is lost.

#### 4. Full terminal access, not just a chat interface

Native Remote Control gives you a chat-like interface for sending prompts to Claude Code. But sometimes you need to do things outside of Claude Code: run `git log`, check `npm run build` output, edit a config file, restart a dev server, check disk space, or run tests manually.

With Tmux + Termius, you have a real terminal. You can do anything you'd do sitting at your PC.

### Summary: when to use what

| Scenario | Best tool |
|---|---|
| Quick prompt to Claude Code while away from desk | Native Remote Control (Section 1) |
| Building a visual project (game, web app, UI) and need to see it | Add Tailscale (Section 2) |
| Want to use tools other than Claude Code (Codex, Open Code, etc.) | Add Tmux + Termius (Section 3) |
| Need reliable sessions that survive phone disconnects | Add Tmux + Termius (Section 3) |
| Want the full "I'm at my desk but actually on my couch" experience | All three — Tailscale + Tmux + Termius (Sections 2 + 3) |

---

## Section 2: Tailscale — Access localhost from your phone

### What Tailscale is (plain English)

When you run a web app on your PC, it starts a server that listens on a "port" — think of a port as a numbered door (like door 5173). You access the app by going to `localhost:5173` in your browser, where "localhost" means "this computer only." Your iPhone can't reach `localhost` because it's a different device.

Tailscale solves this. It creates a private, encrypted network that connects your PC and iPhone. Each device gets a special IP address (like `100.x.x.x`) that only your devices can see. Nobody else on the internet can access these addresses.

Think of it as a secret tunnel between your PC and your phone.

### Step 2.1: Create a Tailscale account

1. On your Windows PC, open a browser
2. Go to **https://login.tailscale.com/start**
3. Choose how to sign up — pick one:
   - **Google** (if you have a Gmail account)
   - **Apple** (if you prefer Apple ID)
   - **GitHub** (if you have a GitHub account)
   - **Microsoft** (if you have an Outlook/Microsoft account)
4. Complete the sign-in process for whichever provider you chose
5. Tailscale will create your account automatically — no password needed

> **Note**: Tailscale uses "Single Sign-On" (SSO). This means your Tailscale password IS your Google/Apple/GitHub/Microsoft password. You won't create a separate Tailscale password.

### Step 2.2: Install Tailscale on Windows

1. Go to **https://tailscale.com/download/windows**
2. Click **Download** to get the installer (a `.exe` file)
3. Open your Downloads folder and double-click the installer file
4. Follow the installation prompts (click Next/Accept/Install)
5. When installation finishes, look at the **bottom-right corner** of your screen (the system tray, next to the clock)
6. You should see a new Tailscale icon (it looks like a small network/mesh shape)
   - If you don't see it, click the small **up arrow** (^) in the system tray to show hidden icons
7. **Right-click** the Tailscale icon
8. Click **Log in** (or **Sign in**)
9. A browser window will open — log in with the **same account** you created in Step 2.1
10. After logging in, the browser will say something like "You can close this window"
11. The Tailscale icon in your system tray should now show a checkmark — this means it's connected

### Step 2.3: Install Tailscale on iPhone

1. Open the **App Store** on your iPhone
2. Tap the **Search** tab at the bottom
3. Type **Tailscale** in the search bar
4. Find the app called **Tailscale** by **Tailscale Inc.**
5. Tap **Get**, then confirm with Face ID, Touch ID, or your Apple ID password
6. Wait for the download to finish, then tap **Open**
7. Tap **Get Started**
8. You will see a popup: **"Tailscale" Would Like to Add VPN Configurations**
   - Tap **Allow** — this is required for Tailscale to work
   - This does NOT monitor your internet traffic. Tailscale only creates a secure tunnel between your devices.
9. Tap **Log In**
10. Choose the **same sign-in method** you used in Step 2.1 (Google, Apple, GitHub, or Microsoft)
11. Complete the login process
12. Your iPhone is now connected to your Tailscale network

> **Important**: You MUST log in with the same account on both devices. If you used Google on your PC, use Google on your iPhone too.

### Step 2.4: Find your Windows Tailscale IP

You need to know your Windows PC's Tailscale IP address. This is the address your iPhone will use to connect to your PC.

**Method A: System tray (easiest)**

1. **Right-click** the Tailscale icon in your system tray (bottom-right of screen)
2. Your Tailscale IP address is displayed in the menu (it starts with `100.`)
3. Click **Copy IPv4 address** to copy it to your clipboard

**Method B: PowerShell command**

1. Open **PowerShell** (press Windows key, type `powershell`, press Enter)
2. Type this command and press Enter:

```powershell
tailscale ip -4
```

3. It will print your Tailscale IP address (something like `100.64.5.123`)
4. Write this number down or copy it — you'll need it throughout this guide

### Step 2.5: Verify both devices are connected

**Check from Windows:**

1. Open **PowerShell**
2. First, find your iPhone's Tailscale IP: on your iPhone, open the **Tailscale** app. On the main screen, you'll see your iPhone listed with an IP address starting with `100.` — this is your iPhone's Tailscale IP. Write it down.
3. Back in PowerShell on your PC, run this command (replace the IP with your iPhone's Tailscale IP from the step above):

```powershell
tailscale ping <your-iphone-tailscale-ip>
```

3. If you see `pong from iPhone...` responses, the connection is working

**Check from the Admin Console:**

1. On your PC, open a browser and go to **https://login.tailscale.com**
2. Click **Machines** in the left sidebar
3. You should see **two devices** listed:
   - Your Windows PC — with status **Connected** (green)
   - Your iPhone — with status **Connected** (green)

If both show as connected, you're good to go.

### Step 2.6: Access localhost from your iPhone

Now for the magic. Let's say you have a web app running on your PC at `localhost:5173`.

1. Make sure your web app is running on your PC (e.g., `npm run dev` in Git Bash)
2. On your iPhone, open **Safari** (or any browser)
3. In the address bar, type:

```
http://<your-windows-tailscale-ip>:5173
```

For example, if your Tailscale IP is `100.64.5.123`:

```
http://100.64.5.123:5173
```

4. Press **Go** (or the Enter key on your keyboard)
5. You should see your web app — the same thing you see on `localhost:5173` on your PC

> **Important**: Replace `5173` with whatever port YOUR app actually uses. Common ports: `3000` (React), `5173` (Vite), `8080` (Vue), `4200` (Angular).

### Tailscale troubleshooting

| Problem | Solution |
|---|---|
| Can't find Tailscale icon in system tray | Click the small up arrow (^) in the system tray to show hidden icons |
| iPhone shows "Not Connected" | Open the Tailscale app, toggle the connection off and back on |
| `tailscale ping` times out | Make sure Tailscale is running on both devices. Try restarting the app on both |
| Website doesn't load from iPhone | 1) Check the port number is correct. 2) Make sure the dev server is actually running on your PC. 3) Try accessing `http://localhost:5173` on your PC first to confirm it works locally |
| "Connection refused" on iPhone | Your dev server might only be listening on `127.0.0.1`. Try starting it with `--host 0.0.0.0` (e.g., `npm run dev -- --host 0.0.0.0`) |

---

## Section 3: Termius + Tmux — Full terminal sync from your phone

This section gives you the most powerful setup: a full terminal on your iPhone that stays in sync with your PC. It works with Claude Code, Codex, Open Code, or any terminal tool.

Here's what you'll set up:

1. **Part A**: Enable SSH on your Windows PC (so your iPhone can connect to it)
2. **Part B**: Install Termius on your iPhone (the app that connects)
3. **Part C**: Install Tmux (keeps sessions alive and synced)
4. **Part D**: Configure SSH to use the right shell
5. **Part E**: The actual workflow

---

### Part A: Enable SSH on Windows

SSH (Secure Shell) is how your iPhone will connect to your Windows PC's terminal. Windows has a built-in SSH server, but it's turned off by default. Let's turn it on.

> **All commands in this section are run in PowerShell as Administrator.**

#### Step 3A.1: Open PowerShell as Administrator

1. Press the **Windows key** on your keyboard
2. Type **PowerShell**
3. In the search results, look for **Windows PowerShell**
4. **Right-click** on it
5. Click **Run as administrator**
6. A blue PowerShell window will open with the title showing "Administrator"
7. If a "User Account Control" popup appears, click **Yes**

> **Warning**: Do NOT use Git Bash for these steps. You must use PowerShell as Administrator.

#### Step 3A.2: Check if OpenSSH Server is already installed

Copy and paste this command into the PowerShell window and press Enter:

```powershell
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
```

Look at the output. You'll see two items:
- `OpenSSH.Client` — probably already says `Installed`
- `OpenSSH.Server` — check what it says:
  - If it says `Installed` — skip to Step 3A.3
  - If it says `NotPresent` — continue to install it below

#### Step 3A.3: Install OpenSSH Server

Copy and paste this command and press Enter:

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Wait for it to finish (1-2 minutes). You should see output containing:

```
Online : True
RestartNeeded : False
```

#### Step 3A.4: Start the SSH service and set it to auto-start

Run these two commands one at a time:

**Command 1** — Start the SSH service now:

```powershell
Start-Service sshd
```

**Command 2** — Make it start automatically every time your PC boots:

```powershell
Set-Service -Name sshd -StartupType 'Automatic'
```

#### Step 3A.5: Create a firewall rule to allow SSH connections

Copy and paste this entire block into PowerShell and press Enter:

```powershell
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```

You should see a message confirming the firewall rule exists.

#### Step 3A.6: Verify everything is working

Run this command:

```powershell
Get-Service sshd
```

The output should show:

```
Status   Name               DisplayName
------   ----               -----------
Running  sshd               OpenSSH SSH Server
```

If **Status** shows `Running`, SSH is ready. You can close the PowerShell window.

---

### Part B: Install Termius on iPhone

Termius is an SSH client — it's the app on your iPhone that connects to your PC's terminal.

#### Step 3B.1: Download Termius

1. Open the **App Store** on your iPhone
2. Tap the **Search** tab at the bottom
3. Type **Termius** in the search bar
4. Find the app called **Termius - Terminal & SSH client** by **Termius Corporation**
5. Tap **Get**, then confirm with Face ID, Touch ID, or your Apple ID password
6. Wait for the download to finish

#### Step 3B.2: Open Termius and initial setup

1. Tap **Open** to launch Termius
2. You'll see a welcome/signup screen
3. **Tap "Skip"** (or close the dialog) — you don't need to create a Termius account
4. You're now on the main screen

#### Step 3B.3: Find your Windows username

Before adding a connection, you need to know your Windows username.

On your Windows PC, open **PowerShell** (regular, not Admin) and type:

```powershell
whoami
```

The output will look like: `DESKTOP-ABC123\simon`

Your username is the part **after the backslash**: `simon`

Write this down.

#### Step 3B.4: Add your Windows PC as a host

1. In Termius on your iPhone, tap the **Hosts** tab (house icon at the bottom)
2. Tap the **+** button (top-right corner)
3. Tap **New Host**
4. Fill in these fields:

| Field | What to enter |
|---|---|
| **Alias** | `My Windows PC` (or any name you like) |
| **Hostname** | Your Windows Tailscale IP from Step 2.4 (e.g., `100.64.5.123`) |
| **Port** | `22` (this should already be the default) |
| **Username** | Your Windows username from Step 3B.3 (e.g., `simon`) |
| **Password** | Your Windows login password (the password you use to log into your PC) |

5. Tap **Save** (checkmark icon, top-right)

#### Step 3B.5: Test the connection

1. Back in the Hosts list, tap on **My Windows PC** (or whatever you named it)
2. Termius will try to connect
3. **First time only**: You'll see a message about the "host key fingerprint" — this is a security check:

```
The authenticity of host '100.64.5.123' can't be established.
Are you sure you want to continue connecting?
```

4. Tap **Yes** (or **Continue**) — this is normal for the first connection
5. If you see a command prompt like this, the connection is successful:

```
simon@DESKTOP-ABC123 C:\Users\simon>
```

6. Type `dir` and press Enter to confirm you can run commands — you should see a list of files

> **If the connection fails**, see the [Troubleshooting section](#section-5-troubleshooting) at the end of this guide.

---

### Part C: Install Tmux (Two Paths)

Tmux is what makes the magic happen. It creates terminal sessions that **stay alive** even when you disconnect, and **sync** across devices. When you open a tmux session on your PC and attach to it from your phone, both screens show the exact same thing in real time.

**The problem**: Tmux is a Linux/Unix tool. It does not come with Git Bash on Windows. You have two options:

---

#### Choosing Your Path

| | **Path 1: Git Bash + MSYS2** | **Path 2: WSL2** |
|---|---|---|
| **What it means** | Copy tmux into your existing Git Bash | Install Linux on Windows, run tmux there |
| **Keeps your current Git Bash setup?** | Yes | No — Claude Code runs in WSL2 instead |
| **Difficulty** | Medium (manual file copying) | Easy (a few commands) |
| **Reliability** | Can break when Git for Windows updates | Rock-solid |
| **Best for** | People who want to stay in Git Bash | People who want the most reliable setup |

**Pick one path and follow it. You do NOT need to do both.**

---

#### Path 1: Tmux in Git Bash (keep your current setup)

This approach installs tmux in MSYS2 and copies the binary + DLLs into your Git Bash installation.

##### Step 3C-P1.1: Download and install MSYS2

1. Open a browser on your PC and go to **https://www.msys2.org/**
2. Click the download link for the installer (it will be a `.exe` file, something like `msys2-x86_64-*.exe`)
3. Open your Downloads folder and double-click the installer
4. Click through the installation wizard:
   - **Installation Folder**: Leave the default (`C:\msys64`) — do not change this
   - Click **Next** through all screens
   - Click **Install**
5. When installation finishes, a checkbox says "Run MSYS2 now" — **leave it checked** and click **Finish**
6. An MSYS2 terminal window will open (it looks similar to Git Bash but has a different title)

> **Note**: MSYS2 is a separate tool from Git Bash. Don't worry — installing it won't break your Git Bash.

##### Step 3C-P1.2: Install tmux inside MSYS2

In the MSYS2 terminal window that just opened, type this command and press Enter:

```bash
pacman -S tmux
```

When it asks `Proceed with installation? [Y/n]`, type **Y** and press Enter.

Wait for the installation to finish.

Verify it installed correctly:

```bash
tmux --version
```

You should see something like `tmux 3.4` (the version number may vary).

##### Step 3C-P1.3: Copy tmux files to Git Bash

Now you need to copy tmux and its required files from MSYS2 to Git Bash. Open **PowerShell as Administrator** (Windows key > type PowerShell > right-click > Run as administrator).

Run these commands one at a time:

```powershell
Copy-Item "C:\msys64\usr\bin\tmux.exe" "C:\Program Files\Git\usr\bin\" -Force
```

```powershell
Copy-Item "C:\msys64\usr\bin\msys-event-*.dll" "C:\Program Files\Git\usr\bin\" -Force
```

```powershell
Copy-Item "C:\msys64\usr\bin\msys-event_core-*.dll" "C:\Program Files\Git\usr\bin\" -Force
```

> **Note**: If the `msys-event_core-*.dll` command gives an error saying "Cannot find path", that's OK — not all MSYS2 versions have this separate file. The important ones are `tmux.exe` and `msys-event-*.dll`.

##### Step 3C-P1.4: Test tmux in Git Bash

1. **Close ALL Git Bash windows** — every single one. This is important because Git Bash caches binaries.
2. Open a **new** Git Bash window
3. Type:

```bash
tmux --version
```

4. If you see a version number (e.g., `tmux 3.4`), tmux is installed successfully
5. Do a quick test:

```bash
tmux new -s test
```

6. You should see a green bar at the bottom of the terminal — this means you're inside a tmux session
7. Type `exit` to leave the test session

> **If tmux doesn't work or gives DLL errors**, see the [Troubleshooting section](#section-5-troubleshooting).

**You're done with Path 1. Skip to [Part D: Configure SSH Default Shell](#part-d-configure-ssh-default-shell).**

---

#### Path 2: Tmux in WSL2 (most reliable)

This approach installs Windows Subsystem for Linux (WSL2), which gives you a real Linux environment. Tmux installs natively with a single command.

##### Step 3C-P2.1: Install WSL2

Open **PowerShell as Administrator** (Windows key > type PowerShell > right-click > Run as administrator).

Run this command:

```powershell
wsl --install
```

This will:
- Enable the WSL2 feature
- Download and install Ubuntu Linux

Wait for it to finish. It may take several minutes depending on your internet speed.

##### Step 3C-P2.2: Restart your computer

When the installation tells you to restart, **restart your computer now**.

After restarting, an Ubuntu terminal window should open automatically asking you to create a username and password.

> If the Ubuntu window doesn't open automatically: Press the Windows key, type **Ubuntu**, and click on it.

##### Step 3C-P2.3: Set up your Ubuntu username and password

1. The terminal will ask: `Enter new UNIX username:`
   - Type a username (can be anything — e.g., `simon`) and press Enter
2. It will ask: `New password:`
   - Type a password and press Enter (you won't see any characters as you type — this is normal)
3. It will ask: `Retype new password:`
   - Type the same password again and press Enter

> **Remember this password.** You'll need it whenever you use `sudo` (admin commands) in WSL2.

##### Step 3C-P2.4: Install tmux

In the Ubuntu terminal, run these commands one at a time:

```bash
sudo apt update
```

(Enter your password when prompted)

```bash
sudo apt install -y tmux
```

Verify the installation:

```bash
tmux --version
```

You should see something like `tmux 3.4`.

##### Step 3C-P2.5: Install Claude Code in WSL2

Since you'll be running Claude Code inside WSL2, you need to install it there too.

**Install Node.js:**

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
```

```bash
sudo apt install -y nodejs
```

Verify Node.js:

```bash
node --version
```

You should see a version number like `v22.x.x`.

**Install Claude Code:**

```bash
npm install -g @anthropic-ai/claude-code
```

Verify Claude Code:

```bash
claude --version
```

##### Step 3C-P2.6: Access your Windows files from WSL2

Your Windows files are accessible in WSL2 at `/mnt/c/`. For example:

```bash
# Go to your Windows home folder
cd /mnt/c/Users/simon

# Go to a project
cd /mnt/c/Users/simon/Downloads/my-project
```

> **Tip**: For best performance, keep projects in WSL2's own filesystem (`/home/simon/projects/`) rather than on the Windows drive (`/mnt/c/...`). But accessing Windows files works fine for most use cases.

**You're done with Path 2. Continue to [Part D: Configure SSH Default Shell](#part-d-configure-ssh-default-shell).**

---

### Part D: Configure SSH Default Shell

When you SSH into your Windows PC from Termius, Windows drops you into **cmd.exe** (the old Command Prompt) by default. That's not useful — you need either Git Bash (Path 1) or WSL2 bash (Path 2).

#### If you chose Path 1 (Git Bash)

Set Git Bash as the default SSH shell. Open **PowerShell as Administrator** and run:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Program Files\Git\bin\bash.exe" -PropertyType String -Force
```

Verify it was set:

```powershell
Get-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell
```

The output should show the path to `bash.exe`.

Now, when you SSH in from Termius, you'll land in a Git Bash shell where tmux is available.

#### If you chose Path 2 (WSL2)

You have two options:

**Option A: Set WSL as default shell (automatic)**

Open **PowerShell as Administrator** and run:

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\wsl.exe" -PropertyType String -Force
```

Now, when you SSH in from Termius, you'll land directly in your WSL2 Ubuntu shell.

**Option B: Type `wsl` manually after connecting (simpler)**

Don't change the default shell. When you SSH in from Termius, you'll land in cmd.exe. Simply type:

```
wsl
```

Press Enter. You're now in your WSL2 Ubuntu shell. This is simpler but requires an extra step each time.

---

### Part E: The Tmux Workflow (the actual magic)

This is what you've been building toward. Here's how to create a synced terminal session between your PC and your iPhone.

#### Step 3E.1: Start a tmux session on your PC

On your Windows PC, open your terminal:
- **Path 1 users**: Open **Git Bash**
- **Path 2 users**: Open **Ubuntu** (press Windows key, type Ubuntu, click it)

Navigate to your project folder:

```bash
# Path 1 (Git Bash)
cd ~/Downloads/my-project

# Path 2 (WSL2) — for Windows files
cd /mnt/c/Users/simon/Downloads/my-project
```

Create a named tmux session:

```bash
tmux new -s claude
```

You should see a **green bar at the bottom** of the terminal. The bar shows the session name `claude`. You are now inside a tmux session.

#### Step 3E.2: Start Claude Code inside tmux

Inside the tmux session, run:

```bash
claude
```

Claude Code is now running inside a tmux session. This is important — because it's inside tmux, the session will **stay alive** even if you disconnect.

#### Step 3E.3: Connect from your iPhone

1. On your iPhone, open the **Termius** app
2. Tap on your **My Windows PC** host (the one you set up in Part B)
3. Wait for it to connect
4. Once connected, you'll see a terminal prompt

If you chose **Path 2, Option B** (didn't change default shell), type `wsl` first:

```
wsl
```

#### Step 3E.4: Attach to the tmux session

Now type:

```bash
tmux attach -t claude
```

Press Enter. **BOOM.** Your iPhone is now showing the exact same terminal session as your PC. Everything is synced in real time:

- Type on your phone → it appears on your PC
- Type on your PC → it appears on your phone
- Claude Code's output appears on both simultaneously

#### Step 3E.5: Detach without killing the session

When you want to stop controlling from your phone **without** stopping Claude Code:

1. Press **Ctrl+B** (on the Termius keyboard, hold Ctrl and tap B)
2. Then press **D** (just the D key, not Ctrl+D)

This "detaches" you from the tmux session. The session keeps running on your PC. You can re-attach any time by SSH-ing in again and running `tmux attach -t claude`.

> **Termius keyboard tip**: To press Ctrl, look for the special key bar above the keyboard in Termius. There should be a `Ctrl` button you can tap.

#### Step 3E.6: Running multiple sessions

You can run multiple tmux sessions at the same time — one for Claude Code, one for Codex, one for general commands:

**On your PC:**

```bash
# Session 1: Claude Code
tmux new -s claude
claude
# Press Ctrl+B then D to detach

# Session 2: General terminal
tmux new -s general
# Do whatever you want here
# Press Ctrl+B then D to detach
```

**On your iPhone, switch between sessions:**

```bash
# Attach to Claude Code session
tmux attach -t claude
# Press Ctrl+B then D to detach

# Attach to general session
tmux attach -t general
# Press Ctrl+B then D to detach
```

**List all active sessions:**

```bash
tmux ls
```

---

## Section 4: Putting It All Together

### The complete daily workflow

Here's what your daily workflow looks like once everything is set up:

#### Before leaving your desk

1. Open your terminal (Git Bash or WSL2 Ubuntu) on your PC
2. Navigate to your project folder
3. Start a tmux session:
   ```bash
   tmux new -s claude
   ```
4. Start Claude Code inside the tmux session:
   ```bash
   claude
   ```
5. Start your dev server (if you're building a web app):
   ```bash
   # In a second tmux session or another terminal window
   npm run dev
   ```
6. Make sure **Tailscale** is running on your PC (check the system tray icon)

#### From your iPhone (couch, train, bed, wherever)

1. Make sure **Tailscale** is connected on your iPhone (open the app, check it says "Connected")
2. Open **Termius**
3. Tap your Windows PC connection
4. Attach to your tmux session:
   ```bash
   tmux attach -t claude
   ```
5. You're now controlling Claude Code from your phone
6. Open **Safari** and go to `http://<your-tailscale-ip>:5173` to see your web app
7. Code, test, iterate — all from your phone

#### When you're done on your phone

1. In Termius, press **Ctrl+B** then **D** to detach from tmux
2. Close Termius — your Claude Code session keeps running on your PC
3. When you're back at your desk, the tmux session is exactly where you left it

---

### Quick reference: Essential tmux commands

| Command | What it does |
|---|---|
| `tmux new -s <name>` | Create a new named session |
| `tmux attach -t <name>` | Attach to an existing session |
| `tmux ls` | List all active sessions |
| `tmux kill-session -t <name>` | Kill (permanently end) a session |
| `Ctrl+B` then `D` | Detach from session (session keeps running) |
| `Ctrl+B` then `C` | Create a new window inside the session |
| `Ctrl+B` then `N` | Switch to the next window |
| `Ctrl+B` then `P` | Switch to the previous window |
| `Ctrl+B` then `0-9` | Switch to window by number |
| `exit` | Close the current window/pane (ends session if it's the last one) |

### Quick reference: Essential Tailscale commands

Run these in **PowerShell** on your Windows PC:

| Command | What it does |
|---|---|
| `tailscale ip -4` | Show your Tailscale IP address |
| `tailscale status` | Show connection status and all devices |
| `tailscale ping <ip>` | Test connection to another device |
| `tailscale serve localhost:5173` | Share a localhost port via HTTPS |
| `tailscale serve status` | Check what's being shared |
| `tailscale serve off` | Stop sharing |

---

## Section 5: Troubleshooting

### Claude Code Remote Control issues

| Problem | Solution |
|---|---|
| `/remote-control` shows an error | Make sure you're running the latest version of Claude Code: `npm update -g @anthropic-ai/claude-code` |
| No "Interactive session" on iPhone Claude app | Make sure you're logged into the same Anthropic account on both your PC and iPhone |
| Session is laggy | This is normal — it depends on your internet speed. Try on a faster connection |

### Tailscale issues

| Problem | Solution |
|---|---|
| Devices don't see each other | Make sure both are logged into the **same** Tailscale account. Check the Admin Console at https://login.tailscale.com |
| iPhone Tailscale shows "Not Connected" | Open the app and toggle the connection. If it still fails, go to iPhone Settings > VPN > remove the Tailscale profile, then re-open the Tailscale app |
| `tailscale ping` times out | Restart Tailscale on both devices. On Windows: right-click tray icon > Disconnect, then reconnect. On iPhone: force-close the app and reopen |
| Localhost works on PC but not from iPhone | 1) Double-check the port number. 2) Try starting your dev server with `--host 0.0.0.0` flag. 3) Check Windows Firewall isn't blocking the port |

### SSH / Termius issues

| Problem | Solution |
|---|---|
| "Connection refused" | SSH service isn't running. Open PowerShell as Admin and run: `Start-Service sshd` |
| "Connection timed out" | Wrong IP address. Double-check your Tailscale IP in Step 2.4 |
| "Authentication failed" | Wrong username or password. Your username is what `whoami` returns (after the backslash). Your password is your Windows login password |
| Connects but shows cmd.exe instead of bash | You haven't set the default shell yet. Follow [Part D](#part-d-configure-ssh-default-shell) |
| SSH connects but immediately disconnects | Check that Git Bash path is correct in the registry. Run: `Get-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell` in PowerShell |

### Tmux issues (Path 1: Git Bash)

| Problem | Solution |
|---|---|
| `tmux: command not found` | Close ALL Git Bash windows and reopen. If still not found, verify `tmux.exe` exists at `C:\Program Files\Git\usr\bin\tmux.exe` |
| DLL errors when starting tmux | Re-copy the DLL files from MSYS2. Make sure the DLL versions match. Run `ls /usr/bin/msys-event*` in MSYS2 to see exact filenames |
| "open terminal failed: not a terminal" | You must use Git Bash (MinTTY terminal), NOT cmd.exe, NOT PowerShell, NOT Windows Terminal with CMD profile |
| Tmux stopped working after Git update | Git for Windows was updated and overwrote the binaries. Re-copy from MSYS2 (repeat Step 3C-P1.3) |
| `.tmux.conf` gives errors | Make sure the file uses Unix line endings (LF, not CRLF). In Git Bash: `dos2unix ~/.tmux.conf` |

### Tmux issues (Path 2: WSL2)

| Problem | Solution |
|---|---|
| `wsl --install` fails | Make sure you're running PowerShell as Administrator. Check your Windows version is 10 build 19041+ or Windows 11 |
| WSL2 is very slow | If accessing files on `/mnt/c/`, that's expected. Move your project to `/home/simon/` for better performance |
| `tmux` gives "no server running" error | This is normal — it means no tmux sessions exist yet. Create one with `tmux new -s claude` |
| Claude Code doesn't start in WSL2 | Check that Node.js is installed: `node --version`. If not, follow Step 3C-P2.5 again |

### General tips

- **Always check Tailscale first** — most connection issues come down to Tailscale not being connected on one of the devices
- **Restart the SSH service** if Termius suddenly can't connect: open PowerShell as Admin and run `Restart-Service sshd`
- **Tmux sessions survive disconnects** — if your phone loses connection, just SSH back in and `tmux attach -t claude`
- **Don't close your PC terminal** — tmux sessions run inside it. If you close the terminal window, the tmux sessions die too. (Exception: if tmux is running as a service, but that's an advanced setup)

---

## Credits

This guide is based on [chongdashu's video: "Claude Code Remote Control + Tailscale + Tmux + Termius"](https://www.youtube.com/watch?v=W_Ri0AeITiU), adapted for Windows + Git Bash + iPhone.
