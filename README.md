# Windows 11 Development Environment Setup

## Overview

Recommended architecture:

```text
Windows 11
├── VS Code
├── Docker Desktop
└── WSL2 Ubuntu
    ├── Git
    ├── Node.js (NVM)
    ├── Python
    ├── Docker CLI
    └── Source Code
```

Principles:

- Use Windows as the host operating system.
- Run development workloads inside Ubuntu (WSL2).
- Use Docker Desktop with the WSL2 backend.
- Connect VS Code to WSL using the Remote - WSL extension.
- Store source code inside the Linux filesystem (`~/projects`), not on Windows drives.

---

# 1. Install WSL2

Open PowerShell as Administrator:

```powershell
wsl --install
```

Restart your computer.

Verify installation:

```powershell
wsl --status
```

List installed distributions:

```powershell
wsl -l -v
```

Install Ubuntu if it is not already installed:

```powershell
wsl --install -d Ubuntu
```

Launch Ubuntu and create:

- Username
- Password

Verify:

```bash
uname -a
```

---

# 2. Configure WSL2 Resources

Create the following file:

```text
C:\Users\<WindowsUser>\.wslconfig
```

Example:

```ini
[wsl2]
memory=12GB
processors=6
swap=4GB
localhostForwarding=true
```

Recommended settings:

| Host RAM | WSL Memory |
|-----------|------------|
| 16 GB | 8 GB |
| 32 GB | 12–16 GB |
| 64 GB | 24–32 GB |

Apply changes:

```powershell
wsl --shutdown
```

Start Ubuntu again.

---

# 3. Update Ubuntu

Update package lists:

```bash
sudo apt update
sudo apt upgrade -y
```

Install common development packages:

```bash
sudo apt install -y \
curl \
wget \
git \
unzip \
zip \
build-essential \
ca-certificates \
software-properties-common
```

---

# 4. Install Git

Verify installation:

```bash
git --version
```

Configure Git:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Verify configuration:

```bash
git config --list
```

---

# 5. Install Node.js Using NVM

## Install NVM

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
```

Reload shell:

```bash
source ~/.bashrc
```

Verify:

```bash
nvm --version
```

## Install Latest LTS Version

```bash
nvm install --lts
```

Set default version:

```bash
nvm alias default lts/*
```

Verify:

```bash
node -v
npm -v
```

---

# 6. Install Python

Verify:

```bash
python3 --version
```

Install pip:

```bash
sudo apt install -y python3-pip
```

Verify:

```bash
pip3 --version
```

Install virtualenv:

```bash
pip3 install virtualenv
```

Verify:

```bash
virtualenv --version
```

---

# 7. Install Docker Desktop

Install Docker Desktop on Windows.

During installation:

- Enable WSL2 Backend
- Enable WSL Integration

After installation:

Docker Desktop

Settings → Resources → WSL Integration

Enable:

```text
Ubuntu
```

Apply and restart Docker Desktop.

---

# 8. Verify Docker Integration

Inside Ubuntu:

```bash
docker version
```

Verify Docker is accessible:

```bash
docker ps
```

Run a test container:

```bash
docker run hello-world
```

---

# 9. Verify Docker Compose

Docker Compose is included with Docker Desktop.

Verify:

```bash
docker compose version
```

Example:

```bash
docker compose up -d
```

---

# 10. Workspace Organization

Create a workspace directory:

```bash
mkdir -p ~/projects
cd ~/projects
```

Recommended structure:

```text
~/projects
│
├── clients/
│   ├── company-a/
│   │   ├── frontend
│   │   ├── backend
│   │   └── infrastructure
│   │
│   └── company-b/
│       ├── web-app
│       ├── api
│       └── mobile-backend
│
├── products/
│   ├── saas-platform
│   ├── mobile-app
│   └── internal-tools
│
├── opensource/
│   ├── package-a
│   └── package-b
│
├── playground/
│   ├── learning-nodejs
│   ├── learning-python
│   └── docker-experiments
│
└── infrastructure/
    ├── postgres
    ├── kafka
    ├── monitoring
    └── nginx
```

---

# 11. Clone Repositories

Example:

```bash
cd ~/projects/clients/company-a

git clone git@github.com:company/frontend.git
git clone git@github.com:company/backend.git
git clone git@github.com:company/infrastructure.git
```

Result:

```text
company-a
├── frontend
├── backend
└── infrastructure
```

---

# 12. Monorepo Example

```bash
cd ~/projects/products

git clone git@github.com:company/platform.git
```

Example structure:

```text
platform
├── apps
│   ├── web
│   ├── admin
│   └── api
│
├── packages
│   ├── ui
│   ├── shared
│   └── utils
│
└── infrastructure
```

---

# 13. Install VS Code

Install Visual Studio Code on Windows.

Recommended extensions:

- Remote - WSL
- Docker
- GitLens
- ESLint
- Prettier

---

# 14. Open Projects in VS Code

Open a project:

```bash
cd ~/projects/clients/company-a/frontend

code .
```

Verify VS Code shows:

```text
WSL: Ubuntu
```

This confirms VS Code is running inside WSL.

---

# 15. Create a VS Code Workspace

For projects with multiple repositories:

Create:

```text
company-a.code-workspace
```

Example:

```json
{
  "folders": [
    {
      "path": "frontend"
    },
    {
      "path": "backend"
    },
    {
      "path": "infrastructure"
    }
  ]
}
```

Open:

```bash
code company-a.code-workspace
```

---

# 16. Configure SSH Keys

Generate a key:

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

Display public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Add the key to:

- GitHub
- GitLab
- Bitbucket

Verify:

```bash
ssh -T git@github.com
```

---

# 17. Environment Variables

Recommended:

```text
frontend/
├── .env.local
├── .env.development
└── .env.production

backend/
├── .env
└── .env.example
```

Ignore secrets:

```gitignore
.env
.env.local
.env.production
```

---

# 18. Useful Shell Aliases

Edit:

```bash
nano ~/.bashrc
```

Add:

```bash
alias ll="ls -lah"
alias gs="git status"
alias gc="git commit"
alias gp="git push"
alias dps="docker ps"
alias dc="docker compose"
alias ..="cd .."
alias ...="cd ../.."
```

Reload:

```bash
source ~/.bashrc
```

---

# 19. Daily Workflow

Start WSL:

```bash
wsl
```

Go to project:

```bash
cd ~/projects/clients/company-a/frontend
```

Start infrastructure:

```bash
docker compose up -d
```

Open VS Code:

```bash
code .
```

Install dependencies:

```bash
npm install
```

Run development server:

```bash
npm run dev
```

Check running containers:

```bash
docker ps
```

Stop services:

```bash
docker compose down
```

---

# 20. Verify Installation

Verify all tools:

```bash
git --version
node -v
npm -v
python3 --version
pip3 --version
docker version
docker compose version
```

Expected output:

```text
git version 2.x
node v22.x
npm 10.x
Python 3.x
Docker 28.x
Docker Compose v2.x
```

---

# Best Practices

Do:

- Keep all source code under `~/projects`.
- Group repositories by client, product, or domain.
- Use SSH keys for Git authentication.
- Open projects using `code .` from WSL.
- Run Docker commands from WSL.
- Use NVM to manage Node.js versions.
- Store source code inside the Linux filesystem.
- Keep infrastructure repositories separate from application repositories.

Avoid:

- Working directly from `/mnt/c`.
- Storing source code in Windows folders such as:

```text
C:\Users\<user>\Documents
```

- Installing Node.js or Python directly on Windows when WSL is the primary development environment.
- Running duplicate Docker installations inside WSL when Docker Desktop is already being used.
- Sharing a single `.env` file across unrelated projects.

---

# Recommended Long-Term Structure

```text
~/projects
├── clients/
├── products/
├── opensource/
├── playground/
└── infrastructure/
```

This structure scales well from a few repositories to hundreds of projects while keeping development environments organized and maintainable.
