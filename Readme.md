# WSDCF MP4 Extractor

从 **DMM VR Player** 的 `.wsdcf` DRM 加密文件中提取标准 MP4 视频。

脚本直接调用播放器自带的解密库完成 AES-128-CBC 解密，**不破解密钥、不绕过许可证**：只有在播放器中拥有有效版权、能够正常播放的文件才能被导出，本质上是"把已经买下的内容备份成普通 MP4"。

> ⚠️ **仅供个人备份使用**：请仅对你本人已合法购买 / 已获得授权的内容使用本工具；请勿分发、传播、转售导出结果，并遵守当地版权法规与 DMM 服务条款。

---
## 演示

<video src="https://private-user-images.githubusercontent.com/47425892/679193391-9399fb51-4adb-4f18-acd9-a5e291bc9b7b.mp4?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTI1MTEwNTksIm5iZiI6MTc5MjUxMDc1OSwicGF0aCI6Ii80NzQyNTg5Mi82NzkxOTMzOTEtOTM5OWZiNTEtNGFkYi00ZjE4LWFjZDktYTVlMjkxYmM5YjdiLm1wND9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjEwMjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYxMDIwVDE1MzkxOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWFlYTdmMGRhNWY3NDBlYTUyOTM2NzFhNGIyN2Y5MzY2MzIzNTMyZDBkYjY4NWI3MjhkOWEzMmM2YWM1NDY4ZTgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT12aWRlbyUyRm1wNCJ9.feqf4d28bugqJo9s1plrTairzicxtAtA__RI9pPJd2g" data-canonical-src="https://private-user-images.githubusercontent.com/47425892/679193391-9399fb51-4adb-4f18-acd9-a5e291bc9b7b.mp4?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTI1MTEwNTksIm5iZiI6MTc5MjUxMDc1OSwicGF0aCI6Ii80NzQyNTg5Mi82NzkxOTMzOTEtOTM5OWZiNTEtNGFkYi00ZjE4LWFjZDktYTVlMjkxYmM5YjdiLm1wND9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjEwMjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYxMDIwVDE1MzkxOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWFlYTdmMGRhNWY3NDBlYTUyOTM2NzFhNGIyN2Y5MzY2MzIzNTMyZDBkYjY4NWI3MjhkOWEzMmM2YWM1NDY4ZTgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT12aWRlbyUyRm1wNCJ9.feqf4d28bugqJo9s1plrTairzicxtAtA__RI9pPJd2g" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-height:640px; min-height: 200px" jm_neat="1147788289">
</video> 
## 特性

- **单文件脚本**，无需安装额外组件，无需修改播放器
- **自动检测** DMM VR Player 安装路径，开箱即用
- **复用播放器本地许可证**，导出过程无需联网
- **流式解密**，恒定 4 MB 缓冲区，内存占用与文件体积无关
- **批量处理** + 实时进度 / 速度显示
- 跨盘符无关，Windows x64 原生支持

## 环境要求

| 项目 | 要求 |
|------|------|
| 操作系统 | Windows x64 |
| Python | 3.10+ |
| 依赖 | [pythonnet](https://github.com/pythonnet/pythonnet)（`pip install pythonnet`） |
| 运行时 | .NET Framework 4.7.2+ |
| 其他 | 已安装 DMM VR Player，且目标文件拥有有效许可证（能在播放器中正常播放） |

## 使用
> **首次使用前**：请先在 DMM VR Player 中把目标文件正常播放一次，让播放器缓存许可证。否则解密会返回空数据。
```powershell
# 单个文件，指定输出名
python wsdcf_extract.py video.wsdcf -o video.mp4

# 单个文件，默认输出到同目录同名 .mp4
python wsdcf_extract.py video.wsdcf

# 批量导出到指定目录
python wsdcf_extract.py a.wsdcf b.wsdcf --output-dir ./out

# 播放器装在非标准路径时手动指定
python wsdcf_extract.py video.wsdcf --install-path "C:\Program Files\DMMVRPlayer_Windows"
```

运行时会打印进度，例如：

```text
Initializing WSDCF Extractor...
  Player: C:\Users\<you>\AppData\Local\DMMVRPlayer

Input:  video.wsdcf
Output: video.mp4
  1.20 GB  [104 MB/s]  11s
  2.40 GB  [105 MB/s]  23s
  Done: 3.61 GB in 35.0s (105 MB/s)
```

导出完成后可用任意播放器或工具检查：

```powershell
ffprobe video.mp4
```

### 命令行参数

| 参数 | 说明 |
|------|------|
| `inputs` | 一个或多个 `.wsdcf` 输入文件（必填） |
| `-o, --output` | 输出文件路径，**仅限单输入** |
| `-d, --output-dir` | 输出目录，默认与输入文件同目录 |
| `--install-path` | DMM VR Player 安装路径，默认自动检测 |

## 工作原理

`.wsdcf` 由**明文元数据（Envelope）**和**AES-128-CBC 加密体（Body）**组成。脚本不做任何破解，而是复用播放器自身的解密管道：

```text
python ↔ pythonnet ↔ Assembly-CSharp.dll (Unity 应用层)
                         └─ DrmDecryptContext(wsdcf 路径)
                              └─ GetSession() → DrmDecryptSession
                                   └─ ReadAt(offset, buffer, 0, count)
                                        ├─ WsdTransportSession 读取密文块
                                        └─ ICryptoTransform 执行 AES-128-CBC 解密
```


## 故障排除

| 现象 | 原因 | 处理方式 |
|------|------|----------|
| `DMM VR Player not found` | 未安装或装在非标准路径 | 用 `--install-path` 手动指定 |
| `ReadAt returned no data - invalid license?` | 本地许可证无效 / 未缓存 | 先在播放器中播放该文件一次 |
| `Decrypted data is not MP4 - license issue?` | 许可证与文件不匹配 | 确认文件属于当前账号，重新播放后再试 |
| `ERROR: pythonnet is required but not installed` | 缺少依赖 | `pip install pythonnet`，conda 环境需先激活 |
| `DllNotFoundException: wsdenvlp.dll` | 原生 DLL 未加载 | 检查 `--install-path` 是否指向完整安装目录 |
| 导出文件不完整 | 磁盘空间不足 | 预留与源文件相近的可用空间 |


## 免责声明

本项目仅用于**个人备份已合法获得的内容**。使用者需自行确保其行为符合当地法律法规及 DMM 的服务条款；作者不对任何滥用行为或由此产生的后果负责。
