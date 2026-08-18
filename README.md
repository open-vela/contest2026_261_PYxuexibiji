#OpenVela ESP32-P4 平台适配

## 一、作品简介

将 OpenVela 操作系统移植至 ESP32-P4 平台，完成板级支持包开发，实现显示、视觉、网络、存储等核心外设驱动，构建具备多媒体处理与边缘连接能力的嵌入式软件基座。

## 二、选题方向

新硬件适配。ESP32-P4 是乐鑫新一代高性能 RISC-V 应用处理器，集成丰富多媒体接口与 AI 指令集，但 OpenVela 尚未官方支持该平台。通过完成 BSP 移植，可为智能音箱、工业网关、边缘 AI 终端等产品提供底层系统支撑。

## 三、目录结构

- `board/esp32p4/` — 板级支持包源码（Kconfig、设备树、启动代码、驱动）
- `logs/` — AI Coding 日志
- `README.md` — 作品说明（本文件）

## 四、运行方式

1. 拉取工程：
```bash
repo init -u https://github.com/open-vela/contest2026_261_PYxuexibiji -b dev-ai-contest-2026 -m contest2026_261_PYxuexibiji.xml
repo sync -c -j8
```

2. 编译：
```bash
cd ..
./build.sh vendor/openvela/boards/contest2026_261_board/esp32p4 menuconfig
./build.sh vendor/openvela/boards/contest2026_261_board/esp32p4 -j8
```

3. 烧录运行：
使用 esptool 将生成的固件烧录至 ESP32-P4 开发板，串口连接后可进入 NSH Shell。

## 五、AI Coding 使用说明

本作品在需求拆解、驱动框架设计、寄存器配置、调试排错等环节均借助 AI 辅助完成。AI 帮助快速定位 NuttX 驱动适配接口、分析 ESP32-P4 TRM 寄存器定义、生成初始化时序代码，显著提升了 BSP 开发效率。完整对话日志见 `logs/` 目录。

## 六、作品规划

OpenVela 操作系统 ESP32-P4 平台适配规划

一、项目背景
OpenVela 是面向物联网场景的开源实时操作系统，具备轻量化、模块化、高可扩展等特性。ESP32-P4 是乐鑫新一代高性能应用处理器，集成双核 RISC-V CPU、AI 指令集、丰富多媒体接口及无线扩展能力。本项目旨在将 OpenVela 完整移植至 ESP32-P4 平台，构建具备显示、视觉、网络、存储等核心能力的嵌入式软件基座。

二、总体目标
完成 OpenVela 在 ESP32-P4 上的板级支持包开发，实现内核稳定运行、基础外设驱动全覆盖、关键子系统可用，形成可复用的参考设计方案。

三、阶段规划

第一阶段：基础平台搭建
完成芯片时钟、中断、GPIO、UART、Timer、Watchdog 等底层基础设施适配，建立基础编译与烧录流程，确保内核能够正常启动并进入 Shell 交互。

第二阶段：多媒体子系统
显示模块：适配 DSI 接口控制器，完成显示链路初始化、时序配置与帧缓冲管理，支持图形界面渲染输出。
摄像头模块：适配 CSI 接口与图像传感器通路，实现图像数据采集、格式转换及预览功能，为视觉应用提供底层支持。

第三阶段：连接与存储能力
网络模块：适配片上以太网 MAC 及 PHY 层驱动，完成链路协商、数据收发与协议栈对接，实现有线网络连通。
存储模块：适配 SD 卡控制器，支持 SDMMC 协议通信、块设备注册及文件系统挂载，满足数据持久化需求。

第四阶段：系统优化与测试
开展稳定性压力测试、性能调优、功耗优化及文档整理，输出完整的 BSP 开发指南与示例工程。

四、预期成果
形成一套成熟的 ESP32-P4 OpenVela BSP，覆盖主流外设驱动，具备多媒体处理、网络通信与本地存储能力，可作为智能音箱、工业网关、边缘 AI 终端等产品的技术原型。

## 七、已知问题

1. DSI 屏幕需要 reboot 后才能正常显示，首次上电存在初始化时序异常，需排查复位信号与电源稳定等待时间。
2. SD 卡需要先不插卡上电一次，然后再插卡重新上电才能正常挂载，疑似卡检测引脚电平或上电时序与 SDMMC 控制器初始化不匹配。
3. CSI 摄像头初始化耗时较长，从驱动加载到首帧出图需要较长时间，需优化传感器上电、I2C 配置及 DMA 缓冲区分配流程。

# contest2026_261_PYxuexibiji

👋 欢迎参加 **2026 首届 openvela AI 硬件开发者大赛**！

这是组委会为你的队伍创建的**专属参赛仓库**（本仓为样例/模板，队伍编号 `261`；你看到的将是你自己的 `contest2026_<编号>_<队伍名>`）。比赛期间，你的全部参赛代码、打包产物与 AI Coding 日志都提交到这里。

> 本仓既是「代码仓」，又内置了一键拉取整套 openvela 工程的 `repo` 清单（manifest）。你只需跟它打交道，**自始至终只动一个文件夹**。

---

## 一、先读这些官方文档

**通用（所有赛道必读）：**

| 文档                                                                                                                                     | 用途                                           |
| ---------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| [《大赛总览》](https://github.com/open-vela/docs/blob/dev-ai-contest-2026/zh-cn/contest_2026/contest_overview.md)                        | 赛道、流程、评分、资源，建议先通读             |
| [《参赛代码提交指南》](https://github.com/open-vela/docs/blob/dev-ai-contest-2026/zh-cn/contest_2026/code_submission_guide.md)           | 仓库获取、提交流程、时间与权限（**以此为准**） |
| [《AI Coding 日志归集与提交手册》](https://github.com/open-vela/docs/blob/dev-ai-contest-2026/zh-cn/contest_2026/ai_coding_log_guide.md) | 如何导出 AI 对话日志并提交到 `logs/`           |

**按你的赛道选读（三选一）：**

| 赛道                  | 教程导航                                                                                                                                                 |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 快应用 / 手表应用创新 | [快应用教程导航](https://github.com/open-vela/docs/blob/dev-ai-contest-2026/zh-cn/contest_2026/quickapp/quickapp_guide_index.md)                         |
| AI 硬件产品创新       | [AI 硬件赛道教程导航](https://github.com/open-vela/docs/blob/dev-ai-contest-2026/zh-cn/contest_2026/ai_hardware/ai_hardware_guide_index.md)              |
| 新硬件适配            | [新硬件适配赛道教程导航](https://github.com/open-vela/docs/blob/dev-ai-contest-2026/zh-cn/contest_2026/hardware_porting/hardware_porting_guide_index.md) |

---

## 二、第一步：拉取完整工程

用组委会提供的命令一键拉取「openvela 全量源码 + 你的专属仓」：

```bash
repo init -u https://github.com/open-vela/contest2026_261_PYxuexibiji \
  -b dev-ai-contest-2026 -m contest2026_261_PYxuexibiji.xml
repo sync -c -j8
```

同步后，你的整个仓库位于工作区的 `contest2026_261_PYxuexibiji/`，openvela 全量源码在外层（`nuttx/`、`apps/`、`packages/`、`vendor/` 等）。

---

## 三、第二步：在哪里写代码

**只在自己的仓目录 `contest2026_261_PYxuexibiji/` 里开发。** 不同作品形态放在对应子目录，manifest 会通过 `<linkfile>` 把它们**软链**到 openvela 编译树该在的位置——你不用手动 copy：

| 作品形态 | 你的代码放这里             | 系统自动映射到                                 |
| -------- | -------------------------- | ---------------------------------------------- |
| 应用     | `app/hello_app/`           | `packages/demos/contest2026_261_hello_app`     |
| 快应用   | `quickapp/hello_quickapp/` | `packages/apps/contest2026_261_hello_quickapp` |
| 板级适配 | `board/contest_board/`     | `vendor/openvela/boards/contest2026_261_board` |

> 用不到的形态目录可以删掉；新增作品时按同样规则加子目录，并在 `contest2026_261_PYxuexibiji.xml` 里补一条 `<linkfile>` 映射即可。**生产仓库（packages/nuttx/vendor 等）零改动。**

建议仓库目录约定（便于评委定位）：

```text
app/ | quickapp/ | board/   # 你的作品代码
logs/                       # AI Coding 日志（主动导出后提交，格式见 logs/README.md）
README.md                   # 作品说明（提交前请改成你自己的，见第六节）
```

> 仓内附带了一个 `.gitignore.example`，给出了**编译产物**等不需要进仓的文件示例。如需启用，`cp .gitignore.example .gitignore` 后按需增删即可。**注意 `logs/` 下最终导出的 AI Coding 日志必须提交，不要忽略。**
>
> `logs/` 的目录结构与提交格式见 [logs/README.md](logs/README.md)。

---

## 四、第三步：编译与运行

编译/运行步骤随作品形态不同而不同，请参考你所在赛道的教程导航：

- 快应用 / 手表应用：[快应用教程导航](https://github.com/open-vela/docs/blob/dev-ai-contest-2026/zh-cn/contest_2026/quickapp/quickapp_guide_index.md)（含模拟器与开发板部署）。
- AI 硬件产品创新：[AI 硬件赛道教程导航](https://github.com/open-vela/docs/blob/dev-ai-contest-2026/zh-cn/contest_2026/ai_hardware/ai_hardware_guide_index.md)（环境搭建、编译烧录、Skill 开发）。
- 新硬件适配：[新硬件适配赛道教程导航](https://github.com/open-vela/docs/blob/dev-ai-contest-2026/zh-cn/contest_2026/hardware_porting/hardware_porting_guide_index.md)（BSP 移植、最小 NSH 基线）。

子目录已通过 manifest 中的 `<linkfile>` 软链进 openvela 编译树，因此构建在 openvela 工作区**根目录**（即你这个仓的上一级）进行。openvela 使用 `build.sh` 作为统一入口，接收一个 **board config 路径**作为参数：

```bash
# 进入 openvela 工作区根目录（你的仓的上一级）
cd ..

# 通用语法：第一个参数是 board config 路径，第二个参数可以是 menuconfig / distclean 等
./build.sh <board-config-path> [menuconfig|distclean] [-j8]
```

> 具体的 board config 路径、目标产物、模拟器/真机部署方式请以你所在赛道的教程导航为准。本仓 `app/` `quickapp/` `board/` 三个示例骨架对应的 Kconfig 选项可通过 `menuconfig` 启用。

---

## 五、第四步：提交作品

1. **fork** 你的专属仓 → 开发 → `git commit` 并推送 → 向专属仓发起 **Pull Request**，可**自行 review 并合入**（无需等组委会）。
2. **AI Coding 日志**：与 AI 工具的对话会自动记录到本机 staging（不会自动上传），需你**主动导出/打包**选定会话到仓内 `logs/` 目录后一并提交。详见[《AI Coding 日志归集与提交手册》](https://github.com/open-vela/docs/blob/dev-ai-contest-2026/zh-cn/contest_2026/ai_coding_log_guide.md)。
3. 若需改动 **nuttx 等公共仓库**，不在本仓改，而是 fork 对应公共仓、以 PR 提交到 `dev-ai-contest-2026` 分支，由组委会 review 后合入。

> ⏰ **提交作品截止：9 月 20 日**。截止后统一收回 push 权限，仍可查看 / clone。
>
> 获奖后再按要求将作品 PR 至 openvela 上游对应仓库（走标准 PR + CI 流程）。

### 关于 PR 与 CLA

- 本仓所有改动通过 **Pull Request** 合入（分支保护强制，可自行合入自己的 PR）。
- 首次贡献需在[**官网签署 CLA**](https://openvela.com/#/community/cla)；PR 上会自动跑 `cla/signature` 检查，在官网签署成功后，在 PR 评论 `/check-cla` 复检即可通过。

---

## 六、提交前：把本 README 改成你的作品说明

本文件目前是组委会给的**使用说明书**。**作品提交前，请把它替换成你自己作品的说明**，方便评委快速了解你做了什么、怎么跑起来。建议至少包含以下内容：

```markdown
# <你的作品名>

## 一、作品简介
<一句话/一段话说明这个作品是什么、解决什么问题、亮点在哪>

## 二、选题方向
<快应用 / 手表应用创新 ｜ AI 硬件产品创新 ｜ 新硬件适配 ｜ 自定方向，并简述理由>

## 三、目录结构
<列出你这个仓里各目录/文件的作用，例如：>
- `app/xxx/`        — <说明>
- `board/xxx/`      — <说明>
- `quickapp/xxx/`   — <说明>
- `logs/`           — AI Coding 日志
- `docs/` 或其他    — <说明>

## 四、运行方式
<拉取工程后，如何编译、烧录/部署、运行的完整步骤；最好能让评委照着一步步复现>

## 五、AI Coding 使用说明
<说明本作品如何借助 AI 辅助开发：
- 在需求拆解 / 方案设计 / 编码 / 调试 / 文档等环节如何与 AI 协作；
- AI 对开发效率或质量带来的实际帮助。
完整对话日志见 logs/ 目录>
```

> 提示：将会根据「作品本身 + 你的 README 说明 + `logs/` 里的 AI Coding 日志」来理解和评估你的作品，README 写清楚很重要。

---

## 附：仓库命名规范

`contest2026_<编号>_<队伍名>` — 编号三位零填充；队名 slug（全小写、英文/拼音、连字符）。例：`contest2026_261_PYxuexibiji`。
（仓库由组委会统一创建，**每队仅一个仓**，无需自行命名。）
