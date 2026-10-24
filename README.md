# 大肥鱼直播 · 可移植包

把「B 站刷片 → 分析 → 写讲稿 → 播视频 + 念稿 + 读弹幕回答」整套直播流程搬到另一台电脑。

> 本包**不含任何 API Key**，也**不含本机人设与世界记忆**（使用社区通用版「鲸鱼娘·大肥鱼」基础设定）。

## 下载

前往 [**Releases**](https://github.com/yuanstarstar/dafeiyu-live-portable/releases) 下载最新便携包：

- `dafeiyu-live-portable-20261024.zip`（约 3.3 MB）

解压后按下面的步骤安装即可。

## 环境要求

| 项 | 要求 | 说明 |
| --- | --- | --- |
| 操作系统 | Windows 10/11 | 声音播放、结束直播姬依赖 Windows 实现 |
| Python | 3.11+（实测 3.14.4） | 安装时勾选 "Add python.exe to PATH" |
| ffmpeg | 建议放进 PATH | 切片、抽帧、合成；找不到会尝试 `LIVEHUB_FFMPEG` |
| Node.js | 可选 | 仅「VLM 插件」分析视频时需要 |
| B 站账号 | 需要 | 扫码登录后才有推荐流、字幕、发弹幕、下播能力 |
| 哔哩哔哩直播姬 | 另装 | 本包不含；用「浏览器源」指向 `http://127.0.0.1:8788/overlay` |

## 三步跑起来

1. **解压到纯英文路径**（例如 `D:\dafeiyu-live`），双击 `install.cmd`
   - 自动建 `.venv`、装依赖、生成 `config.yaml`
2. **配置密钥**（用环境变量，不要写进 `config.yaml`、不要提交）
   - `XIAOMI_API_KEY`：VLM 分析用（没有则退化为本地分析，仍能跑）
   - `DEEPSEEK_API_KEY`：写讲稿 / 弹幕问答用（强烈建议）
3. **扫码登录并启动**
   - 双击 `login_bili.cmd` 扫码登录 B 站
   - 双击 `run_launcher.cmd` 打开启动器；服务监听 `http://127.0.0.1:8788`

## 连接直播姬

1. 直播姬新建场景 → 添加「浏览器 / 网页」源 → URL `http://127.0.0.1:8788/overlay`，1920×1080
2. 右键该源 → 属性 → 勾选「通过 OBS 控制音频」
3. 桌面音频保持默认采集，即可同时收到 TTS 与页面原声
4. 推流码自行填写；定时下播会调用 B 站接口关直播间并结束直播姬

## 目录说明

```
app/        FastAPI 服务（/api/*、/media、/overlay、/console、/watch）
livehub/    核心：拉片、待播池、分析与写稿、导演、运行时、TTS、弹幕、防火墙
web/        直播画面 / 主播控制台 / 观众页
assets/     立绘、贴纸、水印等素材
tools/      扫码登录、轮播、缓存清理、自检、生成讲稿等命令行工具
```

## 常见问题

- **没有声音**：TTS 需要外部语音包 `gen_line.py`；没有时把 `voice.enabled` 改为 `false`，只放视频原声也能正常直播。
- **config.yaml 改哪些**：`live.room_id`（自己的直播间号，0 = 模拟）、`live.persona_file`、`voice.*`、`bili.keywords/interests/min_score`、`llm` 段。
- **费用**：token 花费与配置的模型直接相关，建议先在模拟模式跑通再开直播。

## 声明

本项目仅用于学习与个人直播辅助。请遵守 B 站平台规则；使用产生的账号与内容风险由使用者自行承担。
