# GuardianTool

Welcome to my Adobe I/O Application!

## Setup

1. Follow the below steps in your terminal:
* brew install nvm (Node Version Manager)
* Check if a .nvm folder was created by typing "ls -la" in the terminal
  - if .nvm folder doesn't exist:
    * mkdir ~/.nvm
    * code .zprofile (or open file in your preferred text editor. using 'code' will open the file in VScode)
    * Add this code in the file:
      ```bash
      eval "$(/opt/homebrew/bin/brew shellenv)"

      export NVM_DIR="$HOME/.nvm"
      [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh" # This loads nvm
      [ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" # This loads nvm bash_completion      
      ```
  - type 'nvm' in the terminal to confirm it is correctly installed
  - nvm install 18: intall version 18
  - npm install -g @adobe/aio-cli
    * type 'aio' in the terminal to more info on different commands you can use
   
2. Setup an ssh key for your git corp account
   * [Creating ssh key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
   * [Adding ssh key to git account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
   * [Other Refererence article](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
  
3. Once your ssh key has been added to you git.corp account follow these steps:
   * ssh-add --apple-use-keychain ~/.ssh/id_ed25519
   * git config --get user.name "<username>"
   * git config --global user.email "<email@adobe.com>"
  
4. Cloning the git repository
   * mkdir seoguardian
   * cd seoguardian
   * git clone git@git.corp.adobe.com:wcms/starlord.git
   * cd starlord
   * git remote add upstream git@git.corp.adobe.com:wcms/starlord.git
   * git checkout -b <branchname>

5. Test if git repo is cloned and upstream is set correctly

6. Get access to Adobe IO Console (connect with you reporting manager)

7. Once you have access follow these steps in your terminal in you seoguardian folder
   * aio login - this will open a window in the browser where you will login using your credentials
   * aio console org select - Adobe Corp
   * aio console project select - Guardian
   * aio console workspace select - stage
   * aio console workspace download .env
  
8. Create your own workspace in the Adobe Developer Console
   * update workplace in your terminal using "aio console workspace select <yourworkspace>"
   * aio console workspace download .env
   * change "namespace" in the .env file to the name of your workspace
   * aio app deploy - will give you the delayed link for your workspace which will reflect your changes.

9. Run the Guardian application
    * npm install - install all the packages in package.json
    * aio app run

## Local Dev

- `aio app run` to start your local Dev server
- App will run on `localhost:9080` by default

By default the UI will be served locally but actions will be deployed and served from Adobe I/O Runtime. To start a
local serverless stack and also run your actions locally use the `aio app run --local` option.

## Test & Coverage

- Run `aio app test` to run unit tests for ui and actions
- Run `aio app test --e2e` to run e2e tests

## Deploy & Cleanup

- `aio app deploy` to build and deploy all actions on Runtime and static files to CDN
- `aio app undeploy` to undeploy the app

## Config

### `.env`

You can generate this file using the command `aio app use`. 

```bash
# This file must **not** be committed to source control

## please provide your Adobe I/O Runtime credentials
# AIO_RUNTIME_AUTH=
# AIO_RUNTIME_NAMESPACE=
```

### `app.config.yaml`

- Main configuration file that defines an application's implementation. 
- More information on this file, application configuration, and extension configuration 
  can be found [here](https://developer.adobe.com/app-builder/docs/guides/appbuilder-configuration/#appconfigyaml)

#### Action Dependencies

- You have two options to resolve your actions' dependencies:

  1. **Packaged action file**: Add your action's dependencies to the root
   `package.json` and install them using `npm install`. Then set the `function`
   field in `app.config.yaml` to point to the **entry file** of your action
   folder. We will use `webpack` to package your code and dependencies into a
   single minified js file. The action will then be deployed as a single file.
   Use this method if you want to reduce the size of your actions.

  2. **Zipped action folder**: In the folder containing the action code add a
     `package.json` with the action's dependencies. Then set the `function`
     field in `app.config.yaml` to point to the **folder** of that action. We will
     install the required dependencies within that directory and zip the folder
     before deploying it as a zipped action. Use this method if you want to keep
     your action's dependencies separated.

## Debugging in VS Code

While running your local server (`aio app run`), both UI and actions can be debugged, to do so open the vscode debugger
and select the debugging configuration called `WebAndActions`.
Alternatively, there are also debug configs for only UI and each separate action.

## Typescript support for UI

To use typescript use `.tsx` extension for react components and add a `tsconfig.json` 
and make sure you have the below config added
```
 {
  "compilerOptions": {
      "jsx": "react"
    }
  } 
```
