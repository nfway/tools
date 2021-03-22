# Aria2 （https,and AriaNG) +Rclone + Google Drive + Goindex + File browser(https)

- [0. Google Drive API](#0-google-drive-api)
  * [Enable API](#enable-api)
- [1.Install Rclone](#1install-rclone)
  * [Prepare](#prepare)
  * [Install Rclone](#install-rclone)
  * [The process](#the-process)
  * [Mount](#mount)
  * [Mount when start](#mount-when-start)
- [Install Aria2](#install-aria2)
  * [Prepare](#prepare-1)
  * [Installation](#installation)
  * [Upload after download complete](#upload-after-download-complete)
- [https for Aria2(acme.sh)](#https-for-aria2-acmesh-)
- [File Browser](#file-browser)
  * [Installation](#installation-1)
  * [Start File Browser when reboot](#start-file-browser-when-reboot)
  * [Setting](#setting)
- [Add domain to filebrowser](#add-domain-to-filebrowser)
- [Goindex with Worker space](#goindex-with-worker-space)
- [AriaNG](#ariang)
- [VPS auto restart](#vps-auto-restart)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


环境 Centos 8 64x

## 0. Google Drive API

Refer: https://bynss.com/howto/35095.html

### Enable API

访问：https://console.developers.google.com/projectcreate

- Create Project
- Go to APIs overview
- Enable API and Service
- Step to Library,search, "Google Drive API", enable, then manage
- step to credentials, create credentials, Create OAuth client ID
- OAuth consent screen
- choose external ,  Available to any user with a Google Account. 
- fill the blanks
- step  credentials, create  OAuth client ID, type : desktop app
- done, you get client id and secret , like:

```
841357527774-abcdyefgrqknr42ca6c7fsvg.apps.googleusercontent.com
MKaXqPk7Qevs978679iixPUBDRe
```



## 1.Install Rclone

refer: https://www.yunloc.com/562.html

### Prepare

```
 yum -y install epel-release wget unzip screen fuse fuse-devel
```

### Install Rclone

```
curl https://rclone.org/install.sh | sudo bash
```

### The process

```
rclone config
```

```
    No remotes found - make a new one
    n) New remote
    s) Set configuration password
    q) Quit config
    n/s/q> n
    name> remote
```

根据提示选择 Google Drive

```
    Type of storage to configure.
    Enter a string value. Press Enter for the default ("").
    Choose a number from below, or type in your own value
     1 / A stackable unification remote, which can appear to merge the contents of several remotes
       \ "union"
     2 / Alias for a existing remote
       \ "alias"
     3 / Amazon Drive
       \ "amazon cloud drive"
     4 / Amazon S3 Compliant Storage Provider (AWS, Alibaba, Ceph, Digital Ocean, Dreamhost, IBM COS, Minio, etc)
       \ "s3"
     5 / Backblaze B2
       \ "b2"
     6 / Box
       \ "box"
     7 / Cache a remote
       \ "cache"
     8 / Dropbox
       \ "dropbox"
     9 / Encrypt/Decrypt a remote
       \ "crypt"
    10 / FTP Connection
       \ "ftp"
    11 / Google Cloud Storage (this is not Google Drive)
       \ "google cloud storage"
    12 / Google Drive
       \ "drive"
    13 / Hubic
       \ "hubic"
    14 / JottaCloud
       \ "jottacloud"
    15 / Local Disk
       \ "local"
    16 / Mega
       \ "mega"
    17 / Microsoft Azure Blob Storage
       \ "azureblob"
    18 / Microsoft OneDrive
       \ "onedrive"
    19 / OpenDrive
       \ "opendrive"
    20 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
       \ "swift"
    21 / Pcloud
       \ "pcloud"
    22 / QingCloud Object Storage
       \ "qingstor"
    23 / SSH/SFTP Connection
       \ "sftp"
    24 / Webdav
       \ "webdav"
    25 / Yandex Disk
       \ "yandex"
    26 / http Connection
       \ "http"
    Storage> 12
```

输入 Google API 的相关资料

```
    Google Application Client Id
    Leave blank normally.
    Enter a string value. Press Enter for the default ("").
    client_id> 
    Google Application Client Secret
    Leave blank normally.
    Enter a string value. Press Enter for the default ("").
    client_secret> 
```

继续

```
    Scope that rclone should use when requesting access from drive.
    Enter a string value. Press Enter for the default ("").
    Choose a number from below, or type in your own value
     1 / Full access all files, excluding Application Data Folder.
       \ "drive"
     2 / Read-only access to file metadata and file contents.
       \ "drive.readonly"
       / Access to files created by rclone only.
     3 | These are visible in the drive website.
       | File authorization is revoked when the user deauthorizes the app.
       \ "drive.file"
       / Allows read and write access to the Application Data folder.
     4 | This is not visible in the drive website.
       \ "drive.appfolder"
       / Allows read-only access to file metadata but
     5 | does not allow any access to read or download file content.
       \ "drive.metadata.readonly"
    scope> 1
```

填入团队盘的文件夹ID： https://drive.google.com/drive/folders/0AMPQGbi_lh_mUk68788PVA

```
    ID of the root folder
    Leave blank normally.
    Fill in to access "Computers" folders. (see docs).
    Enter a string value. Press Enter for the default ("").
    root_folder_id> 
    Service Account Credentials JSON file path 
    Leave blank normally.
    Needed only if you want use SA instead of interactive login.
    Enter a string value. Press Enter for the default ("").
    service_account_file> 0AMPQGbi_lh_mUk68788PVA
```

and no

```
    Edit advanced config? (y/n)
    y) Yes
    n) No
    y/n> n
    
 	Remote config
    Use auto config?
     * Say Y if not sure
     * Say N if you are working on a remote or headless machine
    y) Yes
    n) No
    y/n> n
```

继续

```
    If your browser doesn't open automatically go to the following link: https://accounts.google.com/o/oauth2/auth?access_type=offline&client_id=202264815644.apps.googleusercontent.com&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob&response_type=code&scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fdrive&state=55663e7e07382e3dddb9025c86de4f
    Log in and authorize rclone for access
    Enter verification code> 4/UQGiRz375eb-OixO5EUtZMxBhJwAQ4zOyvA1wtJWKq2Ocmzh3zNYE
```

```
    Configure this as a team drive?
    y) Yes
    n) No
    y/n> y
```

继续按照操作即可。

```
    --------------------
    [remote]
    type = drive
    scope = drive
    token = {"access_token":"ya29.GlsQByNiBURlXoPpe-bDpa2kF99Jo4rrmjicBXdWIT6loPUhS7SJ9XWUIk2LP4vO231nra_zpUwHn6no0Y_LBbXYFZvyVf0gRthepF2VuPFdhBFEKY7XYJaelt","token_type":"Bearer","refresh_token":"1/ry1JGhRiqqE6-PqRN-S2icZ_Oz9uOTXfSNxWA85zUnjU5gEm-6TejL6o-hjyuY","expiry":"2019-05-21T04:36:23.300542043-04:00"}
    --------------------
    y) Yes this is OK
    e) Edit this remote
    d) Delete this remote
    y/e/d> y
```

### Mount

- mkdir -p  /root/gdrive
- rclone mount remote:video /root/gdrive --allow-other --allow-non-empty --vfs-cache-mode writes
- 解释：remote是 安装 rclone 时的 name， video 是 google drive 里的文件夹,  /root/gdrive 是本地文件夹
- 挂载只要几秒钟，但终端不会返回成功信息，关闭 SSH 重连即可。
- df -h 查看结果

### Mount when start

先安装 nano 

```
yum install -y nano
```

**方法一：失效**

refer：https://www.moerats.com/archives/481/

```
command="mount remote:video /root/gdrive --allow-other --allow-non-empty --vfs-cache-mode writes"

cat > /etc/systemd/system/rclone.service <<EOF
[Unit]
Description=Rclone
After=network-online.target

[Service]
Type=simple
ExecStart=$(command -v rclone) ${command}
Restart=on-abort
User=root
```

方法二：

```
nano /etc/systemd/system/rclone.service
```

输入：

```
[Unit]
Description=rclone
    
[Service]
User=root
ExecStart=rclone mount remote:video /root/gdrive --allow-other --allow-non-empty --vfs-cache-mode writes
Restart=on-abort
    
[Install]
WantedBy=multi-user.target
```

开机启动

```
systemctl start rclone
```

设置开机自启

```
systemctl enable rclone
```

其他命令

```
重启：systemctl restart rclone
停止：systemctl stop rclone
状态：systemctl status rclone
```



## Install Aria2

refer https://p3terx.com/archives/offline-download-of-onedrive-gdrive.html

### Prepare 

```
yum -y install wget curl ca-certificates
```

### Installation

```
wget -N git.io/aria2.sh && chmod +x aria2.sh && ./aria2.sh
```

### Upload after download complete

```
nano /root/.aria2c/aria2.conf
```

找到“下载完成后执行的命令”，把`clean.sh`替换为`upload.sh`。

`nano /root/.aria2c/script.conf`

打开附加功能脚本配置文件进行修改，有中文注释，按照自己的实际情况进行修改

```none
# 网盘名称(RCLONE 配置时填写的 name)
drive-name=remote
```

同时需要修改远程网盘的目录。

- 重启 Aria2 。脚本选项重启或者执行以下命令：

```none
service aria2 restart
```

执行 `upload.sh` 脚本，提示 `success` 即配置成功。

```none
/root/.aria2c/upload.sh
```

## https for Aria2(acme.sh)

https://github.com/acmesh-official/acme.sh/wiki/%E8%AF%B4%E6%98%8E

```
curl  https://get.acme.sh | sh
```

生成证书

- 停止 nginx (使用80端口)

- ```
  acme.sh  --issue -d mydomain.com   --standalone
  ```

- ```
  nano /root/.aria2c/aria2.conf
  ```

```
#是否启用RPC服务的SSL/TLS加密
rpc-secure=true
#申请的域名crt证书文件路径，自行修改
rpc-certificate=//etc/letsencrypt/live/xxxx.xxx/fullchain.pem
##申请的域名key证书文件路径，自行修改
rpc-private-key=/etc/letsencrypt/live/xxxx.xxx/privkey.pem
```

## File Browser

refer1:https://blog.csdn.net/Homewm/article/details/87931165

refer2:https://blog.csdn.net/ywd1992/article/details/93030495

### Installation

```
yum -y  install curl
curl -fsSL https://filebrowser.xyz/get.sh | bash
```

创建配置数据库：

```
filebrowser -d /etc/filemanager/filebrowser.db config init
```

监听地址和端口，设置语言

```
filebrowser -d /etc/filemanager/filebrowser.db config set --address 0.0.0.0 
filebrowser -d /etc/filemanager/filebrowser.db config set --port 6080
filebrowser -d /etc/filemanager/filebrowser.db config set --locale zh-cn 
```

设置日志、用户

```
filebrowser -d /etc/filemanager/filebrowser.db config set --log /var/log/filebrowser.log
filebrowser -d /etc/filemanager/filebrowser.db users add admin password123 --perm.admin
```

admin 为用户，password123 为密码

### Start File Browser when reboot

编写 service

```
nano /etc/systemd/system/filebrowser.service
```

输入

```
[Unit]
Description=File Browser
After=network.target

[Service]
ExecStart=filebrowser -d /etc/filemanager/filebrowser.db

[Install]
WantedBy=multi-user.target
```

开机启动及自启动

```
systemctl daemon-reload
systemctl start filebrowser
systemctl enable filebrowser
```

### Setting

点击【设置】→【全局设置】→【用户默认设置】→ 将目录范围改为你想要显示的文件夹，如` ./root/downloads` →点击保存

## Add domain to filebrowser

- cd nginx folder
- stop nginx service

- acme.sh -d  yourdomain.com  --standalone




     server {
     listen       443 ssl http2;
        listen       [::]:443 ssl http2;
        server_name  yourdomain.com; #使用的域名
    
        ssl_certificate "/root/.acme.sh/yourdomain.com/yourdomain.com.cer"; # ssl public
        ssl_certificate_key " /root/.acme.sh/yourdomain.com/yourdomain.com.key"; # ssl privkey
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
    
        location / {
                proxy_pass http://localhost:6080;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    
    }
## Goindex with Worker space

快速：https://github.com/Aicirou/goindex-theme-acrou [中文文档](https://github.com/Aicirou/goindex-theme-acrou/blob/master/README_zh.md)

1. 本地安装[rclone](https://rclone.org/downloads/)
2. 使用`rclone`获取`refresh_token`
3. 下载`index.js` (https://github.com/Aicirou/goindex-theme-acrou/tree/master/go2index) 然后替换`client_id`,`client_secret`,`refresh_token` 为你刚刚获取到的
4. 把代码部署到[Cloudflare Workers](https://www.cloudflare.com/)

一般是这样的：

```javascript
// =======Options START=======
var authConfig = {
  siteName: "Forgotten Planet", // 网站名称
  version: "1.1.1", // 程序版本
  theme: "acrou",
  // 强烈推荐使用自己的 client_id 和 client_secret
  client_id: "需要填入",
  client_secret: "需要填入",
  refresh_token: "需要填入", // 授权 token
  /**
   * 设置要显示的多个云端硬盘；按格式添加多个
   * [id]: 可以是 团队盘id、子文件夹id、或者"root"（代表个人盘根目录）；
   * [name]: 显示的名称
   * [user]: Basic Auth 的用户名
   * [pass]: Basic Auth 的密码
   * [protect_file_link]: Basic Auth 是否用于保护文件链接，默认值（不设置时）为 false，即不保护文件链接（方便 直链下载/外部播放 等）
   * 每个盘的 Basic Auth 都可以单独设置。Basic Auth 默认保护该盘下所有文件夹/子文件夹路径
   * 【注意】默认不保护文件链接，这样可以方便 直链下载/外部播放;
   *       如果要保护文件链接，需要将 protect_file_link 设置为 true，此时如果要进行外部播放等操作，需要将 host 替换为 user:pass@host 的 形式
   * 不需要 Basic Auth 的盘，保持 user 和 pass 同时为空即可。（直接不设置也可以）
   * 【注意】对于id设置为为子文件夹id的盘将不支持搜索功能（不影响其他盘）。
   */
  "roots": [
	{
		"id": "团队盘需要填入",
		"name": "",
		"user": "",
		"pass": "",
		"protect_file_link": false
	}
],
  default_gd: 0,
  /**
   * 文件列表页面每页显示的数量。【推荐设置值为 100 到 1000 之间】；
   * 如果设置大于1000，会导致请求 drive api 时出错；
   * 如果设置的值过小，会导致文件列表页面滚动条增量加载（分页加载）失效；
   * 此值的另一个作用是，如果目录内文件数大于此设置值（即需要多页展示的），将会对首次列目录结果进行缓存。
   */
  files_list_page_size: 50,
  /**
   * 搜索结果页面每页显示的数量。【推荐设置值为 50 到 1000 之间】；
   * 如果设置大于1000，会导致请求 drive api 时出错；
   * 如果设置的值过小，会导致搜索结果页面滚动条增量加载（分页加载）失效；
   * 此值的大小影响搜索操作的响应速度。
   */
  search_result_list_page_size: 50,
  // 确认有 cors 用途的可以开启
  enable_cors_file_down: false,
  /**
   * 上面的 basic auth 已经包含了盘内全局保护的功能。所以默认不再去认证 .password 文件内的密码;
   * 如果在全局认证的基础上，仍需要给某些目录单独进行 .password 文件内的密码验证的话，将此选项设置为 true;
   * 【注意】如果开启了 .password 文件密码验证，每次列目录都会额外增加查询目录内 .password 文件是否存在的开销。
   */
  enable_password_file_verify: false,
};

var themeOptions = {
  cdn: "https://cdn.jsdelivr.net/gh/Aicirou/goindex-theme-acrou",
  // 主题版本号
  version: "2.0.5",
  //可选默认系统语言:en/zh-chs/zh-cht
  languages: "zh-chs",
  render: {
    /**
     * 是否渲染HEAD.md文件
     * Render HEAD.md file
     */
    head_md: true,
    /**
     * 是否渲染README.md文件
     * Render README.md file
     */
    readme_md: true,
    /**
     * 是否渲染文件/文件夹描述
     * Render file/folder description or not
     */
    desc: true,
  },
  /**
   * 播放器选项
   * Player options
   */
  player: {
    /**
     * 播放器api（不指定则使用浏览器默认播放器）
     * Player api(Use browser default player if not specified)
     */
    api: "https://api.jsonpop.cn/demo/blplyaer/?url=",
  },
};
// =======Options END=======

/**
 * global functions
 */
const FUNCS = {
  /**
   * 转换成针对谷歌搜索词法相对安全的搜索关键词
   */
  formatSearchKeyword: function (keyword) {
    let nothing = "";
    let space = " ";
    if (!keyword) return nothing;
    return keyword
      .replace(/(!=)|['"=<>/\\:]/g, nothing)
      .replace(/[,，|(){}]/g, space)
      .trim();
  },
};

/**
 * global consts
 * @type {{folder_mime_type: string, default_file_fields: string, gd_root_type: {share_drive: number, user_drive: number, sub_folder: number}}}
 */
const CONSTS = new (class {
  default_file_fields =
    "parents,id,name,mimeType,modifiedTime,createdTime,fileExtension,size";
  gd_root_type = {
    user_drive: 0,
    share_drive: 1,
    sub_folder: 2,
  };
  folder_mime_type = "application/vnd.google-apps.folder";
})();

// gd instances
var gds = [];

function html(current_drive_order = 0, model = {}) {
  return `
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0,maximum-scale=1.0, user-scalable=no"/>
  <title>${authConfig.siteName}</title>
  <style>
    @import url(${themeOptions.cdn}@${themeOptions.version}/dist/style.min.css);
  </style>
  <script>
    window.gdconfig = JSON.parse('${JSON.stringify({
      version: authConfig.version,
      themeOptions: themeOptions,
    })}');
    window.themeOptions = JSON.parse('${JSON.stringify(themeOptions)}');
    window.gds = JSON.parse('${JSON.stringify(
      authConfig.roots.map((it) => it.name)
    )}');
    window.MODEL = JSON.parse('${JSON.stringify(model)}');
    window.current_drive_order = ${current_drive_order};
  </script>
</head>
<body>
    <div id="app"></div>
    <script src="${themeOptions.cdn}@${
    themeOptions.version
  }/dist/app.min.js"></script>
</body>
</html>
`;
}

addEventListener("fetch", (event) => {
  event.respondWith(handleRequest(event.request));
});

/**
 * Fetch and log a request
 * @param {Request} request
 */
async function handleRequest(request) {
  if (gds.length === 0) {
    for (let i = 0; i < authConfig.roots.length; i++) {
      const gd = new googleDrive(authConfig, i);
      await gd.init();
      gds.push(gd);
    }
    // 这个操作并行，提高效率
    let tasks = [];
    gds.forEach((gd) => {
      tasks.push(gd.initRootType());
    });
    for (let task of tasks) {
      await task;
    }
  }

  // 从 path 中提取 drive order
  // 并根据 drive order 获取对应的 gd instance
  let gd;
  let url = new URL(request.url);
  let path = decodeURI(url.pathname);

  /**
   * 重定向至起始页
   * @returns {Response}
   */
  function redirectToIndexPage() {
    return new Response("", {
      status: 301,
      headers: { Location: `/${authConfig.default_gd}:/` },
    });
  }

  if (path == "/") return redirectToIndexPage();
  if (path.toLowerCase() == "/favicon.ico") {
    // 后面可以找一个 favicon
    return new Response("", { status: 404 });
  }

  // 特殊命令格式
  const command_reg = /^\/(?<num>\d+):(?<command>[a-zA-Z0-9]+)(\/.*)?$/g;
  const match = command_reg.exec(path);
  let command;
  if (match) {
    const num = match.groups.num;
    const order = Number(num);
    if (order >= 0 && order < gds.length) {
      gd = gds[order];
    } else {
      return redirectToIndexPage();
    }
    // basic auth
    for (const r = gd.basicAuthResponse(request); r; ) return r;
    command = match.groups.command;

    // 搜索
    if (command === "search") {
      if (request.method === "POST") {
        // 搜索结果
        return handleSearch(request, gd);
      } else {
        const params = url.searchParams;
        // 搜索页面
        return new Response(
          html(gd.order, {
            q: params.get("q") || "",
            is_search_page: true,
            root_type: gd.root_type,
          }),
          {
            status: 200,
            headers: { "Content-Type": "text/html; charset=utf-8" },
          }
        );
      }
    } else if (command === "id2path" && request.method === "POST") {
      return handleId2Path(request, gd);
    } else if (command === "view") {
      const params = url.searchParams;
      return gd.view(params.get("url"), request.headers.get("Range"));
    } else if (command !== "down" && request.method === "GET") {
      return new Response(html(gd.order, { root_type: gd.root_type }), {
        status: 200,
        headers: { "Content-Type": "text/html; charset=utf-8" },
      });
    }
  }
  const reg = new RegExp(`^(/\\d+:)${command}/`, "g");
  path = path.replace(reg, (p1, p2) => {
    return p2 + "/";
  });
  // 期望的 path 格式
  const common_reg = /^\/\d+:\/.*$/g;
  try {
    if (!path.match(common_reg)) {
      return redirectToIndexPage();
    }
    let split = path.split("/");
    let order = Number(split[1].slice(0, -1));
    if (order >= 0 && order < gds.length) {
      gd = gds[order];
    } else {
      return redirectToIndexPage();
    }
  } catch (e) {
    return redirectToIndexPage();
  }

  // basic auth
  // for (const r = gd.basicAuthResponse(request); r;) return r;
  const basic_auth_res = gd.basicAuthResponse(request);
  path = path.replace(gd.url_path_prefix, "") || "/";
  if (request.method == "POST") {
    return basic_auth_res || apiRequest(request, gd);
  }

  let action = url.searchParams.get("a");

  if (path.substr(-1) == "/" || action != null) {
    return (
      basic_auth_res ||
      new Response(html(gd.order, { root_type: gd.root_type }), {
        status: 200,
        headers: { "Content-Type": "text/html; charset=utf-8" },
      })
    );
  } else {
    if (path.split("/").pop().toLowerCase() == ".password") {
      return basic_auth_res || new Response("", { status: 404 });
    }
    let file = await gd.file(path);
    let range = request.headers.get("Range");
    if (gd.root.protect_file_link && basic_auth_res) return basic_auth_res;
    const is_down = !(command && command == "down");
    return gd.down(file.id, range, is_down);
  }
}

async function apiRequest(request, gd) {
  let url = new URL(request.url);
  let path = url.pathname;
  path = path.replace(gd.url_path_prefix, "") || "/";

  let option = { status: 200, headers: { "Access-Control-Allow-Origin": "*" } };

  if (path.substr(-1) == "/") {
    let deferred_pass = gd.password(path);
    let body = await request.text();
    body = JSON.parse(body);
    // 这样可以提升首次列目录时的速度。缺点是，如果password验证失败，也依然会产生列目录的开销
    let deferred_list_result = gd.list(
      path,
      body.page_token,
      Number(body.page_index)
    );

    // check .password file, if `enable_password_file_verify` is true
    if (authConfig["enable_password_file_verify"]) {
      let password = await gd.password(path);
      // console.log("dir password", password);
      if (password && password.replace("\n", "") !== body.password) {
        let html = `{"error": {"code": 401,"message": "password error."}}`;
        return new Response(html, option);
      }
    }

    let list_result = await deferred_list_result;
    return new Response(JSON.stringify(list_result), option);
  } else {
    let file = await gd.file(path);
    let range = request.headers.get("Range");
    return new Response(JSON.stringify(file));
  }
}

// 处理 search
async function handleSearch(request, gd) {
  const option = {
    status: 200,
    headers: { "Access-Control-Allow-Origin": "*" },
  };
  let body = await request.text();
  body = JSON.parse(body);
  let search_result = await gd.search(
    body.q || "",
    body.page_token,
    Number(body.page_index)
  );
  return new Response(JSON.stringify(search_result), option);
}

/**
 * 处理 id2path
 * @param request 需要 id 参数
 * @param gd
 * @returns {Promise<Response>} 【注意】如果从前台接收的id代表的项目不在目标gd盘下，那么response会返回给前台一个空字符串""
 */
async function handleId2Path(request, gd) {
  const option = {
    status: 200,
    headers: { "Access-Control-Allow-Origin": "*" },
  };
  let body = await request.text();
  body = JSON.parse(body);
  let path = await gd.findPathById(body.id);
  return new Response(path || "", option);
}

class googleDrive {
  constructor(authConfig, order) {
    // 每个盘对应一个order，对应一个gd实例
    this.order = order;
    this.root = authConfig.roots[order];
    this.root.protect_file_link = this.root.protect_file_link || false;
    this.url_path_prefix = `/${order}:`;
    this.authConfig = authConfig;
    // TODO: 这些缓存的失效刷新策略，后期可以制定一下
    // path id
    this.paths = [];
    // path file
    this.files = [];
    // path pass
    this.passwords = [];
    // id <-> path
    this.id_path_cache = {};
    this.id_path_cache[this.root["id"]] = "/";
    this.paths["/"] = this.root["id"];
    /*if (this.root['pass'] != "") {
            this.passwords['/'] = this.root['pass'];
        }*/
    // this.init();
  }

  /**
   * 初次授权；然后获取 user_drive_real_root_id
   * @returns {Promise<void>}
   */
  async init() {
    await this.accessToken();
    /*await (async () => {
            // 只获取1次
            if (authConfig.user_drive_real_root_id) return;
            const root_obj = await (gds[0] || this).findItemById('root');
            if (root_obj && root_obj.id) {
                authConfig.user_drive_real_root_id = root_obj.id
            }
        })();*/
    // 等待 user_drive_real_root_id ，只获取1次
    if (authConfig.user_drive_real_root_id) return;
    const root_obj = await (gds[0] || this).findItemById("root");
    if (root_obj && root_obj.id) {
      authConfig.user_drive_real_root_id = root_obj.id;
    }
  }

  /**
   * 获取根目录类型，设置到 root_type
   * @returns {Promise<void>}
   */
  async initRootType() {
    const root_id = this.root["id"];
    const types = CONSTS.gd_root_type;
    if (root_id === "root" || root_id === authConfig.user_drive_real_root_id) {
      this.root_type = types.user_drive;
    } else {
      const obj = await this.getShareDriveObjById(root_id);
      this.root_type = obj ? types.share_drive : types.sub_folder;
    }
  }

  /**
   * Returns a response that requires authorization, or null
   * @param request
   * @returns {Response|null}
   */
  basicAuthResponse(request) {
    const user = this.root.user || "",
      pass = this.root.pass || "",
      _401 = new Response("Unauthorized", {
        headers: {
          "WWW-Authenticate": `Basic realm="goindex:drive:${this.order}"`,
        },
        status: 401,
      });
    if (user || pass) {
      const auth = request.headers.get("Authorization");
      if (auth) {
        try {
          const [received_user, received_pass] = atob(
            auth.split(" ").pop()
          ).split(":");
          return received_user === user && received_pass === pass ? null : _401;
        } catch (e) {}
      }
    } else return null;
    return _401;
  }

  async view(url, range = "", inline = true) {
    let requestOption = await this.requestOption();
    requestOption.headers["Range"] = range;
    let res = await fetch(url, requestOption);
    const { headers } = (res = new Response(res.body, res));
    this.authConfig.enable_cors_file_down &&
      headers.append("Access-Control-Allow-Origin", "*");
    inline === true && headers.set("Content-Disposition", "inline");
    return res;
  }

  async down(id, range = "", inline = false) {
    let url = `https://www.googleapis.com/drive/v3/files/${id}?alt=media`;
    let requestOption = await this.requestOption();
    requestOption.headers["Range"] = range;
    let res = await fetch(url, requestOption);
    const { headers } = (res = new Response(res.body, res));
    this.authConfig.enable_cors_file_down &&
      headers.append("Access-Control-Allow-Origin", "*");
    inline === true && headers.set("Content-Disposition", "inline");
    return res;
  }

  async file(path) {
    if (typeof this.files[path] == "undefined") {
      this.files[path] = await this._file(path);
    }
    return this.files[path];
  }

  async _file(path) {
    let arr = path.split("/");
    let name = arr.pop();
    name = decodeURIComponent(name).replace(/\'/g, "\\'");
    let dir = arr.join("/") + "/";
    // console.log(name, dir);
    let parent = await this.findPathId(dir);
    // console.log(parent);
    let url = "https://www.googleapis.com/drive/v3/files";
    let params = { includeItemsFromAllDrives: true, supportsAllDrives: true };
    params.q = `'${parent}' in parents and name = '${name}' and trashed = false`;
    params.fields =
      "files(id, name, mimeType, size ,createdTime, modifiedTime, iconLink, thumbnailLink)";
    url += "?" + this.enQuery(params);
    let requestOption = await this.requestOption();
    let response = await fetch(url, requestOption);
    let obj = await response.json();
    // console.log(obj);
    return obj.files[0];
  }

  // 通过reqeust cache 来缓存
  async list(path, page_token = null, page_index = 0) {
    if (this.path_children_cache == undefined) {
      // { <path> :[ {nextPageToken:'',data:{}}, {nextPageToken:'',data:{}} ...], ...}
      this.path_children_cache = {};
    }

    if (
      this.path_children_cache[path] &&
      this.path_children_cache[path][page_index] &&
      this.path_children_cache[path][page_index].data
    ) {
      let child_obj = this.path_children_cache[path][page_index];
      return {
        nextPageToken: child_obj.nextPageToken || null,
        curPageIndex: page_index,
        data: child_obj.data,
      };
    }

    let id = await this.findPathId(path);
    let result = await this._ls(id, page_token, page_index);
    let data = result.data;
    // 对有多页的，进行缓存
    if (result.nextPageToken && data.files) {
      if (!Array.isArray(this.path_children_cache[path])) {
        this.path_children_cache[path] = [];
      }
      this.path_children_cache[path][Number(result.curPageIndex)] = {
        nextPageToken: result.nextPageToken,
        data: data,
      };
    }

    return result;
  }

  async _ls(parent, page_token = null, page_index = 0) {
    // console.log("_ls", parent);

    if (parent == undefined) {
      return null;
    }
    let obj;
    let params = { includeItemsFromAllDrives: true, supportsAllDrives: true };
    params.q = `'${parent}' in parents and trashed = false AND name !='.password'`;
    params.orderBy = "folder,name,modifiedTime desc";
    params.fields =
      "nextPageToken, files(id, name, mimeType, size , modifiedTime, thumbnailLink, description)";
    params.pageSize = this.authConfig.files_list_page_size;

    if (page_token) {
      params.pageToken = page_token;
    }
    let url = "https://www.googleapis.com/drive/v3/files";
    url += "?" + this.enQuery(params);
    let requestOption = await this.requestOption();
    let response = await fetch(url, requestOption);
    obj = await response.json();

    return {
      nextPageToken: obj.nextPageToken || null,
      curPageIndex: page_index,
      data: obj,
    };

    /*do {
            if (pageToken) {
                params.pageToken = pageToken;
            }
            let url = 'https://www.googleapis.com/drive/v3/files';
            url += '?' + this.enQuery(params);
            let requestOption = await this.requestOption();
            let response = await fetch(url, requestOption);
            obj = await response.json();
            files.push(...obj.files);
            pageToken = obj.nextPageToken;
        } while (pageToken);*/
  }

  async password(path) {
    if (this.passwords[path] !== undefined) {
      return this.passwords[path];
    }

    // console.log("load", path, ".password", this.passwords[path]);

    let file = await this.file(path + ".password");
    if (file == undefined) {
      this.passwords[path] = null;
    } else {
      let url = `https://www.googleapis.com/drive/v3/files/${file.id}?alt=media`;
      let requestOption = await this.requestOption();
      let response = await this.fetch200(url, requestOption);
      this.passwords[path] = await response.text();
    }

    return this.passwords[path];
  }

  /**
   * 通过 id 获取 share drive 信息
   * @param any_id
   * @returns {Promise<null|{id}|any>} 任何非正常情况都返回 null
   */
  async getShareDriveObjById(any_id) {
    if (!any_id) return null;
    if ("string" !== typeof any_id) return null;

    let url = `https://www.googleapis.com/drive/v3/drives/${any_id}`;
    let requestOption = await this.requestOption();
    let res = await fetch(url, requestOption);
    let obj = await res.json();
    if (obj && obj.id) return obj;

    return null;
  }

  /**
   * 搜索
   * @returns {Promise<{data: null, nextPageToken: null, curPageIndex: number}>}
   */
  async search(origin_keyword, page_token = null, page_index = 0) {
    const types = CONSTS.gd_root_type;
    const is_user_drive = this.root_type === types.user_drive;
    const is_share_drive = this.root_type === types.share_drive;

    const empty_result = {
      nextPageToken: null,
      curPageIndex: page_index,
      data: null,
    };

    if (!is_user_drive && !is_share_drive) {
      return empty_result;
    }
    let keyword = FUNCS.formatSearchKeyword(origin_keyword);
    if (!keyword) {
      // 关键词为空，返回
      return empty_result;
    }
    let words = keyword.split(/\s+/);
    let name_search_str = `name contains '${words.join(
      "' AND name contains '"
    )}'`;

    // corpora 为 user 是个人盘 ，为 drive 是团队盘。配合 driveId
    let params = {};
    if (is_user_drive) {
      params.corpora = "user";
    }
    if (is_share_drive) {
      params.corpora = "drive";
      params.driveId = this.root.id;
      // This parameter will only be effective until June 1, 2020. Afterwards shared drive items will be included in the results.
      params.includeItemsFromAllDrives = true;
      params.supportsAllDrives = true;
    }
    if (page_token) {
      params.pageToken = page_token;
    }
    params.q = `trashed = false AND name !='.password' AND (${name_search_str})`;
    params.fields =
      "nextPageToken, files(id, name, mimeType, size , modifiedTime, thumbnailLink, description)";
    params.pageSize = this.authConfig.search_result_list_page_size;
    // params.orderBy = 'folder,name,modifiedTime desc';

    let url = "https://www.googleapis.com/drive/v3/files";
    url += "?" + this.enQuery(params);
    // console.log(params)
    let requestOption = await this.requestOption();
    let response = await fetch(url, requestOption);
    let res_obj = await response.json();

    return {
      nextPageToken: res_obj.nextPageToken || null,
      curPageIndex: page_index,
      data: res_obj,
    };
  }

  /**
   * 一层一层的向上获取这个文件或文件夹的上级文件夹的 file 对象。注意：会很慢！！！
   * 最多向上寻找到当前 gd 对象的根目录 (root id)
   * 只考虑一条单独的向上链。
   * 【注意】如果此id代表的项目不在目标gd盘下，那么此函数会返回null
   *
   * @param child_id
   * @param contain_myself
   * @returns {Promise<[]>}
   */
  async findParentFilesRecursion(child_id, contain_myself = true) {
    const gd = this;
    const gd_root_id = gd.root.id;
    const user_drive_real_root_id = authConfig.user_drive_real_root_id;
    const is_user_drive = gd.root_type === CONSTS.gd_root_type.user_drive;

    // 自下向上查询的终点目标id
    const target_top_id = is_user_drive ? user_drive_real_root_id : gd_root_id;
    const fields = CONSTS.default_file_fields;

    // [{},{},...]
    const parent_files = [];
    let meet_top = false;

    async function addItsFirstParent(file_obj) {
      if (!file_obj) return;
      if (!file_obj.parents) return;
      if (file_obj.parents.length < 1) return;

      // ['','',...]
      let p_ids = file_obj.parents;
      if (p_ids && p_ids.length > 0) {
        // its first parent
        const first_p_id = p_ids[0];
        if (first_p_id === target_top_id) {
          meet_top = true;
          return;
        }
        const p_file_obj = await gd.findItemById(first_p_id);
        if (p_file_obj && p_file_obj.id) {
          parent_files.push(p_file_obj);
          await addItsFirstParent(p_file_obj);
        }
      }
    }

    const child_obj = await gd.findItemById(child_id);
    if (contain_myself) {
      parent_files.push(child_obj);
    }
    await addItsFirstParent(child_obj);

    return meet_top ? parent_files : null;
  }

  /**
   * 获取相对于本盘根目录的path
   * @param child_id
   * @returns {Promise<string>} 【注意】如果此id代表的项目不在目标gd盘下，那么此方法会返回空字符串""
   */
  async findPathById(child_id) {
    if (this.id_path_cache[child_id]) {
      return this.id_path_cache[child_id];
    }

    const p_files = await this.findParentFilesRecursion(child_id);
    if (!p_files || p_files.length < 1) return "";

    let cache = [];
    // 把查出来的每一级的path和id都缓存一下
    p_files.forEach((value, idx) => {
      const is_folder =
        idx === 0 ? p_files[idx].mimeType === CONSTS.folder_mime_type : true;
      let path =
        "/" +
        p_files
          .slice(idx)
          .map((it) => it.name)
          .reverse()
          .join("/");
      if (is_folder) path += "/";
      cache.push({ id: p_files[idx].id, path: path });
    });

    cache.forEach((obj) => {
      this.id_path_cache[obj.id] = obj.path;
      this.paths[obj.path] = obj.id;
    });

    /*const is_folder = p_files[0].mimeType === CONSTS.folder_mime_type;
        let path = '/' + p_files.map(it => it.name).reverse().join('/');
        if (is_folder) path += '/';*/

    return cache[0].path;
  }

  // 根据id获取file item
  async findItemById(id) {
    const is_user_drive = this.root_type === CONSTS.gd_root_type.user_drive;
    let url = `https://www.googleapis.com/drive/v3/files/${id}?fields=${
      CONSTS.default_file_fields
    }${is_user_drive ? "" : "&supportsAllDrives=true"}`;
    let requestOption = await this.requestOption();
    let res = await fetch(url, requestOption);
    return await res.json();
  }

  async findPathId(path) {
    let c_path = "/";
    let c_id = this.paths[c_path];

    let arr = path.trim("/").split("/");
    for (let name of arr) {
      c_path += name + "/";

      if (typeof this.paths[c_path] == "undefined") {
        let id = await this._findDirId(c_id, name);
        this.paths[c_path] = id;
      }

      c_id = this.paths[c_path];
      if (c_id == undefined || c_id == null) {
        break;
      }
    }
    // console.log(this.paths);
    return this.paths[path];
  }

  async _findDirId(parent, name) {
    name = decodeURIComponent(name).replace(/\'/g, "\\'");

    // console.log("_findDirId", parent, name);

    if (parent == undefined) {
      return null;
    }

    let url = "https://www.googleapis.com/drive/v3/files";
    let params = { includeItemsFromAllDrives: true, supportsAllDrives: true };
    params.q = `'${parent}' in parents and mimeType = 'application/vnd.google-apps.folder' and name = '${name}'  and trashed = false`;
    params.fields = "nextPageToken, files(id, name, mimeType)";
    url += "?" + this.enQuery(params);
    let requestOption = await this.requestOption();
    let response = await fetch(url, requestOption);
    let obj = await response.json();
    if (obj.files[0] == undefined) {
      return null;
    }
    return obj.files[0].id;
  }

  async accessToken() {
    console.log("accessToken");
    if (
      this.authConfig.expires == undefined ||
      this.authConfig.expires < Date.now()
    ) {
      const obj = await this.fetchAccessToken();
      if (obj.access_token != undefined) {
        this.authConfig.accessToken = obj.access_token;
        this.authConfig.expires = Date.now() + 3500 * 1000;
      }
    }
    return this.authConfig.accessToken;
  }

  async fetchAccessToken() {
    console.log("fetchAccessToken");
    const url = "https://www.googleapis.com/oauth2/v4/token";
    const headers = {
      "Content-Type": "application/x-www-form-urlencoded",
    };
    const post_data = {
      client_id: this.authConfig.client_id,
      client_secret: this.authConfig.client_secret,
      refresh_token: this.authConfig.refresh_token,
      grant_type: "refresh_token",
    };

    let requestOption = {
      method: "POST",
      headers: headers,
      body: this.enQuery(post_data),
    };

    const response = await fetch(url, requestOption);
    return await response.json();
  }

  async fetch200(url, requestOption) {
    let response;
    for (let i = 0; i < 3; i++) {
      response = await fetch(url, requestOption);
      console.log(response.status);
      if (response.status != 403) {
        break;
      }
      await this.sleep(800 * (i + 1));
    }
    return response;
  }

  async requestOption(headers = {}, method = "GET") {
    const accessToken = await this.accessToken();
    headers["authorization"] = "Bearer " + accessToken;
    return { method: method, headers: headers };
  }

  enQuery(data) {
    const ret = [];
    for (let d in data) {
      ret.push(encodeURIComponent(d) + "=" + encodeURIComponent(data[d]));
    }
    return ret.join("&");
  }

  sleep(ms) {
    return new Promise(function (resolve, reject) {
      let i = 0;
      setTimeout(function () {
        console.log("sleep" + ms);
        i++;
        if (i >= 2) reject(new Error("i>=2"));
        else resolve(i);
      }, ms);
    });
  }
}

String.prototype.trim = function (char) {
  if (char) {
    return this.replace(
      new RegExp("^\\" + char + "+|\\" + char + "+$", "g"),
      ""
    );
  }
  return this.replace(/^\s+|\s+$/g, "");
};

```

## AriaNG

- 下载 https://github.com/mayswind/AriaNg/releases/download/1.1.6/AriaNg-1.1.6.zip
- 解压缩
- 上传到 github
- 使用 Github pages 服务，不开启 https

## VPS auto restart

refer: https://www.moerats.com/archives/120/

- yum install vixie-cron crontabs\

- 如果报错 Unable to find a match: vixie-cron

  ```
  yum -y install vim-enhanced.x86_64
  ```

- systemctl start crond / chkconfig crond on

- 编辑定时脚本

  ```
  crontab -e
  #每天凌晨1点重启服务器
  0 1 * * * /sbin/reboot
  #每3小时重启一次
  0 */3 * * * /sbin/reboot
  ```

- 重启 crond
  - systemctl stop crond
  - systemctl start crond / service crond start

