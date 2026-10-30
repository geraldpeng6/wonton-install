# 云吞安装脚本

在装好 Docker 的 Debian 或 Ubuntu 主机（amd64、arm64）上执行：

    curl -fsSL https://raw.githubusercontent.com/geraldpeng6/wonton-install/main/install.sh | sudo sh

需要 Docker Engine 28 或更新的版本和 docker compose 插件（Docker 官方仓库的 docker-ce、docker-compose-plugin）。

脚本装好云吞的核心、身份提供方（Ory Hydra）和 Matrix 服务器（Tuwunel），密钥都在这台主机上生成：

- `/opt/wonton`：compose 文件、`.env` 和密钥（只有 root 能读）
- `/var/lib/wonton`：数据
- `/usr/local/bin/wonton`：服务器上的命令

装好后终端里显示配对二维码，用云吞 App 扫码就成为主人；浏览器打开 `http://<主机的 IP>:8480/` 也能看到运行状态和
同一个二维码。防火墙要放行 TCP 8480 和 UDP 8482。

- 以后加设备：在已配对的 App 里点“添加设备”。
- 设备都丢了：`sudo wonton pair`。
- 升级：再执行一次安装命令。

核心的镜像是 `ghcr.io/geraldpeng6/wonton-core`，1.0 之前装的是 `beta` 渠道；App 目前只在内测里分发。核心、Tuwunel 等
镜像在 GHCR，Hydra 的在 Docker Hub；国内的主机常常拉不到，拉镜像失败时先给 Docker 配镜像加速或代理，再执行一次安装
命令。国内的镜像仓库以后会加上。可调的环境变量见 `install.sh` 开头。
