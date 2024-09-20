![image.png](https://cdn.nlark.com/yuque/0/2023/png/26428688/1675753425204-482ff8ba-4c40-4983-8935-645ca65113b2.png#averageHue=%23f8f8f8&clientId=u1f1945c5-3e1b-4&from=paste&height=574&id=u2e9ff888&originHeight=1148&originWidth=1080&originalType=binary&ratio=1&rotation=0&showTitle=false&size=108145&status=done&style=none&taskId=uec456db3-e161-407b-bcc8-ca77e39f2b0&title=&width=540)

# 通讯
```javascript
// contextIsolation: false

// on main process index.js
ipcMain.on('event-name', (event, data) => {
  const value = 'return value';
  event.reply(data.waitingEventName, value);
});

// on render frame index.html
const send =(callback)=>{
  const waitingEventName = 'event-name-reply';
  
  ipcRenderer.once(waitingEventName, (event, data) => {
    callback(data);
  });
  
  ipcRenderer.send('event-name', {waitingEventName});
};

send((value)=>{
  console.log(value);
});

```

**renderer to main to renderer**
An example of sending and handling messages between the renderer and main processes
```javascript
// In main process
const {ipcMain} = require('electron');

// sync 
ipcMain.on('synchronous-message', (event, arg) => {
  console.log(arg)  // prints "ping"
  // It will block synchronously if `event.returnValue` not be assigned
  // Sending a synchronous message will block the whole renderer process until the reply is received, so use this method only as a last resort. It's much better to use the asynchronous version, invoke().
  event.returnValue = 'pong'
})

// async
ipcMain.on('asynchronous-message', (event, arg) => {
  console.log(arg)  // prints "ping"
  event.sender.send('asynchronous-reply', 'pong')
})
```

```javascript
// In renderer process
const {ipcRenderer} = require('electron')


console.log(
  ipcRenderer.sendSync('synchronous-message', 'ping')
) // prints "pong"


ipcRenderer.send('asynchronous-message', 'ping')

// async
ipcRenderer.on('asynchronous-reply', (event, arg) => {
  console.log(arg) // prints "pong"
})
```

**window/renderer listen main**
**main to renderer to window**
```javascript
// main.js

/**
 * @param {string} eventName
 * @param {...any[]} args
 */
function emitEvent(eventName, ...ars) {
  mainWindow.webContents.send("emit-event", eventName, ...args);
}


// preload.js
const { contexrBridge, ipcRenderer } = require("electron");
const { EventEmitter } = require("events");

const eventCenter = new EventEmitter();

contextBridge.exposeInMainWorld("obj", {
  addEventLister: (event, listener) => {
    eventCenter.on(event, listener);
  },
  // note: removeEventListener to clean memory before page unload
  removeEventListener: (event, listener) => {
    eventCenter.off(event, listener);
  },
  emitEvent: (event, ...args) => {
    eventCenter.emit(event, ...args);
  }
});

ipcRenderer.on("emit-event", (_, eventName, ...args)=>{
  eventCenter.emit(eventName, ...args);
});


// index.tsx
useEffect(()=>{
  const listener = (...args) => {
    console.log(args);
  };
  
  window.obj.addEventListener("foo", listener);

  return ()=>{
    window.obj.removeEventListener("foo", listener);
  }
}, []);

steps:
1. index.tsx addEventListener
2. ipcRenderer eventCenter addEventListener
2. main emit and ipcRender eventCenter call listener
```


**main to preload.js**
send messages from the main process to renderer process in Electron
```javascript
win.webContents.send("message", "pong");
```


# Cookie
由于网络请求返回的 cookies 只能保存在对应的域名下 ，而且在 Electron 的 file 协议下，document.cookie = cookies 无效，因此应当由 Electron 主进程负责维护 cookies。
```javascript
// main.js
const { app, session } = require("electron");

app.whenReady().then(async () => {
  // Writes cookies data to disk when cookies changed so that restore cookies when app restarts.
  session.defaultSession.cookies.on("changed", () => {
    session.defaultSession.cookies.flushStore().catch((err) => console.log(err));
  });

  ipcMain.handle("set-cookie", (event, cookie) => {
    session.defaultSession.cookies.set(cookie).catch((err) => console.log(err));
  });

  ipcMain.handle("remove-cookie", (event, url, name) => {
    session.defaultSession.cookies.remove(url, name);
  });

  ipcMain.on("get-cookies", (event, url) => {
    if (url) {
      session.defaultSession.cookies.get({ url })
        .then((cookies) => {
          event.returnValue = cookies;
        })
        .catch((err) => {
          console.log(err);
          event.returnValue = null;
        });
    }
  });  

  // patch request headers
  session.defaultSession.webRequest.onBeforeSendHeaders((details, callback) => {
      session.defaultSession.cookies.get({ url: env["BASE_URL"] })
          .then((cookies) => {
              if (cookies.length) {
                  details.requestHeaders["Cookie"] = cookies.map(
                      cookie => `${cookie.name}=${encodeURIComponent(cookie.value)}`
                  ).join("; ");
              }
          }).finally(() => {
              callback({ requestHeaders: details.requestHeaders });
          });
  });

  // save cookies when `set-cookie` in response headers
  session.defaultSession.webRequest.onHeadersReceived((details, callback) => {
      /** @type {Record<string, string[]>} */
      const headers = Object.keys(details.responseHeaders).reduce((headers, field) => {
          headers[field.toLowerCase()] = details.responseHeaders[field];
          return headers;
      }, {});

      headers["set-cookie"]?.forEach(str => {
          const cookie = cookie.parse(str);
          session.defaultSession.cookies.set({
              url: env["BASE_URL"],
              ...cookie,
          });
      });

      callback({ responseHeaders: details.responseHeaders });
  });
})
```

```javascript
// preload.js
const { contextBridge, ipcRenderer } = require("electron");

contextBridge.exposeInMainWorld("$electronContext", {
    /**
     * @param {Electron.CookiesSetDetails} cookie 
     * note that the expiration date of the cookie as the number of seconds since the UNIX epoch. 
     * If omitted then the cookie becomes a session cookie and will not be retained between sessions.
     * @returns {void}
     */
    setCookie: (cookie) => ipcRenderer.invoke("set-cookie", cookie),
    /**
     * @param {string} url 
     * @returns {Electron.Cookie[] | null}
     */
    getCookies: (url) => ipcRenderer.sendSync("get-cookies", url),
    /**
     * @param {string} url 
     * @param {string} name 
     * @returns {void}
     */
    removeCookie: (url, name) => ipcRenderer.invoke("remove-cookie", url, name),
});
```

# interceptFileProtocol
静态文件和前端路由跳转处理
```javascript
const { app, protocol, net } = require("electron");
const path = require("path");
const fsExtra = require("fs-extra");

const isWin = process.platform === "win32";
const tempPath = path.join(app.getPath("temp"), "yourAppName")

app.whenReady().then(()=>{
  protocol.interceptFileProtocol("file", (request, callback)=>{
    // request.url 返回的路径的分隔符在 mac、linux、windows 中都是正斜杠`/`
    // remove scheme "file://" (mac or linux) or "file:///D:" (win)
    
    // In mac, for example, request.url will may be
    // (index.html <script src="/js/index.js"></script> will request below) 
    // static file: file:///js/index.js  
    
    // (call history.pushState or location.pathname = xxx will go to url below)
    // frontend route: file:///route1/route2 (mac or linux)
    // file:///D:/route1/route2 (windows)
    let url = request.url.substring(isWin ? 10 : 7);
    // after substring
    // static file: /js/index.js
    // frontend route: /route1/route2

    // appPath 路径分隔符在 windows 下是反斜杠`\`
    // 因此对于 windows 的路径需要处理盘符(D:)和反斜杠
    // windows "D:\user\app" => "/user/app"
    const _appPath = isWin
      ? appPath.replace(/\\/g, "/").slice(2)
      : appPath;

    // appPath: /user/app
    // /user/app/js/index.js => /js/index.js
    if (url.startsWidth(_appPath)) {
      url = url.slice(_appPath.length);
    }

    // 假设 app 的静态资源(js/index.js)都是放在 appPath 目录下的 build 目录
    // /user/app/build/js/index.js
    let filename = path.join(appPath, "build", url)；
    /** @see http://nodejs.cn/api/path/path_normalize_path.html */
    // new URL("file:///D:\\user\\app\\js\\index.js").pathname = "/D:/user/app/js/index.js"
    filename = path.normalize(new URL("file://" + filename).pathname);

  	if (isWin) {
      // remove leading backslash (\)
      filename = filename.slice(1);
    }

    // 这里参考了服务器 serve static files 的行为
    // 当访问 /docs/ 时会默认访问 /docs/index.html
    if (filename.endsWith(path.sep)) {
      filename += "index.html";
    }

  	const ext = path.extname(filename);

    if (ext && fs.existsSync(filename)) {
      callback({ path: filename })
    } else if (env["BASE_URL"] && ext && ext !==".html") {
      // env["BASE_URL"] 是请求的基本路径
      // 这里根据自己的需求是否过滤掉 html
      // 该拦截器只能返回本地文件，不能返回通过请求返回的文件，但可以将请求的文件保存在本地再返回
      // callback({ url: env["BASE_URL"] + url });
      // setting the `url` option won't work, so we download the file into a temp 
      // local location, and respond it with `path` option.
      ;(async ()=>{
        filename = tempPath + url;
        url = env["BASE_URL"] + url;
        const metadataFile = filename + ".metadata";
        const dir = path.dirname(filename);
        const headers = {};

        await fsExtra.ensureDir(dir);

        if (fs.existsSync(filename) && fs.existsSync(metadataFile)) {
          // 实现协商缓存
          const metadata = await fsExtra.readJSON(metadataFile);
          headers["If-Mofided-Since"] = metadata["Last-Modified"];
          headers["If-None-Match"] = metadata["Etag"];
        }

        /** @type {import("http").IncomingMessage} */
        const res = await new Promise((resolve, reject) => {
          const req = net.request(url);

          for (const field of Object.keys(headers)) {
            req.setHeader(field, headers[fied]);
          }

          req.on("response", (res) => {
            resolve(res);
          }).on("error", (err) => {
            reject(err);
          }).end();
        });

        await new Promise((resolve, reject) => {
          if (res.statusCode === 200) {
            // IncomingMessage implements the Readable Stream interface and is therefore an EventEmitter.
            res.pipe(fs.createWriteStream(filename));
          } else {
            res.on("data", () => null);
          }

          res.once("close", () => {
            resolve();
          }).once("error", (err) => {
            reject(err);
          });
        });

        if (res.statusCode === 200) {
          /** 
           * @see https://www.electronjs.org/docs/latest/api/incoming-message#responseheaders
           * All res.header names are lowercased
           */
          await fsExtra.writeJSON(metadataFile, {
            "Last-Modifed": res.headers["last-modified"],
            "Etag": res.headers["etag"]
          });
        }

        callback({ path: filename });
      })().catch(err => {
        console.error(err);
      });
    } else {
      // 调用原请求
      callback({ url: request.url });
    }
  })
})
```

# windows 路径问题
## 前端路由跳转
例如 history.pushState 或 location.pathname = foo，location.pathname 会包含盘符，但路径分隔符在三个系统平台均为为正斜杠 /，
```javascript
location.pathname: file:///D:/foo
```
解决方案

- 采用 hash 路由模式，路由跳转只改变hash值，location.pathname 不变
- 路径比较的时候过滤盘符（/D:）
```javascript
function getURI(search: boolean | string = false, hash:boolean | string = false) {
  // 盘符有可能是小写
  let url = location.pathname.replace(/^\/[A-Z]:/i, "");

  if (typeof search === "string") {
    url += search;
  } else if (search) {
    url += location.search;
  }

  if (typeof hash === "string") {
    url += hash;
  } else if (hash) {
    url += location.hash;
  }

  return url;
}  
```

# interceptFileProtocol
请阅读 interceptFileProtocol 章节

# 通过应用注册的协议跳转到对应的路由
应用协议在 liunx 中好像只能小写。
封装一个 openUrl 链接 main 和 renderer，通过 url query 传参，例如 ?redirect=


# Reload
重新调用 win.loadUrl 或 win.loadFile

# 背景 filter 效果跟随背后的颜色
```typescript
new BrowserWindow({
  visualEffectState: "followWindow",
  vibrancy: "under-window",
});
```


# 进程管理
[https://www.electronjs.org/docs/latest/tutorial/process-model](https://www.electronjs.org/docs/latest/tutorial/process-model)
When a BrowserWindow instance is destroyed, its corresponding renderer process gets terminated as well.(preload.js)

当 contextIsolation 为 false 时，preload.js 可以访问 window，但这个 window 不同于页面的 window

- preload.js 的 window 不包含（即不可访问）页面 window 所添加的属性
- preload.js 中 contextBridge.exposeInMainWorld 定义的对象会注入到页面 window

**win.webContents.send**
main to renderer: 可以通过参数传递函数、对象等，部分类型不可传递
> Send an asynchronous message to the renderer process via channel, along with arguments. Arguments will be serialized with the [Structured Clone Algorithm](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm), just like [postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage), so prototype chains will not be included. Sending Functions, Promises, Symbols, WeakMaps, or WeakSets will throw an exception.




**ipcRenderer.send**
window to renderer to main: 可以通过参数传递函数、对象等，部分类型不可传递
> **NOTE:** Sending non-standard JavaScript types such as DOM objects or special Electron objects will throw an exception.
> Since the main process does not have support for DOM objects such as ImageBitmap, File, DOMMatrix and so on, such objects cannot be sent over Electron's IPC to the main process, as the main process would have no way to decode them. Attempting to send such objects over IPC will result in an error.


# 第三方库
常用库，一定要在对应平台下打包，同一个包在不同环境下会有区别
密码弹窗 keytar，touchID 不同环境下

- electron-log
- electron-store
- electron-updater
- sudo-prompt
- keytar
- macos-touchid

# Code Signing
https://www.electron.build/code-signing
**mac：**
[https://kilianvalkhof.com/2019/electron/notarizing-your-electron-application/](https://kilianvalkhof.com/2019/electron/notarizing-your-electron-application/)
[https://github.com/electron/osx-sign/tree/main/entitlements](https://github.com/electron/osx-sign/tree/main/entitlements)

**windows：**
[Globalsign 标准型(EV型)代码签名证书（硬件Token证书）新使用指南](https://www.trustauth.cn/code-guide/37988.html)
[https://oldj.net/article/2022/07/15/code-signing-with-electron-on-windows/#2-%E4%BD%BF%E7%94%A8%E8%AF%81%E4%B9%A6%E8%BF%9B%E8%A1%8C%E7%AD%BE%E5%90%8D](https://oldj.net/article/2022/07/15/code-signing-with-electron-on-windows/#2-%E4%BD%BF%E7%94%A8%E8%AF%81%E4%B9%A6%E8%BF%9B%E8%A1%8C%E7%AD%BE%E5%90%8D)

**linux:**
无需签名
# Electron builder Auto-updatable
[https://www.electron.build/auto-update](https://www.electron.build/auto-update)

- macOS: DMG.
> zip target for macOS is **required** for Squirrel.Mac, otherwise latest-mac.yml cannot be created, which causes autoUpdater error. Default [target](https://www.electron.build/configuration/mac#MacOptions-target) for macOS is dmg+zip, so there is no need to explicitly specify target.

- Linux: AppImage.
> 请确保你的系统中安装了 AppImageLauncher，如若没有，请使用下面的命令进行安装：
> sudo add-apt-repository ppa:appimagelauncher-team/stable
> sudo apt update
> sudo apt install appimagelauncher

- Windows: NSIS / nsisWEB.
> 采用 nsisWEB 的形式，可以自动让安装程序选择下载对应架构的安装包

**electron-builder.config.js**
```typescript
{
  publish: {
    provider: "generic",
    // 存放安装包/更新包的地址
    url: process.env["PUBLISH_URL"], 
  }
}
```

**main.js**
```typescript
const { autoUpdate } = require("electron-updater");
const electronLog = require("electron-log");
const { app, ipcMain } = require("electron");

if(!isDev) {
  autoUpdater.logger = electronLog;
}

app.whenReady().then(() => {
  ipcMain.on("合适事件", ()=>{
    autoUpdater.checkForUpdates();
  })
})

// autoUpdater.quitAndInstall() 适用于菜单按钮："退出并更新"

// autoUpdater.on('checking-for-update', () => {
// })
// autoUpdater.on('update-available', (info) => {
// })
// autoUpdater.on('update-not-available', (info) => {
// })
// autoUpdater.on('error', (err) => {
// })
// autoUpdater.on('download-progress', (progressObj) => {
// })
// autoUpdater.on('update-downloaded', (info) => {
//   询问是否立即更新
//   autoUpdater.quitAndInstall();  
// })
```

## ubuntu .deb 自动更新需要自己实现
**app.postbuild.js 打包后实现对应的 latest-linux-deb.yml**
```typescript
const { createHash } = require("crypto");
const fs = require("fs-extra");
const path = require("path");
const { dump } = require("js-yaml");
const builderConfig = require("./electron-builder.config");
const package = require("./package.json");
const arch = process.arch === "x64" ? "amd64" : process.arch;

function hashFile(file, algorithem = "sha512", encoding = "base64", options) {
  return new Promise((resolve, reject) => {
    const hash = createHash(algorithem);
    hash.on("error", reject).setEncoding(encoding);

    /* highWaterMark: 1024 * 1024 better to use more memory but hash faster */
    fs.createReadStream(file, {...options, highWaterMark: 1024 * 1024})
      .on("error", reject)
      .on("end", ()=>{
        hash.end();
        resolve(hash.read());
      })
      .pipe(hash);
  })
}

async function generateLinuxYml() {
  if(process.platform !== "linux") return;
    
  const distPath = path.join(__dirname, builderConfig.directories.output);
  const files = fs.readdirSync(distPath);
  const targets = files.filter(file => file.includes(package.version) && file.endsWith(".deb"));
  const defaultTarget = targets.find(file => file.includes(arch)) || targets[0];

  try {
    const defaultTargetSha512 = await hashFile(path.join(distPath, defaultTarget));
    const updateInfo = {
      version: package.version,
      files: [],
      path: defaultTarget,
      sha512: defaultTargetSha512,
      releaseDate: new Date().toISOString(),
    }

    for (const target of targets) {
      const filePath = path.join(distPath, target);
      const meta = fs.statSync(filePath);
      const info = {};

      info.url = target;
      info.size = meta.size;
      
      if(target === defaultTarget) {
        info.sha512 = defaultTargetSha512;
      } else {
        const sha512 = await hashFile(filePath);
        info.sha512 = sha512;
      }

      updateInfo.files.push(info);
    }

    fs.outputFileSync(
      path.join(distPath, "latest-linux-deb.yml"),
      dump(updateInfo, { lineWidth: 8000 })
    )
  } catch(err){
      console.error("Failed to generate latest-linux-deb.yml:\n", err);
  }
}

generateLinuxYml();
```
**main.js**
```typescript
const { app, session, net, dialog } = require("electron");
const child_process = require("child_process");
const sudo = require("sudo_prompt");

const protocalName = app.name.toLowerCase();
const appId = `com.foo.${protocalName}`;
const tempPath = path.join(app.getPath("temp"), appId);
/** @type {{version: string; sha512: string;}} */
const debUpdateInfo = {};
/** @type {string} */
let debUpdatesDownloadUrl;
/** @type {string} */
let debUpdatesSavePath;

// check for updates for deb target
async function checkForDebUpdates() {
  const releaseUrl = process.env["PUBLISH_URL"];
  
  if(!releaseUrl) return;

  try {
    /* @type {string} */
    const yml = await new Promise((resolve, reject) => {
      const request = net.request(releaseUrl + "/latest-linux-deb.yml");

      /* @type {Buffer[]} */
      const chunks = [];

      request.on("response", (response) => {
        response.on("data", (chunk) => {
          chunks.push(chunk);
        });
        response.on("end", () => {
          resolve(Buffer.concat(chunks).toString());
        })
      }).once("error", err => {
        reject(err);
      });

      request.end();
    })

    const currentVersion = app.getVersion();
    const latestVersion = yml.match(/version:\s(.+?)\n/)?.[1];
    const sha512 = yml.match(/sha512:\s(.+?)\n/)?.[1];

    if(latestVersion && latestVersion !== currentVersion) {
      debUpdateInfo.version = latestVersion;
      debUpdateInfo.sha512 = sha512;

      // only support amd64.deb at present
      const fileName = yml.match(/url:\s(.+?amd64\.deb)/)?.[1];

      if(fileName) {
        debUpdatesDownloadUrl = `${releaseUrl}/${fileName}`;
        // do something
      } else {
        // do something
      }
    }
  } catch (err) {
    console.error(err);
  }
}

app.whenReady().then(() => {
  session.defaultSession.on("will-download", (event, item, webContents) => {
    let savePath; 
    const isDebUpdate = item.getURL === debUpdatesDownloadUrl;

    if(isDebUpdate) {
      savePath = path.join(tempPath, path.basename(debUpdatesDownloadUrl));
      item.setSavePath(savePath);
    }

    item.once("done", async (event, state) => {
      if(state === "completed") {
        if(isDebUpdate) {
          const sha512 = await hashFile(savePath);

          if(sha512 === debUpdateInfo.sha512) {
            debUpdatesSavePath = savePath;
            // do something
          } else {
            // 下载更新包失败
          }
        }
      }
    })
  })

// 关闭应用前进行更新
app.on("before-quier", () => {
  if(debUpdatesSavePath) {
    const hasCommand = (command) => {
      try {
        return !!child_process.execSync(`which ${command}`).toString();
      } catch {
        return false;
      }
    }

    if(hasCommand("gnome-terminal")) {
      let cmd = `gnome-terminal -x bash -c "echo Updating ${app.name}...; `;

      if(hasCommand("pkexec")) {
        cmd += `pkexec --disable-internal-agent`;
      } else if(hasCommand("kdesudo")) {
        cmd += `kdesudo --comment '${app.name} wants to make changes. Enter your password to allow this.' -d -- `;
      }

      // 不清楚会不会自动删除安装包，不会则用 fs 删除即可
      cmd += `apt install ${debUpdatesSavePath}; exec ${protocalName} & > /dev/null; exec bash"`;

      try {
        child_process.exexSync(cmd);
      } catch (err) {
        dialog.showMessageBoxSync({
          type: "error",
          message: err.message || String(err)
        })
      }
    }
  }

  app.exit(0);
}).on("quit", ()=>{
  
})
```

# Electron builder 配置
**electron-builder.config.js**
```typescript
const fsExtra = require("fs-extra");
const path = require("path");
const dotenv = require("dotenv");

dotenv.config({
  path: path.join(__dirname, ".env");
});

const isWind = process.platform === "win32";
const shouldSign = process.env["BUILD_SIGN"] === "true";
const buildAll = process.env["BUILD_ALL"] === "true";
const artifactName = "${name}-${version}-${os}-${arch}.${ext}";
// https://www.electron.build/configuration/mac.html
const category = "Office";
const arch = shouldSign && buildAll ? [
  "x64",
  "arm64",
] : [process.arch];
const winCertFile = "./certs/foo.pfx";
const hasWinCertFile = fsExtra.existsSync(winCertFile);

/** @type {import("electron-builder").Configuration} */
module.exports = {
  productName: "Hello",
  appId: "com.foo.bar",
  directories: {
    // 先把前端打包到 app/build
    buildResources: "app/build",
    output: "out",
  },
  // 相对于 app 目录, https://www.electron.build/configuration/contents#files
  files: [
    "build",
    "main.js",
    "package.json",
    "node_modules",
    "!**/node_modules/*/{CHANGELOG.md,README.md,README,readme.md,readme}",
    "!**/node_modules/*/{test,__tests__,tests,powered-test,example,examples}",
    "!**/node_modules/*.d.ts",
    "!**/node_modules/.bin",
    "!**/*.{iml,o,hprof,orig,pyc,pyo,rbc,swp,csproj,sln,xproj}",
    "!.editorconfig",
    "!**/._*",
    "!**/{.DS_Store,.git,.hg,.svn,CVS,RCS,SCCS,.gitignore,.gitattributes}",
    "!**/{__pycache__,thumbs.db,.flowconfig,.idea,.vs,.nyc_output}",
    "!**/{appveyor.yml,.travis.yml,circle.yml}",
    "!**/{npm-debug.log,yarn.lock,.yarn-integrity,.yarn-metadata.json}",
  ],
  /**
   * please ensure that nsis-web installer(.exe) and its packages are in the same directory
   * was configured by env["PUBLISH_URL"], otherwise it will install fail and prompt `windows` 正在寻找 foo.exe`.
   */
  publish: {
    provider: "generic",
    url: process.env["PUBLISH_URL"],
  },
  mac: {
    artifactName,
    category,
    target: shouldSign ? [
      { target: "dmg", arch },
      { target: "zip", arch },
    ] : { target: "dmg", arch },
    identify: shouldSign ? void 0 : null,
    hardenedRuntime: true,
    gateKeeperAssess: false,
    entitlements: "entitlements.mac.plist",
    entitlementsInherit: "entitlements.mac.plist",
  },
  dmg: {
    sign: false,
  },
  afterSign: shouldSign ? "notarize.js" : void 0,
  win: {
    artifactName: "${name}-${version}-${os}.${ext}",
    target: {
      target: "nsis-web",
      arch,
    },
    asar: true,
    icon: "public/icon.png",
    certificateFile: !isWin && shouldSign && hasWinCertFile ? winCertFile : void 0,
    // 以下命令来自: https://github.com/electron-userland/electron-builder/blob/5668dc204b83ae0c1edf79a4998f41292007d230/packages/app-builder-lib/src/codeSign/windowsCodeSign.ts
    // powershell 运行以下命令查看 subjectName: Get-ChildItem -Recurse Cert: -CodeSigningCert | Select-Object -Property Subject,PSParentPath,Thumbprint | ConvertTo-Json -Compress
    certificateSubjectName: process.env["WINDOWS_CERTFICATE_SUBJECT_NAME"],
    certificatePassword: process.env["WINDOWS_CERTFICATE_PASSWORD"],
  },
  nsisWeb: {
    oneClick: false,
    perMachine: false,
    allowToChangeInstallationDirectory: true,
    deleteAppDataOnUninstall: true,
    createDesktopShortcut: "always",
  },
  linux: {
    artifactName,
    category,
    target: buildAll ? [
      { target: "AppImage", arch },
      { target: "deb", arch },
    ]: { target: "deb", arch },
    desktop: {
      "MimeType": "x-scheme-handler/foo;" // foo - 应用协议
    },
    asar: true,
    asarUnpack: ["build/icon.png"],
  }
}
```
# Linux 打包配置
## app.setAsDefaultProtocolClient doesn’t work on Linux .AppImage
[https://github.com/electron-userland/electron-builder/issues/4035](https://github.com/electron-userland/electron-builder/issues/4035)

**1.**
You do need to add "x-scheme-handler/MyApp;" as a known mimetype in your .desktop file.
You don't need to specify the full path of your .desktop file, as ~/.local/share/applications/ is one of the default paths for them.
Desktop file are generated by electron-builder, and managed automaticaly by the OS.
You can customize it through your package.json:
```javascript
"linux": {
  "desktop": {
    "MimeType": "x-scheme-handler/MyApp;" 
  }
}
```
The electron-builder doc might not be extremely explicit: [https://www.electron.build/configuration/appimage](https://www.electron.build/configuration/appimage)
BTW, don't put a space in your appimage name ! There is an escaping probleme somewhere...
see: [#3547](https://github.com/electron-userland/electron-builder/issues/3547)

**2.**
Make sure you have the appimage "installed"/"Integrated" so that the .desktop file is found by your OS. (on Manjaro Linux, the already installed [https://github.com/TheAssassin/AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher) works fine for this. Double click the appimage and click on Integrate and Run)
## AppImage AppImageLauncher
[https://www.electron.build/configuration/appimage](https://www.electron.build/configuration/appimage)
Since electron-builder 21 desktop integration is not a part of produced AppImage file. [AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher) is the recommended way to integrate AppImages.

AppImageLauncher 可以自动将 AppImage 程序快捷方式添加到桌面环境的程序启动器/菜单（包括程序图标和合适的说明）中。此外，它还允许您管理，更新和删除它们。

**AppImageLauncher 安装**
```javascript
sudo add-apt-repository ppa:appimagelauncher-team/stable

sudo apt update

sudp apt install appimagelauncher
```

# asar 问题
[https://www.electronjs.org/docs/latest/tutorial/asar-archives](https://www.electronjs.org/docs/latest/tutorial/asar-archives)
Most fs APIs can read a file or get a file's information from ASAR archives **without unpacking**
在 app 引用的静态资源，需要通过 asarUnpack 中进行配置，如果只是读取文件则不需要
```javascript
linux: {
    desktop: {
        "MimeType": "x-scheme-handler/MyApp;",
    },
    asar: true,
    // A glob patterns relative to the app directory(Two package.json ), which specifies which files to unpack when creating the asar archive.
    asarUnpack: ["build/icon.png"],
},
```

# Menu
```typescript
/**
 * `dictation` and `emoji` enabled by default
 * These can be removed using the `systemPreferences` API
 * @see https://github.com/electron/electron/issues/8283
 */
```

# 从 VSCode 源码中我学到了什么？
[https://mp.weixin.qq.com/s/nMjPJ7VbvxQLBvG41krNTQ](https://mp.weixin.qq.com/s/nMjPJ7VbvxQLBvG41krNTQ)
