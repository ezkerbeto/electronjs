# REACT DESKTOP APP
## Necessary packages
* [Node](https://nodejs.org/es/ 'Node')
* [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm 'npm')
* [Yarn](https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable 'Yarn')
* [Electron](https://www.electronjs.org/ 'Electron')
<details><summary><h2>Create and configure<h2></summary>
  
<p>  


## Create react app
```bash
npx create-react-app react-desktop-app
```
```bash
cd react-desktop-app
```
```bash
yarn
```
```bash
yarn start
```
## Add electron to react app
```
yarn add electron electron-builder –dev
yarn add wait-on concurrently –dev
yarn add electron-is-dev
```
## Create electron configs  
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
```bash
yarn react-app-dev
```
</p>

</details>
