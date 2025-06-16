# Emby Intel GPU 定制版

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

1. 克隆仓库：
```bash
git clone https://github.com/yourusername/emby_intelgpu.git
cd emby_intelgpu
```

2. 构建 Docker 镜像（仅构建正式版本）：
```bash
# 查看可用的版本标签
git tag

# 构建特定版本的镜像
git checkout v1.1  # 切换到要构建的版本标签
docker build -t emby_intelgpu:v1.1 .
```

3. 运行容器：
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
  emby_intelgpu:v1.1
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