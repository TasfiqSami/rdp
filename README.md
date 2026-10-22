<div align="center">

# 🖥️ Windows RDP Workspace

### A GitHub Actions-powered Windows desktop, accessed securely through Tailscale

<p>
  <img src="https://img.shields.io/badge/Platform-Windows%20Server-blue?style=for-the-badge&logo=windows" alt="Windows">
  <img src="https://img.shields.io/badge/Automation-GitHub%20Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" alt="GitHub Actions">
  <img src="https://img.shields.io/badge/Network-Tailscale-242424?style=for-the-badge&logo=tailscale&logoColor=white" alt="Tailscale">
</p>

**Start the workflow · Connect over Tailscale · Sign in · Let your desktop personalize itself**

</div>

---

## ✨ What this workflow does

This repository workflow starts a Windows runner and prepares it for Remote Desktop access. After you sign in to the `Tasfiq` desktop, scheduled tasks apply dark mode and begin installing the selected applications.

| Feature | What to expect |
|---|---|
| 🔐 Remote access | RDP is intended to be reachable through Tailscale, not exposed publicly |
| 👤 Windows account | Creates the local account `Tasfiq` using your GitHub secret |
| 🌙 Dark mode | Applies Windows dark-mode settings after interactive sign-in |
| 📦 App setup | Starts the configured application and CLI installations after sign-in |
| 🧾 Install log | Writes installation output to the desktop log file when the script runs |

## 📥 Applications configured

The workflow attempts to install the following apps and command-line tools. Availability and successful installation can vary by package, runner image, and permissions.

### Desktop applications

- Google Antigravity
- Brave Browser
- Visual Studio Code
- Neat Download Manager
- Discord
- Git
- GitHub CLI
- Node.js LTS
- ChatGPT desktop app (best effort)

### Developer CLIs

- OpenAI Codex CLI (`@openai/codex`)
- Claude Code (`@anthropic-ai/claude-code`)
- Vercel CLI (`vercel`)
- GitHub Copilot CLI (`@github/copilot`)
- Google Antigravity CLI (best effort)

---

## 🚀 Setup guide

### 1. Add the workflow file

Create this file in your repository:

```text
.github/
└── workflows/
    └── rdp.yml
```

Paste the complete workflow YAML into `rdp.yml` and commit the file.

### 2. Add GitHub Actions secrets

Open **Repository → Settings → Secrets and variables → Actions**, then add these repository secrets:

| Secret name | Value |
|---|---|
| `RDP_PASSWORD` | A strong password for the local Windows account `Tasfiq` |
| `TAILSCALE_AUTH_KEY` | An authentication key created in your Tailscale account |

> [!IMPORTANT]
> Add secrets through GitHub's secret settings. Do not paste passwords or authentication keys into the YAML file, commit history, screenshots, or public issues.

**Password tips:** use a long, unique password; avoid the account name and any personal details. Follow the workflow's password requirements.

### 3. Run the workflow

1. Open the repository's **Actions** tab.
2. Select the **RDP** workflow.
3. Choose **Run workflow**.
4. Open the running job and wait for setup to complete.
5. Find the Tailscale IP in the workflow logs.
6. Connect with Remote Desktop using that IP and the `Tasfiq` account.

### 4. Sign in and let setup run

Once you're fully signed in to the Windows desktop:

- Windows dark mode is scheduled to apply.
- Application installation starts in the interactive user session.
- Keep the RDP session connected while the setup runs.
- Check the desktop for `Install-Everything.log` if the workflow creates it and you need to troubleshoot.

> [!NOTE]
> The scheduled tasks trigger at user logon. If you sign in before the tasks have been registered, sign out and sign in again after registration so the logon triggers can run.

---

## 🔒 Security checklist

- [ ] Keep `RDP_PASSWORD` and `TAILSCALE_AUTH_KEY` in GitHub Secrets.
- [ ] Use a Tailscale auth key with the shortest practical lifetime and suitable restrictions.
- [ ] Do not expose TCP port `3389` to the public internet.
- [ ] Never share workflow logs if they contain sensitive connection details.
- [ ] Stop the workflow when you no longer need the runner.

## ⚠️ Important limitations

- **Temporary machine:** GitHub-hosted runners are ephemeral. Files and installed apps generally disappear when the job ends.
- **Runtime limit:** GitHub-hosted jobs have a finite maximum duration. This workflow is configured for up to **6 hours**, not permanent hosting.
- **Installations are best effort:** WinGet or npm installations may fail because of package availability, permissions, network issues, or installer behavior.
- **Interactive setup:** The dark-mode and installation tasks are designed to start after the `Tasfiq` user logs in. Some installers may still need manual attention.
- **Check the logs:** A workflow being successful does not guarantee every optional app installed successfully. Review `Install-Everything.log` on the desktop when available.

---

## 🛠️ Troubleshooting

<details>
<summary><strong>RDP cannot connect</strong></summary>

- Confirm the GitHub Actions job is still running.
- Confirm Tailscale connected successfully and use the Tailscale IP shown in the logs.
- Verify the device is authorized in your tailnet and that your tailnet policy allows the connection.
- Confirm the workflow's firewall rule and RDP service setup completed successfully.

</details>

<details>
<summary><strong>Dark mode did not apply</strong></summary>

- Make sure you signed in as `Tasfiq`.
- Check whether the logon task was registered before you signed in.
- If it was registered after sign-in, sign out and sign back in.

</details>

<details>
<summary><strong>Some applications did not install</strong></summary>

- Look for `Install-Everything.log` on the desktop.
- Check which package failed and whether it needs elevated permissions or a different installer.
- Keep in mind that ChatGPT desktop and Antigravity CLI installation are best-effort steps.

</details>

<details>
<summary><strong>The session suddenly stopped</strong></summary>

The GitHub Actions job may have reached its maximum runtime, been cancelled, or failed. Check the Actions run summary and logs.

</details>

---

<div align="center">

**Made for a temporary, personalized Windows workspace.**

<sub>Keep credentials private. Use only on repositories and accounts you control.</sub>

</div>
