### **🚀 GitHub Actions: The 80/20 Rule Course – Example Workflows**

Here are example YAML workflows for each module, following best practices and real-world scenarios.

---

## **🔹 Module 1: Basic GitHub Actions Workflow**

📌 **Trigger: Runs on push to `main` branch**  
📌 **Steps:** Checkout code → Print "Hello World"  
📌 **File:** `.github/workflows/hello-world.yml`

```yaml
name: Hello World

on: push  # Triggers when code is pushed

jobs:
  say_hello:
    runs-on: ubuntu-latest
    steps:
      - name: Print a message
        run: echo "Hello, GitHub Actions!"
```

---

## **🔹 Module 2: Workflows & Triggers**

📌 **Trigger: Runs on `pull_request` events**  
📌 **Steps:** Checkout code → Run tests  
📌 **File:** `.github/workflows/pr-check.yml`

```yaml
name: Pull Request Check

on:
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Run tests
        run: echo "Running tests..."
```

---

## **🔹 Module 3: Runners & Secrets**

📌 **Trigger: Manual trigger (`workflow_dispatch`)**  
📌 **Uses:** Secrets & Environment variables  
📌 **File:** `.github/workflows/env-secrets.yml`

```yaml
name: Environment and Secrets Demo

on: 
  workflow_dispatch  # Manually triggered

jobs:
  demo:
    runs-on: ubuntu-latest
    env:
      APP_ENV: production  # Define an environment variable

    steps:
      - name: Print Environment Variable
        run: echo "Environment: $APP_ENV"

      - name: Use a Secret
        run: echo "API Key is ${{ secrets.API_KEY }}" # Store API_KEY in GitHub secrets
```

---

## **🔹 Module 4: Using & Writing Custom Actions**

📌 **Trigger: Runs on push**  
📌 **Uses:** Prebuilt GitHub Actions (`checkout`, `setup-node`)  
📌 **File:** `.github/workflows/setup-node.yml`

```yaml
name: Setup Node.js

on: push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: Install Dependencies
        run: npm install
```

📌 **Writing a Custom GitHub Action (JavaScript)**  
Create a folder `my-action` with these files:

🔹 **Action Definition (`action.yml`)**

```yaml
name: 'My Custom Action'
description: 'Prints a custom message'
runs:
  using: 'node16'
  main: 'index.js'
```

🔹 **Action Code (`index.js`)**

```javascript
const core = require('@actions/core');

try {
  const message = core.getInput('message') || 'Hello from Custom Action!';
  console.log(message);
} catch (error) {
  core.setFailed(error.message);
}
```

Use this action in a workflow:

```yaml
jobs:
  custom-action-demo:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ./my-action
        with:
          message: "Hello from my custom action!"
```

---

## **🔹 Module 5: Caching & Artifacts**

📌 **Trigger: Runs on push**  
📌 **Uses:** Caching npm dependencies  
📌 **File:** `.github/workflows/cache-dependencies.yml`

```yaml
name: Cache Dependencies

on: push

jobs:
  cache:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Cache Node.js Modules
        uses: actions/cache@v4
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: ${{ runner.os }}-node-

      - name: Install Dependencies
        run: npm install
```

---

## **🔹 Module 6: CI/CD with GitHub Actions**

📌 **Trigger: Push to `main`**  
📌 **Uses:** Test & Deploy a Node.js app  
📌 **File:** `.github/workflows/ci-cd.yml`

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Production
        run: echo "Deploying application..."
```

---

## **🔹 Module 7: Security & Best Practices**

📌 **Trigger: Pull request**  
📌 **Uses:** Dependabot & Secret Scanning  
📌 **File:** `.github/dependabot.yml`

```yaml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
```

📌 **Adding a Linter Check (`.github/workflows/linter.yml`)**

```yaml
name: Lint Codebase

on: pull_request

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: Install Dependencies
        run: npm install

      - name: Run Linter
        run: npm run lint
```

---

## **🚀 Final Project: Full CI/CD Pipeline**

📌 **Trigger: Push to `main`**  
📌 **Uses:** Build → Test → Deploy (with manual approval)  
📌 **File:** `.github/workflows/full-ci-cd.yml`

```yaml
name: Full CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Manual Approval Required
        run: echo "Waiting for approval..."

      - name: Deploy to Production
        run: echo "Deploying application..."
```

---

## **🎯 What’s Next?**

1. **Try modifying these workflows for your projects.**
2. **Explore GitHub Actions Marketplace** for more prebuilt actions.
3. **Set up self-hosted runners** if needed for enterprise solutions.

🔥 **You're now ready to automate your workflows like a pro!** 🚀💡