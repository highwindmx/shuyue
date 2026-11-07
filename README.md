# 数音盒

> 从数学公式出发生成音乐的单文件浏览器应用：把 f(n) 的第 n 个值当成第 n 个音，函数值决定音高，n 决定时间。

打开一个 HTML 文件就能玩。公式、细胞自动机、混沌递推、素数间隔、π 的小数位都可以直接变成旋律、和弦与鼓点；每条音轨的数学曲线与钢琴卷帘同屏显示，播放头贯穿上下。零依赖、零构建、零网络请求，全部声音在本机实时合成。

## 功能特性

-   **公式引擎**：自写递归下降解析器（tokenize → Pratt AST → evalNode，白名单函数，**不使用 eval**），支持变量 n / t / x、常量 PI / E / PHI / TAU、四则与幂取模，以及 sin cos frac fibmod prime popcount collatz pidigit gcd 等函数
-   **π 不是硬编码**：小数位由 Machin 反正切公式用 BigInt 定点级数现算前 300 位；斐波那契走 Pisano 递推，大 n 不丢精度
-   **43 个预设**：23 条公式 + 5 条一维 CA + 4 种二维动力系统 + 7 条交叉轨 + 4 个自定义入口，分 8 组选单
-   **多轨工作台**：可增删音轨，每轨独立设置公式、音色、音阶、音量、声像、相位平移、移调、时值、静音 / 独奏、单轨倒放
-   **19 种音色**：4 种基础波形、钢琴、尼龙 / 钢弦吉他、拨弦、钟琴、管风琴、电贝斯、合成低音，以及鼓组与 6 件独立打击乐；吉他用 Karplus-Strong 弦模型，鼓用扫频正弦加滤波白噪声
-   **复音织体**：二维细胞自动机（生命游戏 / Brian's Brain / Seeds / Day & Night）每步生成 0–6 个音的和弦或休止，网格快照随播放头刷新
-   **参数调制**：公式 B 可调制公式 A 的音高，也可只改音量 / 声像 / 时值 / 触发门（共 9 种模式），谱面一个音都不动
-   **可视化**：每轨上半区画连续 f(t) 淡线、采样插值实线、空心采样点，或 CA 演化图案 / 2D 网格；下半区是该轨钢琴卷帘，竖叠表示和弦、条高表示音量、偏移表示声像
-   **播放控制**：默认循环，支持暂停恢复、全局倒放、倒影、10 个一键组合，空格与 L / R / I / S / A 快捷键
-   **导出**：立体声 WAV（OfflineAudioContext 离线渲染）、多轨 MIDI format 1；打击乐轨自动走 channel 10 并映射 GM 鼓键
-   **历史版本保留**：V1 至 V5 全部原样留档，可直接对照迭代过程

## 系统要求

-   现代桌面浏览器（Chrome / Edge / Firefox / Safari），需支持 Web Audio API 与 Canvas 2D
-   窗口 1440×900 及以上体验最佳，默认 3 轨一屏完整显示
-   无需 Node.js、Python、构建工具、后端服务、安装程序或账号
-   首次点击「播放」时浏览器会解锁音频上下文，属正常行为

## 构建命令

本项目零构建，无需任何安装步骤。直接打开当前版本即可：

```bat
:: Windows
start "" "index-V5.html"
```

```bash
# macOS
open index-V5.html

# Linux
xdg-open index-V5.html
```

如需通过本地 HTTP 访问（可选，file:// 亦可用）：

```bash
python -m http.server 8080
```

然后访问 http://localhost:8080/index-V5.html

## 使用说明对照表

| 操作 | 效果 |
| --- | --- |
| 打开 index-V5.html | 载入当前版本，默认 3 轨、循环开启 |
| 空格 | 播放 / 暂停恢复，时间轴冻结后严格接上 |
| L / R / I / S / A | 循环 / 全局倒放 / 全局倒影 / 停止 / 新增音轨 |
| 顶部「一键组合」 | 一次载入 3 轨公式与音色配置，共 10 组 |
| 轨内公式选单 | 在 43 个预设与自定义入口间切换，按数学类别分组 |
| 直接改公式输入框并回车 | 立刻换成自己的 f(n)；x 表示上一步的值，可写递推 |
| 音色选单 | 19 种音色按 4 组分类；鼓组按该轨音高分位自动分配 6 件鼓 |
| 音量 / 声像滑块 | 边播边拖即时生效，不中断播放 |
| 平移滑块 | 整轨按步数循环移位；两条同公式轨错开即为卡农 |
| 勾选倒影 / 倒放 | 对序列做十二音技法的对称变换，卷帘同步重绘 |
| 导出 WAV / MIDI | 立体声音频或可拖进 DAW 的多轨工程 |

### 版本对照表

| 文件 | 版本 | 主要增量 |
| --- | --- | --- |
| index.html | V1 | 单序列播放器，8 个公式预设，WAV / MIDI 导出 |
| index-V2.html | V2 | 多轨叠加、顶部吸附播放条、暂停 / 循环 / 倒放、23 个预设 |
| index-V3.html | V3 | 一屏布局、一维细胞自动机、交叉轨、数学曲线可视化 |
| index-V4.html | V4 | 二维动力系统、复音和弦与休止、B 调制音量 / 声像 / 时值 / 触发门、逐音符声像 |
| index-V5.html | V5（当前） | 控件左右分区、默认循环、19 种音色与打击乐、MIDI 鼓通道 |

> 版本文件一律另存新增，旧版不覆盖，便于逐版对照与回退。

## 技术栈

-   **前端**：原生 HTML / CSS / JavaScript，单文件，零依赖，零构建
-   **音频**：Web Audio API（OscillatorNode、GainNode、BiquadFilterNode、StereoPannerNode、AudioBufferSourceNode、DelayNode、OfflineAudioContext）
-   **绘图**：Canvas 2D，Catmull-Rom 转 Bezier 平滑插值
-   **数学**：自写表达式解析器与 AST 求值、Machin 反正切 BigInt 定点级数、Wolfram 一维 CA、二维环形网格 CA、Karplus-Strong 弦模型
-   **导出**：手写 16-bit PCM WAV 编码、手写 SMF format 1（含 tempo meta、轨道名、CC7 / CC10、GM 鼓映射）

## 目录结构

```text
├── README.md           # 本文档
├── LICENSE             # MIT 许可证
├── .gitignore          # 排除本地工作区数据与导出物
├── index.html          # V1 单序列播放器（历史版本）
├── index-V2.html       # V2 多轨与播放状态
├── index-V3.html       # V3 一屏布局 / 一维 CA / 交叉轨 / 数学曲线
├── index-V4.html       # V4 二维 CA / 复音织体 / 参数调制
└── index-V5.html       # V5 当前版本，打开这个
```

> `.workbuddy/` 为本地工作区数据（记忆日志等），已由 .gitignore 排除，不进入仓库。

## 许可证

[MIT](LICENSE) © 2026 highwindmx

Powered By Workbuddy*Hy4
