# REACT DESKTOP APP
## Necessary packages
* [Node](https://nodejs.org/es/ 'Node')
* [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm 'npm')
* [Yarn](https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable 'Yarn')
### Dont necessary in global
* [Electron](https://www.electronjs.org/ 'Electron')

### Download project
* Download
* run `yarn`
* run in dev `yarn electron-dev`

<details><summary><h2>Create and configure from scratch<h2></summary>
  
<p>  

## Create react app
``` bash
npx create-react-app react-desktop-app
```
``` bash
cd react-desktop-app
```
``` bash
yarn
```
``` bash
yarn start
```
## Add electron to react app
``` bash
yarn add electron electron-builder --dev
```
``` bash
yarn add wait-on concurrently --dev
```
``` bash
yarn add electron-is-dev
```
## Create electron configs  
### public/electron.js
``` js
const electron = require('electron');
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;

const path = require('path');
const isDev = require('electron-is-dev');

let mainWindow;

function createWindow() {
  mainWindow = new BrowserWindow({width: 900, height: 680});
  mainWindow.loadURL(isDev ? 'http://localhost:3000' : `file://${path.join(__dirname, '../build/index.html')}`);
  if (isDev) {
    // Open the DevTools.
    //BrowserWindow.addDevToolsExtension('<location to your react chrome extension>');
    mainWindow.webContents.openDevTools();
  }
  mainWindow.on('closed', () => mainWindow = null);
}

app.on('ready', createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (mainWindow === null) {
    createWindow();
  }
});
```
## Add command to package.json
### before scripts
``` json
"main": "public/electron.js",
```
### inside scripts
``` json
"electron-dev": "concurrently \"yarn start\" \"wait-on http://localhost:3000 && electron .\""
```
## Create .env in /
``` env
BROWSER=none
```
## Run app
``` bash
yarn electron-dev
```
## Warning Content-Security-Policy
### public/index.html
#### after utf-8
``` html
<meta http-equiv="Content-Security-Policy" content="script-src 'self'" />
```
#### TO CREATE THE EXE, REMOVE THIS
</p>

</details>
  
<details><summary><h2>Optional config<h2></summary>
  
<p> 
  
## public/electron.js
### BrowserWindow
``` js
mainWindow = new BrowserWindow({
    width: 900,
    height: 680,
    titleBarStyle: 'hidden',
    titleBarOverlay: {
        color: '#2f3241',
        symbolColor: '#74b1be'
    }
});
```
### before createWindow
``` js
process.env['ELECTRON_DISABLE_SECURITY_WARNINGS'] = 'true';
```
## public/index.html
### body
``` html
<body style="padding: 0; margin:0">
  <div style="-webkit-app-region: drag; width: 100%;height: 35px; background: #2f3241">
    <h2 style="padding: 0; margin:0; color: white">&nbsp;create-react-app</h2>
  </div>
  <div id="root"></div>
</body>
```
</p>

</details>

<details><summary><h2>Build and executable configuration<h2></summary>
  
<p>    
  
## Add command to package.json
### before scripts
``` json
"author": "ezker"
"homepage": "./",
"license": "MIT"
```
### inside scripts
``` json
"electron-pack": "electron-builder -c.extraMetadata.main=build/electron.js",
"preelectron-pack": "yarn build"
```
### after devDependencies
``` json
"build": {
  "appId": "com.ezker.react-desktop-app",
  "files": [
    "build/**/*",
    "node_modules/**/*"
  ],
  "directories": {
    "buildResources": "assets"
  }
}
```
## Create exe
``` bash
yarn electorn-pack --win
```
  
</p>

</details>
