# Agri-Technovation
## introduction:

Welcome to Agri-Technovation, a company dedicated to empowering the agriculture industry through automation and consultancy services. Our GitHub organization houses various repositories aimed at automating agricultural processes and providing valuable insights to our clients regarding the health of their plants and fields.

## main projects:
* [Itest-leaf](https://github.com/Agri-Technovation/itest-leaf) 

* [Fertiliser](https://github.com/Agri-Technovation/fertiliser) 

* [Mfw-Capture](https://github.com/Agri-Technovation/mfw-capture) 

* [Picklogger-Web](https://github.com/Agri-Technovation/picklogger-web) 

## Projects formatting:

Code formatting is done ESlint and Dotnet format. This will style the code usually the user saves a file, ensuring that the good coding practice is followed by all developers.

### How to add eslint to a new project:

Navigate to the folder where you can find the yarn.lock and the tsconfig.json files. This is usually in the ClientApp folder.
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

The linter can be run by executing the follow command.
```sh
yarn eslint
```

### Add eslint to your IDE:

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

### How to add Dotnet format to a new project:

The first step is to download dotnet format globally on you computer. This can be done by running the following command:
```sh
dotnet tool install -g dotnet-format
```

Add a file called ".editorconfig" in the root of the project. This file will contain all configurations for the formatting for the c# code. refer to [itest-leaf's .editorconfig](https://github.com/Agri-Technovation/itest-leaf) as an example on how it should look.

### Add Dotnet format to your IDE:

For Visual studio code you need to add the following to settings.json

```sh
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.format": ["dotnet-format.formatDocument"]
  }
}
```
For jetbrains rider you just need to go into system preferences and select on save for the editorcofig.

## SonarCloud:

SonarCloud serves as a robust code scanning tool designed to identify code smells, bugs, vulnerabilities, and security hotspots within your project. Upon integrating a repository into SonarCloud, it conducts an initial comprehensive scan of the entire project, providing valuable feedback on the overall code quality. Our configuration ensures that SonarCloud runs seamlessly on every pull request made to the main/master branch, with a focus on analysing only the modified code. This tailored approach enhances efficiency by pinpointing potential issues in the specific areas that underwent changes. 

### Adding a new project to SonarCloud:

Go to analize new project, select the Repo that you want to add to SonarCloud and click on setup:

<img width="1178" alt="image" src="https://github.com/Agri-Technovation/.github/assets/62010068/3e7baab1-9c97-4375-99b8-5be9acfd4901">

The next step is to select the "Previous version" option and click on create project.

<img width="1145" alt="image" src="https://github.com/Agri-Technovation/.github/assets/62010068/2faadebd-a853-402a-8b06-dfe2adddb990">

Now the project is added to SonarCloud. The next step will be to add the workflow on GitHub to run analisis on pull requests.

### Adding workflow for SonarCloud:

The first step is to create a token on SonarCloud. You will find this under the securaty tab in your profile. You will need to add this token to your GitHub environment secrets of the repo. This seacret will be used in the work flow.

Create a yml file containing the following:
```sh
name: SonarCloud
on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened]
jobs:
  build:
    name: Build and analyze
    runs-on: windows-latest
    steps:
      - name: Set up JDK 17
        uses: actions/setup-java@v2
        with:
          java-version: 17
          distribution: 'adopt'
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0  # Shallow clones should be disabled for a better relevancy of analysis
          token: ${{ secrets.SUBMODULES_PAT }}
          submodules: true
      - name: Cache SonarCloud packages
        uses: actions/cache@v2
        with:
          path: ~\sonar\cache
          key: ${{ runner.os }}-sonar
      - name: Cache SonarCloud scanner
        id: cache-sonar-scanner
        uses: actions/cache@v2
        with:
          path: .\.sonar\scanner
          key: ${{ runner.os }}-sonar-scanner
      - name: Install SonarCloud scanner
        if: steps.cache-sonar-scanner.outputs.cache-hit != 'true'
        shell: powershell
        run: |
          New-Item -Path .\.sonar\scanner -ItemType Directory
          dotnet tool update dotnet-sonarscanner --tool-path .\.sonar\scanner
      - name: Build and analyze
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # Needed to get PR information, if any
          SONARCLOUD_TOKEN: ${{ secrets.SONARCLOUD_TOKEN }}
        shell: powershell
        run: |
          .\.sonar\scanner\dotnet-sonarscanner begin /k:"Agri-Technovation_Repo_Name" /o:"agri-technovation" /d:sonar.login="${{ secrets.SONARCLOUD_TOKEN }}" /d:sonar.host.url="https://sonarcloud.io" /d:sonar.exclusions="**/Migrations/**"
          dotnet build Web
          .\.sonar\scanner\dotnet-sonarscanner end /d:sonar.login="${{ secrets.SONARCLOUD_TOKEN }}"
```

This workflow will now run when a pull request is made on the main branch.

## Thank you
Thank you for joining us on this exciting journey as we continue to revolutionise agriculture through technology and data-driven solutions. We are committed to enhancing farm productivity, sustainability, and empowering farmers with the knowledge they need to thrive. 

