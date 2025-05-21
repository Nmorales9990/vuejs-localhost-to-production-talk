---
theme: gaia
paginate: true
backgroundColor: #000
color: #fff
colorPreset: dark
footer: Vue.js Global Summit 2025
---

![bg left:30% 70%](poster.png)

# **From Localhost to Production: CI/CD for Vue with GitHub Actions**

Seyyed Ali Mohammadiyeh (Max Base)
Vue.js Global Summit 25 - May 21, 2025

---

# **About Me**

**Seyyed Ali Mohammadiyeh (Max Base)**
Open-source Maintainer, GitHub
Senior Software Engineer
CTO, asrez
📧 [maxbasecode@gmail.com](mailto:maxbasecode@gmail.com)

---

# **Experience**

* 👨‍💻 GitHub: [https://github.com/basemax](https://github.com/basemax)
* 🧠 10+ years of experience in software development
* 🎓 Background in pure and applied mathematics with research experience

---

# **Session Overview**

Learn how to create a **reliable, automated CI/CD pipeline** for your Vue.js applications using **GitHub Actions**.

* Avoid manual and error-prone deployments
* Automate linting, testing, building, and deploying
* Cover static, dynamic, and Docker-based deployment targets

---

## This talk includes:

✅ Real-world CI/CD workflows
✅ Demo-ready GitHub Actions templates
✅ Deployment strategies + secrets management

---

# **Goals of This Talk**

1. Build a complete Vue CI/CD pipeline using GitHub Actions
2. Automate your Vue project from commit to deploy
3. Customize workflows for any deployment environment
4. Learn environments, secrets, talk about rollback tactics

---

# **Why Automate CI/CD?**

* ❌ Manual deployments = time wasted + human error
* ✅ Automation = faster feedback, confidence, and consistency
* ⚖️ Works the same locally, on dev, and in production

---

# **What is GitHub Actions?**

* Built-in CI/CD solution in GitHub
* Define workflows as YAML files
* Runs on push, pull\_request, schedule, etc.
* Supports caching, matrix builds, environments, secrets, Docker

---

# **CI/CD Flow for Vue.js**

1. ✨ Code (Vite + TypeScript)
2. ✏️ Lint (ESLint, Prettier)
3. ✅ Test (Vitest, Jest)
4. 🚀 Build (Vite)
5. 💾 Deploy (Static host, Docker, etc.)

---

# **Directory Structure**

GitHub actions files are located in `.github/workflows/` directory.

```bash
.vue-project/
├── .github/workflows/
│   └── deploy.yml
├── src/
├── public/
├── vite.config.ts
├── package.json
```

---

# **Step 1: Basic GitHub Action**

```yaml
# .github/workflows/deploy.yml
name: Vue CI/CD
on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run build
```

---

# **Add Linting & Testing**

```yaml
      - run: npm run lint
      - run: npm run test
```

Make sure you have scripts in `package.json`:

```json
"scripts": {
  "lint": "eslint .",
  "test": "vitest"
}
```

---

# **Deployment Targets**

* 🌐 **Static hosts**: Netlify, Vercel, GitHub Pages
* 🚨 **Dynamic servers**: Node, Nuxt SSR
* 🚧 **Docker-based servers**: VPS, cloud infra (e.g. DigitalOcean)

---

# **Use Environments & Secrets**

* Define secrets in GitHub Settings → Secrets
* Access them via `${{ secrets.YOUR_SECRET }}`
* Use Environments (dev, staging, prod) to control manual approvals

---

# **First Example: Deploy to GitHub Pages**

For your local machine:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "deploy": "vite build && gh-pages -d dist"
  }
}
```

---

# **First Example: Deploy to GitHub Pages**

For GitHub Actions:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "deploy": "vite build && gh-pages -d dist
      -u 'Max Base (Seyyed Ali Mohammadiyeh) <maxbasecode@gmail.com>'
      -r https://x-access-token:${GITHUB_TOKEN}@github.com/BaseMax/vuejs-cicd-deploy-on-github-pages.git"
  }
}
```

---

# **First Example: Deploy to GitHub Pages**

```yaml
name: 🚀 Deploy project in GitHub Pages
on:
  push:
    branches: [ main ]
jobs:
  gh-pages-deploy:
    name: 🧩 Deploying code to gh-pages branch
    runs-on: ubuntu-latest
    steps:
      - name: 🔀 Checkout code from repository
        uses: actions/checkout@v4
      - name: 🛠 Setup Node version
        uses: actions/setup-node@v4
        with:
          node-version: 20.x
      - name: 📦 Install dependencies
        run: npm install
```

# **First Example: Deploy to GitHub Pages**

```yaml
      - name: 🙍‍♂️ Setup git user
        run: |
          git config user.name "Max Base (Seyyed Ali Mohammadiyeh)"
          git config user.email "maxbasecode@gmail.com"
      - name: 🏗 Deploy project
        run: npm run deploy
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

# **Second Example: Deploy to SFTP/SSH Server**

```yaml
name: 🚀 Deploy project to SFTP server

on:
  push:
    branches: [ main ]

jobs:
  sftp-deploy:
    name: 🧩 Deploying to SSH SFTP
    runs-on: ubuntu-latest

    steps:
      - name: 🔀 Checkout code from repository
        uses: actions/checkout@v4

      - name: 🛠 Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20.x

      - name: 📦 Install dependencies
        run: npm install

      - name: 🧱 Build project
        run: npm run build

```

# **Second Example: Deploy to SFTP/SSH Server**

```yaml
      - name: 📥 Install sshpass
        run: sudo apt-get install -y sshpass

      - name: 📤 Upload dist folder to remote SFTP server
        run: |
          sshpass -p "${{ secrets.SSH_PASSWORD }}" scp -o StrictHostKeyChecking=no -P ${{ secrets.SSH_PORT }} -r dist/* ${{ secrets.SSH_USERNAME }}@${{ secrets.SSH_IP }}:${{ secrets.SSH_PATH }}
```

---

# **Third Example: Deploy to FTP/CPanel Host**

```yaml
name: 🚀 Deploy project via FTP to cPanel

on:
  push:
    branches: [ main ]

jobs:
  ftp-deploy:
    name: 📡 Deploying via FTP to cPanel
    runs-on: ubuntu-latest

    steps:
      - name: 🔀 Checkout code from repository
        uses: actions/checkout@v4

      - name: 🛠 Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20.x

      - name: 📦 Install dependencies
        run: npm install

      - name: 🧱 Build project
        run: npm run build

      - name: 📥 Install lftp
        run: sudo apt-get install -y lftp
```

---

# **Third Example: Deploy to FTP/CPanel Host**

```
      - name: 📤 Upload via FTP with custom port
        run: |
          lftp -u "${{ secrets.FTP_USERNAME }},${{ secrets.FTP_PASSWORD }}" -p ${{ secrets.FTP_PORT }} ${{ secrets.FTP_HOST }} -e "
            set dns:order inet;
            set ftp:ssl-allow no;
            set ssl:verify-certificate no;
            mirror -R -e -n dist ${{ secrets.FTP_PATH }};
            bye
          "
```

---

# **Use Caching to Speed Up**

```yaml
      - uses: actions/cache@v3
        with:
          path: ~/.npm
          key: npm-${{ hashFiles('package-lock.json') }}
```

---

# **Rollback Strategies**

* Keep previous release in a backup folder
* Add health check after deploy
* If failed, restore previous version

```yaml
      - name: Health Check
        run: curl --fail https://example.com || exit 1
```

---

# **Tips & Best Practices**

* One job per stage (lint, test, build, deploy)
* Use matrix to test on multiple Node versions
* Keep workflows short and readable
* Secure secrets, never hardcode credentials

---

# **Full Workflow Overview**

```yaml
on: [push]
jobs:
  lint:
    ...
  test:
    ...
  build:
    ...
  deploy:
    ...
```

---

# **Demo Time!**

Let’s walk through a real Vue project using GitHub Actions end-to-end:
✅ Lint → ✅ Test → ✅ Build → ✅ Deploy

---

# **Recap**

* CI/CD saves time, prevents bugs, and increases confidence
* GitHub Actions + Vue = scalable automation
* Build once, deploy anywhere (static or dynamic)

---

# **Thank You!**

🔗 [github.com/basemax](https://github.com/basemax)
📧 [maxbasecode@gmail.com](mailto:maxbasecode@gmail.com)
🌟 Happy Coding!
