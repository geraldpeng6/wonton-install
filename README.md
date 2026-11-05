# 云吞安装脚本

在装好 Docker 的 Linux 主机（amd64、arm64）上执行：

    curl -fsSL https://raw.githubusercontent.com/geraldpeng6/wonton-install/main/install.sh | sudo sh

需要 Docker Engine 28 或更新的版本（Docker 官方仓库的 docker-ce）。不用另装 compose 插件，脚本用核心镜像里带的。

脚本装好云吞的核心、身份提供方（Ory Hydra）和 Matrix 服务器（Tuwunel），密钥都在这台主机上生成：

- `/opt/wonton`：compose 文件、`.env` 和密钥（只有 root 能读）
- `/var/lib/wonton`：数据
- `/usr/local/bin/wonton`：服务器上的命令

核心启动后在后台装上模型网关和默认的 Agent Hermes。Hermes 的镜像约 4 GB，全新主机上要等几分钟；在 App 的“模型设置”里
添加一家模型服务商的 key，就能和它聊天。

装好后终端里显示配对二维码，用云吞 App 扫码就成为主人；浏览器打开 `http://<主机的 IP>:8480/` 也能看到运行状态和
同一个二维码。防火墙要放行 TCP 8480 和 UDP 8482。

- 以后加设备：在已配对的 App 里点“添加设备”。
- 设备都丢了：`sudo wonton pair`。
- 升级：再执行一次安装命令，或者在 App 里一键升级。
- 退回旧版本：`curl -fsSL <安装脚本地址> | sudo WONTON_CORE_PIN=sha256:… sh`。

核心的镜像是 `ghcr.io/geraldpeng6/wonton-core`，1.0 之前装的是 `beta` 渠道；App 只在内测里分发。核心、Tuwunel 等
镜像在 GHCR，Hydra、Hermes 的在 Docker Hub；国内的主机常常拉不到。连不上镜像仓库时脚本打印给 Docker daemon 配代理的
做法，配好后再执行一次安装命令。可调的环境变量（端口、用户名、时区、出网代理等）见 `install.sh` 开头。
