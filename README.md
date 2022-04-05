# TL;DR instructions
### Create a vite project, add vitest
```bash
pnpm create vite
pnpm i vitest -D
```
### Add scripts to package.json
```json
"test": "vitest",
"coverage": "vitest run --coverage",
"sonar": "pnpm coverage && npx sonar-scanner -Dsonar.login=$SONAR"
# In windows %SONAR% instead of $SONAR
```

### Create a unit test file:
```ts
// src/test/App.test.ts
import App from '../App.vue'
import { test, expect } from 'vitest';
test('first test', () => {
  expect(App).toBeTruthy()
})
```
### Run the tests and coverage if you want now
```bash
pnpm test
pnpm coverage
```
### Add the coverage folder to `.gitignore`
```
coverage
```

## Configure Sonarqube
Create a project in sonarqube, remember the project key and the token
### Add a sonar-project.properties file, set your projecKey and url 
```
sonar.projectKey=vite-vitest-sonarqube
sonar.sources=src/
sonar.javascript.lcov.reportPaths=coverage/lcov.info
sonar.exclusions=dist/**
sonar.coverage.exclusions=**/*.test.*
host.url=http://localhost:9000
```

### Install sonar-scanner if not already installed
```bash
pnpm i sonar-scanner -D
```
### Configure vitest to generate lcov.info files in vitest.config.ts
```ts
import { defineConfig } from 'vite'
import Vue from '@vitejs/plugin-vue'
export default defineConfig({
  plugins: [
    Vue(),
  ],
  test: {
    coverage: {
      reporter: ['text', 'lcov']
    }
  },
})
```
### Run coverage and the scan, use your generated token
```bash
pnpm sonar
# or
npx sonar-scanner -Dsonar.login=<your_token>
```
### Go to your Sonarqube server and look at the report!