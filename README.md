**Assignment-4**

**Husky:** 

Husky is a tool which is used to control code quality and ensure no errors codes are commited.

With Husky we can write script for to run before commiting which basically checks the syntax and any errors before commiting.

So with Husky we can review and remove any errors if there in code when commit.

So with this there cannot be a chance to occur error and we can prevent it before commit.

First we have to Initialize Git and Node Project

git init

npm init -y

Then install Required Dependencies

npm install --save-dev husky lint-staged eslint prettier @commitlint/{cli,config-conventional}

After that we have set Up Husky

npx husky install

Then add this to package.json scripts.

"scripts": {
  "prepare": "husky install"
}

Add Pre-Commit Hook

npx husky add .husky/pre-commit "npx lint-staged"

Configure lint-staged in package.json

"lint-staged": {
  "*.js": [
    "eslint --fix",
    "prettier --write",
    "git add" 
  ]
}

Set Up ESLint

npx eslint --init

Set Up Prettier

Create a .prettierrc file:

{
  "singleQuote": true,
  "semi": true
}

Add Commit Message Hook

npx husky add .husky/commit-msg "npx --no-install commitlint --edit $1"

Create commitlint.config.js in root:

module.exports = {
  extends: ['@commitlint/config-conventional']
};

Test the Hooks

Try committing:

With linting errors → should fail.

Poorly formatted code → should fail.

Invalid commit message → should fail.

Fix issues and try again → should succeed.

Set Up GitHub Actions for CI

Create a .github/workflows/ci.yml:

name: Lint CI

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - run: npm install
      - run: npm run lint
      
Add lint script to package.json:

"scripts": {
  "lint": "eslint ."
}
