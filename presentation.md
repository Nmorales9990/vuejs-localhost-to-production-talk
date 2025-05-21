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
4. Learn caching, environments, secrets, and rollback tactics

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

# **Example: Deploy to GitHub Pages**

```yaml
      - run: npm run build
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

---

# **Example: Deploy to Custom SSH Server**

```yaml
      - name: Upload via SCP
        uses: appleboy/scp-action@v0.1.3
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          password: ${{ secrets.SERVER_PASS }}
          source: "dist"
          target: "/var/www/html"
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

# **Use Environments & Secrets**

* Define secrets in GitHub Settings → Secrets
* Access them via `${{ secrets.YOUR_SECRET }}`
* Use Environments (dev, staging, prod) to control manual approvals

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
