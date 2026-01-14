# Setup Salesforce CLI Action

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Setup%20Salesforce%20CLI-blue.svg)](https://github.com/marketplace/actions/setup-salesforce-cli)
[![GitHub release](https://img.shields.io/github/v/release/rdbumstead/setup-salesforce-action)](https://github.com/rdbumstead/setup-salesforce-action/releases)
[![Test Action](https://github.com/rdbumstead/setup-salesforce-action/actions/workflows/test.yml/badge.svg)](https://github.com/rdbumstead/setup-salesforce-action/actions/workflows/test.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> Flexible, fast, and production-ready Salesforce CLI setup with JWT authentication and optional tooling.

Created by [Ryan Bumstead](https://ryanbumstead.com) | [GitHub](https://github.com/rdbumstead) | [Consulting](mailto:ryan@ryanbumstead.com)

---

## ✨ Features

- 🚀 **Lightning Fast** - 20-45 seconds setup with intelligent caching (vs 2-3 min without)
- 🔐 **Secure JWT Auth** - Non-interactive, certificate-based authentication
- ⚙️ **Fully Optional** - Install only what you need, nothing more
- 🎯 **Flexible Config** - From minimal CLI-only to full-stack development
- 📦 **Smart Caching** - Automatic dependency caching across runs
- 🛠️ **Dev Tools** - Optional Prettier, ESLint, LWC Jest integration
- 🔌 **Plugin Ready** - Optional delta deployments and code analysis
- ⚡ **Zero Bloat** - Default setup is minimal and fast

---

## 🚀 Quick Start

### Minimal Setup (Just CLI + Auth)

```yaml
- name: Setup Salesforce
  uses: rdbumstead/setup-salesforce-action@v1
  with:
    jwt_key: ${{ secrets.SFDX_JWT_KEY }}
    client_id: ${{ secrets.SFDX_CLIENT_ID }}
    username: ${{ vars.SFDX_USERNAME }}
```

**Setup time:** ~20 seconds (cached) | ~1.5 minutes (first run)

---

### Recommended Setup (Delta + Scanner)

```yaml
- name: Setup Salesforce
  uses: rdbumstead/setup-salesforce-action@v1
  with:
    jwt_key: ${{ secrets.SFDX_JWT_KEY }}
    client_id: ${{ secrets.SFDX_CLIENT_ID }}
    username: ${{ vars.SFDX_USERNAME }}
    install_delta: "true"
    install_scanner: "true"
```

**Setup time:** ~35 seconds (cached) | ~2 minutes (first run)

---

### Full-Stack Development

```yaml
- name: Setup Salesforce
  uses: rdbumstead/setup-salesforce-action@v1
  with:
    jwt_key: ${{ secrets.SFDX_JWT_KEY }}
    client_id: ${{ secrets.SFDX_CLIENT_ID }}
    username: ${{ vars.SFDX_USERNAME }}
    install_delta: "true"
    install_scanner: "true"
    install_prettier: "true"
    install_eslint: "true"
    install_lwc_jest: "true"
```

**Setup time:** ~45 seconds (cached) | ~2.5 minutes (first run)

---

## 📋 Inputs

### Authentication Inputs

_Required unless `skip_auth: 'true'` is set_

| Name        | Description                                               |
| ----------- | --------------------------------------------------------- |
| `jwt_key`   | JWT private key for authentication (entire file contents) |
| `client_id` | Connected App consumer key                                |
| `username`  | Salesforce username                                       |

### Configuration

| Name           | Default     | Description                        |
| -------------- | ----------- | ---------------------------------- |
| `is_dev_hub`   | `false`     | Set as default Dev Hub             |
| `node_version` | `20`        | Node.js version to use             |
| `skip_auth`    | `false`     | Skip JWT authentication (CLI only) |
| `alias`        | `TargetOrg` | Alias name for authenticated org   |

### Salesforce Plugins

| Name              | Default | Description                              |
| ----------------- | ------- | ---------------------------------------- |
| `install_scanner` | `false` | Install @salesforce/plugin-code-analyzer |
| `install_delta`   | `false` | Install sfdx-git-delta                   |

### Development Tools

| Name               | Default | Description                             |
| ------------------ | ------- | --------------------------------------- |
| `install_prettier` | `false` | Install Prettier and Salesforce plugins |
| `install_eslint`   | `false` | Install ESLint and Salesforce plugins   |
| `install_lwc_jest` | `false` | Install sfdx-lwc-jest for LWC testing   |

---

## 🛠️ Setup Guide

<details>
<summary><b>Step 1: Create Connected App in Salesforce</b></summary>

### Generate Certificate

Open your terminal:

```bash
openssl req -x509 -nodes -newkey rsa:2048 -keyout server.key -out server.crt -days 365 -subj "/CN=SalesforceDevOps"
```

### Configure Connected App

1. Salesforce Setup → App Manager → New Connected App
2. Basic Info:
   - Name: `GitHub Actions CI/CD`
   - Contact Email: your email
3. Enable OAuth Settings
4. Enable "Use digital signatures"
5. Upload `server.crt`
6. Add OAuth Scopes: `full`, `refresh_token`, `offline_access`
7. Save and wait 2-10 minutes
8. Copy the Consumer Key

[Full Documentation](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_auth_jwt_flow.htm)

</details>

<details>
<summary><b>Step 2: Configure GitHub Secrets</b></summary>

Repository → Settings → Secrets and variables → Actions

**Add Secrets:**

`SFDX_JWT_KEY`: Paste entire contents of `server.key` including BEGIN/END lines

`SFDX_CLIENT_ID`: Paste Consumer Key from Salesforce (starts with 3MVG...)

**Add Variables:**

`SFDX_USERNAME`: Your Salesforce username (e.g., admin@company.com)

</details>

<details>
<summary><b>Step 3: Test Your Setup</b></summary>

Create `.github/workflows/test.yml`:

```yaml
name: Test Setup
on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Salesforce
        uses: rdbumstead/setup-salesforce-action@v1
        with:
          jwt_key: ${{ secrets.SFDX_JWT_KEY }}
          client_id: ${{ secrets.SFDX_CLIENT_ID }}
          username: ${{ vars.SFDX_USERNAME }}

      - name: Verify
        run: sf org display
```

</details>

---

## 🎯 Use Cases

<details>
<summary><b>Minimal Apex Deployment</b></summary>

```yaml
name: Deploy Apex

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Salesforce
        uses: rdbumstead/setup-salesforce-action@v1
        with:
          jwt_key: ${{ secrets.SFDX_JWT_KEY }}
          client_id: ${{ secrets.SFDX_CLIENT_ID }}
          username: ${{ vars.SFDX_USERNAME }}

      - name: Deploy
        run: sf project deploy start --source-dir force-app
```

</details>

<details>
<summary><b>Delta Deployment Pipeline</b></summary>

```yaml
name: Delta Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Salesforce
        uses: rdbumstead/setup-salesforce-action@v1
        with:
          jwt_key: ${{ secrets.SFDX_JWT_KEY }}
          client_id: ${{ secrets.SFDX_CLIENT_ID }}
          username: ${{ vars.SFDX_USERNAME }}
          install_delta: "true"

      - name: Generate Delta
        run: |
          sf sgd:source:delta --to HEAD --from HEAD^ --output changed-sources/

      - name: Deploy Delta
        run: |
          sf project deploy start \
            --manifest changed-sources/package/package.xml \
            --test-level RunLocalTests
```

</details>

<details>
<summary><b>Code Quality Check</b></summary>

```yaml
name: Quality Check

on:
  pull_request:
    branches: [main]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Salesforce
        uses: rdbumstead/setup-salesforce-action@v1
        with:
          jwt_key: ${{ secrets.SFDX_JWT_KEY }}
          client_id: ${{ secrets.SFDX_CLIENT_ID }}
          username: ${{ vars.SFDX_USERNAME }}
          install_scanner: "true"
          install_prettier: "true"

      - name: Check Formatting
        run: prettier --check "force-app/**/*.{cls,js,html}"

      - name: Scan Code
        run: sf scanner:run --target force-app --severity-threshold 2
```

</details>

<details>
<summary><b>LWC Development with Testing</b></summary>

```yaml
name: LWC Tests

on:
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Salesforce
        uses: rdbumstead/setup-salesforce-action@v1
        with:
          jwt_key: ${{ secrets.SFDX_JWT_KEY }}
          client_id: ${{ secrets.SFDX_CLIENT_ID }}
          username: ${{ vars.SFDX_USERNAME }}
          install_eslint: "true"
          install_lwc_jest: "true"

      - name: Lint JavaScript
        run: eslint force-app/**/lwc/**/*.js

      - name: Run Unit Tests
        run: npm run test:unit
```

</details>

<details>
<summary><b>Multi-Environment Deployment</b></summary>

```yaml
name: Multi-Env Deploy

on:
  push:
    branches: [main]

jobs:
  dev:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Salesforce (Dev)
        uses: rdbumstead/setup-salesforce-action@v1
        with:
          jwt_key: ${{ secrets.SFDX_JWT_KEY }}
          client_id: ${{ secrets.SFDX_CLIENT_ID }}
          username: ${{ vars.SFDX_DEV_USERNAME }}

      - run: sf project deploy start --source-dir force-app

  prod:
    needs: dev
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v4

      - name: Setup Salesforce (Prod)
        uses: rdbumstead/setup-salesforce-action@v1
        with:
          jwt_key: ${{ secrets.SFDX_JWT_KEY }}
          client_id: ${{ secrets.SFDX_CLIENT_ID }}
          username: ${{ vars.SFDX_PROD_USERNAME }}
          install_delta: "true"

      - run: |
          sf sgd:source:delta --to HEAD --from HEAD^ --output changed/
          sf project deploy start --manifest changed/package/package.xml
```

</details>

<details>
<summary><b>CLI Installation Only (No Auth)</b></summary>

```yaml
name: Install CLI

on: [push]

jobs:
  setup:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Salesforce CLI
        uses: rdbumstead/setup-salesforce-action@v1
        with:
          skip_auth: "true"
          install_delta: "true"

      - name: Custom Auth
        run: |
          # Your custom authentication logic here
          sf org login web
```

</details>

<details>
<summary><b>Multi-Org Setup</b></summary>

Authenticate to multiple orgs in the same workflow:

    name: Multi-Org Workflow

    on:
      push:
        branches: [main]

    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v4

          - name: Setup Development Org
            uses: rdbumstead/setup-salesforce-action@v1
            with:
              jwt_key: ${{ secrets.SFDX_JWT_KEY }}
              client_id: ${{ secrets.SFDX_CLIENT_ID }}
              username: ${{ vars.DEV_USERNAME }}
              alias: "dev"
              install_delta: "true"

          - name: Setup Production Org
            uses: rdbumstead/setup-salesforce-action@v1
            with:
              jwt_key: ${{ secrets.SFDX_JWT_KEY }}
              client_id: ${{ secrets.SFDX_CLIENT_ID }}
              username: ${{ vars.PROD_USERNAME }}
              alias: "prod"
              install_delta: "true"

          - name: Deploy to Development
            run: |
              sf project deploy start --target-org dev --test-level NoTestRun

          - name: Deploy to Production
            run: |
              sf project deploy start --target-org prod --test-level RunLocalTests

</details>

---

## ⚡ Performance Comparison

| Configuration      | Tools                 | Cache Hit | Cache Miss |
| ------------------ | --------------------- | --------- | ---------- |
| Minimal            | CLI only              | 20s       | 1.5 min    |
| Recommended        | CLI + delta + scanner | 35s       | 2 min      |
| Full Stack         | All tools             | 45s       | 2.5 min    |
| CLI Only (no auth) | CLI + plugins         | 25s       | 1.8 min    |

**Why so fast?**

- Intelligent caching of CLI and plugins
- Conditional installation (skip if cached)
- Parallel npm operations
- No redundant downloads

---

## 🤝 Related Projects

- **[Salesforce Pipeline Templates](https://github.com/rdbumstead/sfdx-pipeline-templates)** - Complete CI/CD workflows
- **[Portfolio](https://ryanbumstead.com)** - Salesforce architecture portfolio

---

## 📊 Comparison with Alternatives

| Feature               | This Action | Manual Setup | Salesforce Official |
| --------------------- | ----------- | ------------ | ------------------- |
| Setup Time (cached)   | 20-45s      | N/A          | 60-90s              |
| JWT Auth Built-in     | ✅          | ❌           | ❌                  |
| Optional Plugins      | ✅          | ❌           | ❌                  |
| Dev Tools Integration | ✅          | ❌           | ❌                  |
| Smart Caching         | ✅          | ❌           | ⚠️ Basic            |
| Skip Auth Option      | ✅          | ✅           | ❌                  |
| Minimal by Default    | ✅          | N/A          | ❌                  |

---

## 🔧 Troubleshooting

<details>
<summary><b>Authentication Failed</b></summary>

**Error:** `ERROR running org:login:jwt: We encountered a JSON web token error`

**Solutions:**

1. Verify JWT key includes BEGIN/END lines
2. Wait 2-10 minutes after creating Connected App
3. Check username matches org
4. Verify Consumer Key is correct
5. Ensure Connected App has OAuth scopes

</details>

<details>
<summary><b>Plugin Installation Timeout</b></summary>

**Error:** `Plugin installation timed out`

**Solutions:**

1. Retry workflow (intermittent npm issues)
2. Check internet connectivity
3. Disable unused plugins

</details>

<details>
<summary><b>Cache Not Working</b></summary>

**Symptom:** Setup takes 2+ minutes every time

**Solutions:**

1. Check cache hit/miss in logs
2. Verify actions/cache@v4 permissions
3. Ensure cache hasn't exceeded 10GB limit

</details>

---

## 🗺️ Roadmap

- [ ] Custom CLI version support
- [ ] Additional plugin installation (custom list)
- [ ] Windows/macOS runner support
- [ ] Sandbox URL configuration
- [ ] Integration with Salesforce Code Analyzer v4

---

## 📝 Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.

---

## 🤝 Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## 📄 License

MIT © [Ryan Bumstead](https://ryanbumstead.com)

---

## 💬 Support

- 🐞 [Report Bug](https://github.com/rdbumstead/setup-salesforce-action/issues)
- 💡 [Request Feature](https://github.com/rdbumstead/setup-salesforce-action/issues)
- 📧 [Consulting](mailto:ryan@ryanbumstead.com)
- 💼 [LinkedIn](https://linkedin.com/in/ryanbumstead)
- 🌐 [Portfolio](https://ryanbumstead.com)

---

**Built with ❤️ for the Salesforce Community**

⭐ Star this repo if it helped you!
