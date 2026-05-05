# Windows 打包指南

本文档演示如何在 **Windows 10/11** 上使用 PyInstaller 将 `video2ppt.py` 打包成单文件 `video2ppt.exe`，无需目标用户额外安装 Python 与依赖。

## 1. 准备环境
1. 安装 **Python 3.10+ (64-bit)** 并勾选"Add Python to PATH"。
2. 打开 **PowerShell** 或 **CMD**，创建并进入项目目录（含 `video2ppt.py`、`video2ppt.spec`）。
3. 安装打包工具及依赖：
   ```powershell
   pip install pyinstaller moviepy imagehash pillow python-pptx tqdm
   ```
4. 下载 [ffmpeg static build](https://www.gyan.dev/ffmpeg/builds/) 对应版本，将 `ffmpeg.exe` 复制到项目根目录（与 `video2ppt.spec` 同级）。

## 2. 生成可执行文件
执行：
```powershell
pyinstaller --clean --noconfirm video2ppt.spec
```
完成后，在 `dist\video2ppt\` 目录会得到：
```
video2ppt.exe
ffmpeg.exe
```
可将整个 `video2ppt` 文件夹打包压缩或重命名后分发。

### 2.1 生成单文件（可选）
若希望 **完全单文件**，无需额外 `ffmpeg.exe`，可尝试：
```powershell
pyinstaller -F video2ppt.py --add-binary "ffmpeg.exe;." --clean --name video2ppt
```
> 注意：单文件模式解压到临时目录运行，首次启动稍慢。

## 3. 目标机运行方式
将产物复制到任何 Win 机器，双击 **video2ppt.exe** 或在命令行：
```cmd
video2ppt.exe -i D:\Videos\demo.mp4 --interval 2 --threshold 2
```
脚本会在同级目录生成 `demo_output` 文件夹。

## 4. 常见问题
| 现象 | 可能原因 |
|------|----------|
| 启动提示缺少 DLL | 目标机缺 VC++ 运行库，安装微软官方 *VC_redist.x64.exe* |
| MoviePy 报找不到 ffmpeg | 请确认 `ffmpeg.exe` 与 `video2ppt.exe` 同目录，或手动将路径加入系统 `PATH` |

## 5. 更新 spec 或依赖
若修改脚本或新增依赖，只需重新执行 **PyInstaller** 命令即可。 