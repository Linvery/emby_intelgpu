# Emby Intel GPU 定制版

Docker Hub: [linvery/emby_intelgpu](https://hub.docker.com/r/linvery/emby_intelgpu)

这是一个基于 Arch Linux 的 Emby 媒体服务器定制版本，专门针对 Intel GPU 硬件加速进行了优化，特别支持 Intel DG1 显卡的 QSV 硬件解码。

## 特性

- 基于 Arch Linux 最新版本
- 支持 Intel QSV 硬件解码
- 特别优化支持 Intel DG1 显卡
- 包含最新的 Intel Media Driver 和 Intel Compute Runtime
- 预装所有必要的编解码器支持
- 已在飞牛 + DG1 + 应用商店i915-sriov-dkms驱动下验证运行

## 系统要求

- Docker 环境
- Intel GPU (特别支持 DG1 显卡)
- 至少 4GB RAM
- 至少 10GB 可用存储空间

## 快速开始

### 方法一：使用 Docker Compose（推荐）

1. 克隆仓库：
```bash
git clone https://github.com/yourusername/emby_intelgpu.git
cd emby_intelgpu
```

2. 配置 docker-compose.yml：
   - 打开 `docker-compose.yml` 文件
   - 修改以下配置：
     - `PUID`：设置为您当前用户的 UID（默认 1000）
     - `PGID`：设置为您当前用户的 GID（默认 1000）
     - `TZ`：设置为您所在的时区（例如：Asia/Shanghai）
     - 修改卷挂载路径：
       - `/path/to/config` 替换为 Emby 配置文件的存储路径
       - `/path/to/media` 替换为您的媒体文件存储路径

3. 启动服务：
```bash
# 启动 Emby 服务
docker-compose up -d

# 查看运行日志
docker-compose logs -f
```

4. 访问服务：
   - 打开浏览器访问 `http://localhost:8096`
   - 首次访问时，按照向导完成 Emby 的初始设置

### 方法二：使用 Docker Run

如果您更喜欢使用 docker run 命令，可以使用以下命令：

```bash
docker run -d \
  --name=emby \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Shanghai \
  -p 8096:8096 \
  -p 8920:8920 \
  -v /path/to/config:/config \
  -v /path/to/media:/media \
  --device=/dev/dri:/dev/dri \
  linvery/emby_intelgpu:latest
```

### 方法三：从源码构建（仅用于开发或测试）

如果您需要从源码构建镜像：

```bash
# 查看可用的版本标签
git tag

# 切换到要构建的版本
git checkout v1.1  # 替换为您需要的版本标签

# 构建镜像
docker build -t emby_intelgpu:v1.1 .
```

## 硬件加速配置

1. 确保主机系统已正确安装 Intel GPU 驱动
2. 在 Emby 管理界面中：
   - 进入 "管理" -> "服务器" -> "转码"
   - 启用硬件加速
   - 选择 "Intel QuickSync (QSV)"

## 注意事项

- 首次启动可能需要几分钟时间
- 确保 Docker 容器有权限访问 GPU 设备
- 建议使用最新版本的 Docker

## 故障排除

如果遇到硬件加速问题：

1. 检查 GPU 设备权限：
```bash
ls -l /dev/dri
```

2. 验证 Intel 驱动是否正确加载：
```bash
docker exec emby vainfo
```

3. 检查容器日志：
```bash
docker logs emby
```

## 致谢

本项目参考了以下资源：
- [Emby 官方文档](https://github.com/MediaBrowser/Emby.Releases)
- [Intel Media Driver](https://github.com/intel/media-driver)
- [Arch Linux 包](https://archlinux.org/packages/extra/x86_64/emby-server)
- [Intel GPU 解码支持](https://blog.jiawei.xin/?p=1902) - 作者：jiawei
- [Emby 方案](https://blog.jiawei.xin/?p=469) - 作者：jiawei
- [Emby 文件](https://cf.mb6.top/tmp/?dir=emby)

特别感谢 jiawei 在 Intel GPU 硬件解码和 Emby 解锁方面提供的技术支持，本项目基于 jiawei 的方案构建镜像。

## 许可证

本项目采用 MIT 许可证 