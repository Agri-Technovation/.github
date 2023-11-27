# Agri-Technovation
## introduction:

Welcome to Agri-Technovation, a company dedicated to empowering the agriculture industry through automation and consultancy services. Our GitHub organization houses various repositories aimed at automating agricultural processes and providing valuable insights to our clients regarding the health of their plants and fields.

## main projects:
* [Itest-leaf](https://github.com/Agri-Technovation/itest-leaf) 

* [Fertiliser](https://github.com/Agri-Technovation/fertiliser) 

* [Mfw-Capture](https://github.com/Agri-Technovation/mfw-capture) 

* [Picklogger-Web](https://github.com/Agri-Technovation/picklogger-web) 

## Projects formatting:

Code formatting is done ESlint and Dotnet format. This will style the code everytime the user saves a file.

### How to add eslint to a project:

Navigate to the folder where you can find the yarn.lock and the tsconfig.json files. This is usualy in the ClientApp folder.
```shx
npm init @eslint/config


You will then be given a couple of options to choose for.
choose the option that with the arrow.


? How would you like to use ESLint? …
  To check syntax only
  To check syntax and find problems
❯ To check syntax, find problems, and enforce code style


? What type of modules does your project use? …
❯ JavaScript modules (import/export)
  CommonJS (require/exports)
  None of these


? Which framework does your project use? …
❯ React
  Vue.js
  None of these


? Does your project use TypeScript? No / >Yes


 Where does your code run? …  (Press <space> to select, <a> to toggle all, <i> to invert selection)
> Browser
  Node


? How would you like to define a style for your project? …
❯ Use a popular style guide
  Answer questions about your style


? Which style guide do you want to follow? …
❯ Standard: https://github.com/standard/eslint-config-standard-with-typescript
  XO: https://github.com/xojs/eslint-config-xo-typescript


? What format do you want your config file to be in? …
  JavaScript
  YAML
❯ JSON


? Would you like to install them now? No / ›Yes


? Which package manager do you want to use? …
  npm
❯ yarn
  pnpm
```
Add the following line in package.json under "scripts"

```sh
"eslint": "eslint '**/*.{ts,tsx}' --fix"
```
Add the following line in .eslintrc.json under "parserOptions"

```sh
"project": ["**/tsconfig.json"]
```

The linter can be run by executing the follow commmand.
```sh
yarn eslint
```

## Add eslint to your IDE

Download ESLint extensions in the VS code marketplace and add the following code to settings.json

```sh
"editor.inlineSuggest.enabled": true,
"editor.codeActionsOnSave": {
"source.fixAll.eslint": true
},
"eslint.validate": [
     "javascript",
     "javascriptreact",
     {"language": "typescript", "autoFix": true},
     {"language": "typescriptreact", "autoFix": true}
],
```
For jetbrains rider you need to install the plugin for eslint and configure the settings to format on save.






Thank you for joining us on this exciting journey as we continue to revolutionize agriculture through technology and data-driven solutions. We are committed to enhancing farm productivity, sustainability, and empowering farmers with the knowledge they need to thrive. 

