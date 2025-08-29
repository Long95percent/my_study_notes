搭建一个属于自己的、与世隔离的“沙盒”练习靶场

**Docker + 故意构建的有漏洞的PHP应用**
1. **功能完整**：包含网站、数据库、登录等核心功能。
2. **环境隔离**：不污染主机系统，用完后可以轻松清理。
3. **模拟真实**：环境要尽可能接近真实的生产服务器。
4. **绝对安全**：只在本地运行，不与互联网交互，避免被黑或攻击他人。
### 核心方案：使用 Docker 容器

**Docker** 是解决你所有痛点的最佳工具。你可以把它理解为一个非常轻量级的“虚拟机”。

- **不弄脏电脑**：所有东西（PHP、MySQL、Apache/Nginx）都运行在一个隔离的“容器”中，与你的主机系统完全分离。删除容器就像删除一个文件夹一样干净。
    
- **模拟真实环境**：你可以使用几乎和真实服务器一模一样的软件版本和配置。
    
- **快速重置**：如果你把靶场“玩坏了”，只需几秒钟就能销毁容器并重新创建一个全新的。

### 详细步骤：手把手搭建你的个人靶场

#### 第一步：安装 Docker

1. 访问 Docker 官网的 [Docker Desktop 下载页面](https://www.docker.com/products/docker-desktop/)。
    
2. 根据你的操作系统（Windows / macOS / Linux）下载并安装对应的版本。
    
    - **对于Windows 10/11 Home版**：安装时会提示你需要使用 WSL 2（Windows Subsystem for Linux），按照指引操作即可，非常简单。
        
3. 安装完成后，启动 Docker Desktop。你会在任务栏（或菜单栏）看到一个小鲸鱼图标。

#### 第二步：选择你的“靶场”项目

不要从零开始写，直接使用现成的、专门为安全练习设计的漏洞应用。它们功能完整，自带数据库结构，并且故意留下了各种漏洞。

**顶级推荐项目：**

1. **Damn Vulnerable Web Application (DVWA)**
    
    - **简介**：这是最著名的Web漏洞练习平台，专为安全渗透测试学习而设计。它包含了SQL注入、XSS、文件上传、命令执行等几乎所有常见漏洞，并且可以调整漏洞的难度等级。
        
    - **GitHub**: [https://github.com/digininja/DVWA](https://github.com/digininja/DVWA)

#### 第三步：一键部署（以 DVWA 为例）

**这步需要vpn**

最简单的方式是使用现成的 `docker-compose` 配置文件，它定义了如何启动PHP、MySQL和Web服务器并将其连接在一起。
1. **新建一个项目文件夹**，比如在桌面上创建 `my_target_range`。
2. **创建 `docker-compose.yml` 文件**：在该文件夹中，新建一个文本文件，命名为 `docker-compose.yml`，用记事本或VS Code等编辑器打开，并粘贴以下内容：
```
services:
  dvwa:
    image: vulnerables/web-dvwa
    ports:
      - "80:80" # 将本机的80端口映射到容器的80端口
    environment:
      - MYSQL_ROOT_PASSWORD=root_password
      - MYSQL_USER=dvwa
      - MYSQL_PASSWORD=p@ssw0rd
      - MYSQL_DATABASE=dvwa
    # 如果你希望数据持久化，可以取消下面的注释
    # volumes:
    #   - ./dvwa:/app
```
这个文件告诉Docker：从网上拉取一个名为`vulnerables/web-dvwa`的镜像，运行它，并设置好数据库密码和端口。
3.  **启动靶场**：
    - 打开终端（命令行 / PowerShell / Terminal）。
    - 使用 `cd` 命令进入到刚才创建的 `my_target_range` 文件夹。
    - 输入以下命令并回车：
        docker-compose up -d
    - Docker会自动下载镜像并启动所有服务。第一次运行需要下载，稍等片刻。
4. **访问你的靶场**：
    - 打开你的浏览器，输入 `http://localhost`。
    - 你应该就能看到 DVWA 的首页了！
    - **首次设置**：页面可能会提示你点击链接 “Create / Reset Database” 来初始化数据库。点击后，系统会自动建表并插入数据
    - **登录**：默认用户名是 `admin`，密码是 `password`。

恭喜！你的个人练习靶场已经搭建完毕！它现在完全运行在你的电脑上，与外界网络隔绝，你可以放心地进行任何攻击测试.

### 如何开始练习？
1. **浏览功能**：在DVWA中，你可以看到有登录、留言板、文件上传等功能，这就是一个“正常网站”的简化版。
2. **选择漏洞模块**：左侧菜单栏有各种漏洞类型（如 SQL Injection, Command Injection, XSS 等）
3. **调整难度**：在 `DVWA Security` 选项卡中，你可以设置漏洞的难度（Low, Medium, High, Impossible），从最简单的开始尝试。
4. **使用工具**：你可以配合 **Burp Suite**（另一款强大的Web渗透测试工具）来拦截和修改发送给这个本地靶场的请求，进行更深入的测试。

### 当你练习结束后
- **暂停环境**：在项目文件夹中运行 `docker-compose stop`，环境会停止，但数据会保留。
- **彻底删除环境**：运行 `docker-compose down`，这会停止并删除所有容器。因为你没有做数据卷持久化（我们注释掉了`volumes`部分），所以下次 `up` 时又会是一个全新的、干净的环境。
- **完全清理**：如果你想释放磁盘空间，还可以使用 `docker system prune` 命令来清理所有不使用的镜像和容器网络。