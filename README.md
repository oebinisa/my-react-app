# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh



To create a basic React application using Vite and set up CI pipelines for both Jenkins and GitHub Actions, follow these steps:

### 1. **Create a React App Using Vite**

First, create a new React project using Vite. Here's how you can do it:

#### Step 1: Install Vite and create a new project

Run the following commands to set up a basic Vite-based React project:

```bash
# Create a new Vite project
npm create vite@latest my-react-app -- --template react

# Navigate into the project directory
cd my-react-app

# Install dependencies
npm install
```

#### Step 2: Start the development server

```bash
npm run dev
```

This will start the Vite development server and your React app should now be running at `http://localhost:5173`.

---

### 2. **Set up CI with GitHub Actions**

Now, let's set up a GitHub Actions workflow to automate the testing and build process.

#### Step 1: Create a `.github/workflows` directory

Inside your project folder, create a folder named `.github/workflows` and inside it, create a file named `ci.yml`.

```bash
mkdir -p .github/workflows
touch .github/workflows/ci.yml
```

#### Step 2: Define the workflow in `ci.yml`

Here’s a simple configuration for testing your React app with GitHub Actions:

```yaml
name: CI for Vite React App

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout the code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

    - name: Build the app
      run: npm run build
```

This workflow:
- Runs on every push to the `main` branch or on pull requests targeting `main`.
- Checks out the repository, sets up Node.js, installs dependencies, runs tests, and builds the app.

---

### 3. **Set up CI with Jenkins**

Now, let's configure Jenkins to automate testing and building the React app.

#### Step 1: Install Jenkins

If you don't have Jenkins installed, you can follow [Jenkins Installation Guide](https://www.jenkins.io/doc/book/installing/).

#### Step 2: Create a `Jenkinsfile`

Create a `Jenkinsfile` in the root of your project to define the Jenkins pipeline.

```bash
touch Jenkinsfile
```

#### Step 3: Define the Jenkins pipeline in the `Jenkinsfile`

Here’s an example of a Jenkins pipeline that performs similar actions to GitHub Actions:

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
    }

    post {
        always {
            junit 'coverage/junit.xml'
            archiveArtifacts artifacts: 'dist/**', allowEmptyArchive: true
        }
    }
}
```

This Jenkins pipeline:
- Checks out the source code.
- Installs dependencies using `npm install`.
- Runs tests using `npm test`.
- Builds the project using `npm run build`.
- Archives the test results and build artifacts.

#### Step 4: Configure Jenkins Pipeline

1. In Jenkins, create a new job of type "Pipeline".
2. In the pipeline section, select "Pipeline script from SCM" and point it to your Git repository.
3. Jenkins will now automatically run the pipeline every time there is a change in the repository.

---

### 4. **Add Basic Test Script**

For testing, you can add a simple test to your `package.json` or create a test file.

#### Example `App.test.jsx`

```jsx
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders hello world', () => {
  render(<App />);
  const linkElement = screen.getByText(/hello world/i);
  expect(linkElement).toBeInTheDocument();
});
```

#### Update `package.json` to include testing:

```json
"scripts": {
  "test": "vitest run"
}
```

---

### Conclusion

Now you have a basic React app using Vite with continuous integration set up via both Jenkins and GitHub Actions! The app will run tests and build processes automatically every time you push changes to your GitHub repository.