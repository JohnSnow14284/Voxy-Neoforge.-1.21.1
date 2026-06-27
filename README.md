# Voxy NeoForge 1.21.1

# Voxy NeoForge 1.21.1 非官方移植版

An unofficial NeoForge 1.21.1 port of **Voxy**, the high-performance Level of Detail terrain renderer for Minecraft.

这是 **Voxy** 的非官方 NeoForge 1.21.1 移植版本。Voxy 是一个高性能远景 LOD 地形渲染 Mod，可以让玩家在不显著增加原版区块渲染压力的情况下看到更远的地形。

> All original Voxy credit belongs to [MCRcortex](https://github.com/MCRcortex), the creator of Voxy.
> Voxy 原作者为 [MCRcortex](https://github.com/MCRcortex)，本项目仅为非官方 NeoForge 移植与兼容性维护版本。

---

## Status / 当前状态

This port is currently **alpha** software. It builds for Minecraft 1.21.1 and NeoForge 21.1.x, but it is still being tested and stabilized.

当前版本仍属于 **alpha 测试阶段**。它可以在 Minecraft 1.21.1 与 NeoForge 21.1.x 环境下构建和运行，但仍建议在新存档或备份存档中测试后再加入长期整合包。

---

## What Works / 已实现功能

* Distant Level of Detail terrain rendering
  远距离 LOD 地形渲染

* Sodium-based rendering integration
  基于 Sodium 的渲染集成

* Sodium 0.8.12 video settings UI integration
  已集成到 Sodium 0.8.12 视频设置界面

* Client configuration through Sodium/Voxy settings
  可通过 Sodium / Voxy 设置界面调整配置

* FakeSight-style extended chunk request support
  集成 FakeSight 风格的扩展区块请求功能

* Configurable extended request distance
  可配置扩展区块请求距离

* Smooth fallback from missing fine LOD to coarse LOD
  当精细 LOD 尚未准备好时，会回退显示粗略 LOD，避免透明空洞

* Automatic refinement retry for coarse LOD sections
  粗略 LOD 会持续尝试细化，不会长期停留在低细节状态

* Improved high-altitude water rendering
  修复高空俯视时水体出现圆形空洞的问题

* Improved far-water seam handling
  改善非常远处 Voxy 水体区块之间的细小缝隙

* Block model baking for solid, cutout, cutout-mipped, and translucent render layers
  支持 solid、cutout、cutout-mipped、translucent 等渲染层的方块模型烘焙

---

## Recent Changes / 近期主要改动

### Sodium UI Integration / Sodium 设置界面集成

Voxy configuration has been integrated into Sodium's video settings UI through Sodium's configuration API.

Voxy 配置已通过 Sodium 配置 API 集成到 Sodium 的视频设置界面中。
进入游戏后可在以下位置找到 Voxy 配置：

```text
Options → Video Settings → Voxy
选项 → 视频设置 → Voxy
```

This is intended for Sodium `mc1.21.1-0.8.12-alpha.4-neoforge` or compatible builds.

该功能主要面向 Sodium `mc1.21.1-0.8.12-alpha.4-neoforge` 或兼容版本。

---

### FakeSight Integration / FakeSight 集成

This port includes a FakeSight-style extended chunk request . Thanks for song_5007.

本移植版集成了 FakeSight 的扩展区块请求功能，感谢大佬song_5007。

It allows the client to request a larger chunk distance than the normal vanilla render distance, which can help Voxy ingest more terrain data for distant LOD rendering.

该功能允许客户端请求比原版渲染距离更大的区块范围，从而帮助 Voxy 获取更多真实区块数据，用于远景 LOD 渲染。

Default behavior:

默认配置：

```text
Enable Extended Request Distance: true
Request Distance: 48 chunks
```

You can adjust these values in the Voxy configuration screen.

可以在 Voxy 配置界面中调整这些选项。

Recommended test values:

建议测试值：

```text
32 chunks
48 chunks
64 chunks
```

Very high values may increase server, network, and client load.

过高的请求距离可能增加服务器、网络和客户端负载。

---

### Distant Terrain Hole Fix / 远景空洞修复

A major issue where distant Voxy sections could appear as transparent holes has been fixed.

已修复一个严重问题：远处 Voxy 区块有时会显示为透明空洞。

The original issue was caused by the renderer stopping the coarse parent LOD mesh before the finer child LOD mesh was ready. The fix keeps rendering the coarse parent mesh as a fallback until the child mesh is available.

原问题的原因是：当 Voxy 判断应该切换到更精细的子级 LOD 时，如果子级 LOD 尚未准备好，原逻辑会停止渲染父级粗略 LOD，导致画面中出现透明空洞。
现在的逻辑会在子级 LOD 准备好之前继续显示父级粗略地形。

The fallback is also actively refined, so coarse terrain should automatically become more detailed instead of staying coarse for a long time.

同时，粗略地形会持续触发细化请求，因此不会长期停留在低细节状态。

---

### High-Altitude Water Fix / 高空水体空洞修复

A water rendering issue at high altitude has been improved.

已改善高空俯视时水体渲染异常的问题。

Previously, when flying very high above vanilla-loaded chunks, water surfaces could disappear in a circular area around the player. This was caused by the handoff between vanilla translucent water rendering and Voxy translucent water rendering.

此前在高空俯视原版加载区块时，玩家脚下海面或河流水面可能出现圆形空洞。
该问题与原版透明水体渲染和 Voxy 透明水体渲染之间的交接有关。

The fade logic now considers 3D camera distance, preventing Voxy water from fading out too early when vanilla water is no longer visible at high altitude.

现在水体淡出逻辑会考虑真实 3D 距离，避免高空时 Voxy 水体过早淡出。

---


## Known Limitations / 已知限制

* Client-side only; do not install this on a dedicated server.
  仅客户端使用，服务端未测试，请暂时不要安装到专用服务器。

* Requires the NeoForge build of Sodium.
  需要 NeoForge 版本的 Sodium。

* This is still an alpha port. Rendering behavior may still differ from upstream Fabric Voxy.
  当前仍为 alpha 移植版本，部分渲染行为可能与 Fabric 原版 Voxy 不完全一致。

* Shader compatibility is still experimental.
  光影兼容仍处于测试阶段。

* Some water handoff artifacts may still occur near the boundary between vanilla chunks and Voxy LOD chunks during very fast movement.
  在快速移动时，原版区块与 Voxy LOD 区块交界处的水体仍可能出现短暂颜色差异或闪烁。

* Very large FakeSight request distances may cause additional server or client load.
  过大的 FakeSight 请求距离可能增加服务器、客户端或网络压力。

---

## Requirements / 运行需求

| Requirement | Version                                        |
| ----------- | ---------------------------------------------- |
| Minecraft   | 1.21.1                                         |
| NeoForge    | 21.1.x                                         |
| Java        | 21                                             |
| Sodium      | mc1.21.1-0.8.12-alpha.4-neoforge or compatible |

| 需求        | 版本                                     |
| --------- | -------------------------------------- |
| Minecraft | 1.21.1                                 |
| NeoForge  | 21.1.x                                 |
| Java      | 21                                     |
| Sodium    | mc1.21.1-0.8.12-alpha.4-neoforge 或兼容版本 |

This version no longer requires Forgified Fabric API as a mandatory dependency.

当前版本不再强制要求 Forgified Fabric API 作为前置依赖。

Recommended optional mods:

推荐可选 Mod：

| Mod                    | Why                                        |
| ---------------------- | ------------------------------------------ |
| Lithium                | General game performance improvements      |
| Iris                   | Shader testing, if supported by your setup |
| Reese's Sodium Options | Optional Sodium settings UI enhancement    |

| Mod                    | 作用                |
| ---------------------- | ----------------- |
| Lithium                | 提升游戏整体性能          |
| Iris                   | 用于测试光影兼容性         |
| Reese's Sodium Options | 可选的 Sodium 设置界面增强 |

---

## Installation / 安装方式

1. Install Minecraft 1.21.1 with NeoForge 21.1.x.
   安装 Minecraft 1.21.1 与 NeoForge 21.1.x。

2. Install the required NeoForge build of Sodium.
   安装 NeoForge 版本的 Sodium。

3. Download the latest `voxy-*.jar` from this repository's GitHub Releases page.
   从本仓库 GitHub Releases 页面下载最新的 `voxy-*.jar`。

4. Place the Voxy jar in your `mods` folder.
   将 Voxy jar 放入 `mods` 文件夹。

5. Start the game and check that Voxy appears in the mod list.
   启动游戏，并确认 Mod 列表中出现 Voxy。

6. Open Sodium video settings and configure Voxy.
   打开 Sodium 视频设置页面，并调整 Voxy 配置。

---

## Configuration / 配置说明

Voxy options can be accessed from Sodium's video settings screen.

Voxy 配置可以从 Sodium 视频设置界面进入。

Recommended first-test settings:

建议首次测试设置：

```text
Voxy Render Distance: Start low, then increase gradually
Extended Request Distance: 32 or 48
Shaders: Disabled for first test
Vanilla Render Distance: 8 to 16
```

中文建议：

```text
Voxy 渲染距离：先设置较低，再逐渐提高
扩展区块请求距离：建议先使用 32 或 48
光影：首次测试建议关闭
原版渲染距离：建议 8 到 16
```

If you encounter rendering issues, test with shaders disabled first.

如果遇到渲染异常，请先关闭光影进行测试。

---

## Troubleshooting / 常见问题

### The Game Crashes on Startup / 游戏启动崩溃

* Confirm you are using Minecraft 1.21.1.
  确认你使用的是 Minecraft 1.21.1。

* Confirm you are using NeoForge 21.1.x.
  确认你使用的是 NeoForge 21.1.x。

* Confirm Sodium is the NeoForge build and compatible with this port.
  确认 Sodium 是 NeoForge 版本，并且与本移植版兼容。

* Remove shader packs and other rendering overhaul mods for the first test.
  首次测试时建议移除光影和其他大型渲染修改 Mod。

* Check `latest.log` for dependency or mixin errors.
  查看 `latest.log` 中是否存在依赖缺失或 mixin 报错。

---

### Distant Terrain Does Not Render / 远景地形不渲染

* Confirm Voxy is enabled in its configuration screen.
  确认 Voxy 配置中已启用渲染。

* Confirm Sodium is installed and active.
  确认 Sodium 已安装并正常运行。

* Lower Voxy render distance temporarily.
  暂时降低 Voxy 渲染距离。

* Lower Extended Request Distance if using FakeSight integration.
  如果启用了 FakeSight 扩展请求，请适当降低请求距离。

* Test in a fresh world or a copied save.
  建议在新世界或备份存档中测试。

---



### Shader Issues / 光影问题

Shader support is experimental during the port.

光影支持仍处于测试阶段。

If a shader pack breaks Voxy rendering, test without the shader pack and report:

如果某个光影导致 Voxy 渲染异常，请先关闭光影测试，并反馈：

```text
Minecraft version
NeoForge version
Sodium version
Iris version
Voxy version
Shader pack name
latest.log
```

---

## Building From Source / 从源码构建

Clone the repository and run Gradle:

克隆仓库并运行 Gradle：

```bash
git clone https://github.com/yarnobachmann/Voxy-Neoforge.-1.21.1.git
cd Voxy-Neoforge.-1.21.1
./gradlew build
```

On Windows:

Windows 下：

```powershell
git clone https://github.com/yarnobachmann/Voxy-Neoforge.-1.21.1.git
cd Voxy-Neoforge.-1.21.1
.\gradlew.bat build
```

The compiled jar will be written to:

构建完成后的 jar 位于：

```text
build/libs/
```

---

## Development Notes / 开发说明

This repository is a NeoForge 1.21.1 port of the Fabric Voxy codebase. Porting work should be tested carefully, especially when changing mixins, shaders, Sodium integration points, or render pipeline behavior.

本仓库是 Fabric Voxy 代码库的 NeoForge 1.21.1 移植版本。
修改 mixin、shader、Sodium 集成点或渲染管线时应谨慎测试。

Important files:

重要文件：

```text
build.gradle
gradle.properties
src/main/resources/META-INF/neoforge.mods.toml
src/main/resources/client.voxy.mixins.json
src/main/resources/common.voxy.mixins.json
src/main/resources/iris.voxy.mixins.json
src/main/resources/fakesight.voxy.mixins.json
```

Key rendering-related files:

关键渲染相关文件：

```text
src/main/java/me/cortex/voxy/client/core/NormalRenderPipeline.java
src/main/java/me/cortex/voxy/client/core/IrisVoxyRenderPipeline.java
src/main/java/me/cortex/voxy/client/core/rendering/hierachical/NodeManager.java
src/main/java/me/cortex/voxy/client/core/rendering/hierachical/NodeChildRequest.java
src/main/java/me/cortex/voxy/client/core/rendering/SectionUpdateRouter.java
src/main/resources/assets/voxy/shaders/lod/hierarchical/traversal_dev.comp
src/main/resources/assets/voxy/shaders/lod/gl46/quads.frag
src/main/resources/assets/voxy/shaders/lod/gl46/quads3.vert
```

---

## Credits / 鸣谢

* [MCRcortex](https://github.com/MCRcortex) - Original Voxy author
  Voxy 原作者

* [Original Voxy repository](https://github.com/MCRcortex/voxy)
  原版 Voxy 仓库

* NeoForge contributors
  NeoForge 贡献者

* Sodium contributors
  Sodium 贡献者

* Iris contributors
  Iris 贡献者
  
* FakeSight contributors
  FakeSight 贡献者

* The Minecraft modding community
  Minecraft Mod 开发社区

---

## License / 许可证

See [LICENSE.md](LICENSE.md).

请查看 [LICENSE.md](LICENSE.md)。

This is an unofficial port and is not affiliated with Mojang, Microsoft, NeoForge, Sodium, Iris, or the original Voxy project.

这是一个非官方移植版本，与 Mojang、Microsoft、NeoForge、Sodium、Iris 或原版 Voxy 项目无官方关联。
