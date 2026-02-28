# Claude Code Remote Control Setup

**Code from your iPhone while AI runs on your Windows PC — using Tmux, Termius, and Tailscale.**

This repo contains a complete, beginner-friendly guide for setting up a remote coding workflow on **Windows + Git Bash + iPhone**.

## The Stack

| Tool | Role |
|---|---|
| **Tmux** | Keeps terminal sessions alive and synced between your PC and phone |
| **Termius** | SSH client on your iPhone — gives you a full terminal on your phone |
| **Tailscale** | Secure private network between your devices — access localhost from your phone |

Plus **Claude Code Remote Control** as a lightweight alternative for quick prompt-sending.

## What You Get

- A persistent terminal session on your PC that you can attach to from your iPhone
- Full two-way sync — type on your phone, see it on your PC, and vice versa
- Access to `localhost` on your phone to preview what you're building
- Works with Claude Code, Codex, or any CLI tool

## Guide

See **[claude-code-remote-control.md](claude-code-remote-control.md)** for the full step-by-step setup.

The guide covers:
1. Claude Code Remote Control (quickest, no extra tools)
2. Tailscale setup (Windows + iPhone)
3. SSH server setup on Windows
4. Termius setup on iPhone
5. Tmux installation (two paths: Git Bash or WSL2)
6. The daily workflow — putting it all together
7. Troubleshooting

## Acknowledgement

This project was inspired by the YouTube video ["Claude Code Remote Control is here... But Has A Problem. Here's How To Fix it."](https://www.youtube.com/watch?v=W_Ri0AeITiU) by chongdashu.
