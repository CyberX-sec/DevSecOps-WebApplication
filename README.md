<h1 align="center">🔐 DevSecOps Pipeline</h1>
<p align="center">
  Built and maintained by <strong>CyberX</strong> — securing your pipeline, step by step.
</p>

---

## ⚙️ Introduction

**DevSecOps** integrates security into every phase of the software development lifecycle—from code to deployment. Instead of treating security as an afterthought, it becomes a shared responsibility and part of the automated CI/CD process.

This project implements a complete DevSecOps pipeline that:

- Scans code for vulnerabilities and secrets
- Validates code formatting
- Builds and scans Docker images
- Deploys to staging, runs security scans
- Automatically promotes to production if all checks pass

---

<details>
<summary><strong>📋 Pipeline Overview</strong></summary><br>

| Stage | Tool/Task | Description |
|-------|-----------|-------------|
| Trigger | `push` to `main` | Starts the pipeline |
| Code Scanning | Bandit, Gitleaks, Semgrep, Black | Static analysis and formatting checks |
| Notifications | Telegram Bot | Sends success/failure alerts |
| Docker Build | Docker | Builds and pushes container image |
| Image Scan | Dockle | Checks for security best practices |
| Staging Deploy | Fly.io | Deploys to staging environment |
| Web Scan | Nikto | Scans live staging app for web vulns |
| Production Deploy | Fly.io | Deploys to production if everything passes |

</details>

---

## 🔁 Workflow Breakdown

### 🧪 1. `security-check`

Performs static code analysis and gathers reports:

- ✅ **Bandit** — Python static analysis (SAST)
- ✅ **Gitleaks** — Detects hardcoded secrets or credentials
- ✅ **Semgrep** — Fast, rule-based code pattern analysis
- ✅ **Black** — Checks Python code formatting
- ✅ Merges all scan results into `full_report.json`
- ✅ Sends **Telegram notifications** on success or failure

> ❌ If any **high severity issues** are found, the pipeline fails immediately.

---

### 🐳 2. `build-and-push`

- Builds a Docker image from the source code
- Tags the image as:  
  `x7m7s7/devops:latest`
- Authenticates and pushes the image to DockerHub

---

### 🔍 3. `dockle-scan`

- Uses **Dockle** to scan the pushed Docker image
- Checks for:
  - Insecure settings
  - Vulnerable packages
  - Misconfigurations
- ❌ Pipeline fails if any `HIGH` or `CRITICAL` issues are detected

---

### 🚀 4. `deploy-staging`

- Deploys the built image to **Fly.io staging environment**
- Uses rolling updates to ensure zero downtime

---

### 🛡️ 5. `nikto-scan`

- Uses **Nikto** to scan the staging environment's HTTP surface
- Detects:
  - Insecure headers
  - Dangerous files
  - Known server vulnerabilities
- Outputs results in HTML format (`nikto_report.html`)

---

### 🌐 6. `deploy-production`

- Final production deployment via **Fly.io**
- Uses a production-specific config file:  
  `fly.production.toml`
- Triggered only if all previous steps succeed

---

## 📦 Reports & Artifacts

All security scan results are uploaded as GitHub Action artifacts:

- `bandit_output.json`
- `gitleaks_report.json`
- `semgrep_output.json`
- `black_output.json`
- `dockle-report.json`
- `nikto_report.html`
- `full_report.json`

---

## 📲 Telegram Notifications

The pipeline integrates with a Telegram Bot to alert the team of pipeline status:

- ✅ Success messages when all checks pass
- ❌ Detailed alert if Bandit, Gitleaks, or Semgrep detect issues

---

## 🔐 Environment Variables & Secrets

| Variable | Purpose |
|----------|---------|
| `IMAGE_TAG` | Docker image name/tag |
| `TELEGRAM_BOT_TOKEN` | Auth token for the Telegram bot |
| `FLY_API_TOKEN` | Token to deploy to staging |
| `FLY_API_TOKEN2` | Token to deploy to production |
| `PASS_SECRET` | DockerHub password/secret |

---

## 🎯 Project Objective

To build a **secure-by-design** CI/CD pipeline that enables:

- Rapid development with automated safety gates
- Early vulnerability detection
- Zero-touch, zero-trust deployment from commit to production

---

<p align="center">
  Made with 🛠️ and secured by <strong>CyberX</strong>
</p>
