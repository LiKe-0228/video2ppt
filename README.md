# video2ppt 使用说明

> **video2ppt.py**：将视频每隔指定秒数抽帧、去重后生成 PPT（16:9 填满裁切）。

---

## 1. 环境准备

| 组件 | 版本建议 | 说明 |
|------|----------|------|
| Python | ≥ 3.8 | 建议使用虚拟环境 |
| ffmpeg | 最新稳定版 | `moviepy` 会自动调用，请确保命令 `ffmpeg` 已加入系统 PATH |

### 1.1 安装 Python 依赖
```bash
pip install moviepy imagehash pillow python-pptx tqdm
```

---

## 2. 快速开始
```bash
python video2ppt.py -i /path/to/video.mp4
```
执行完毕，当前目录将多出一个 `视频名_output/` 文件夹，内含去重后的帧图片及生成的 PPT 文件。

---

## 3. 命令行参数
| 参数 | 缩写 | 默认值 | 作用 |
|------|------|--------|------|
| `--input` | `-i` | — | **必填**，输入视频路径 |
| `--interval` | — | `2` | 抽帧时间间隔（秒） |
| `--threshold` | — | `2` | 感知哈希汉明距离阈值，阈值越小判断越严格 |

### 3.1 示例
抽帧间隔 3 s，去重阈值 3：
```bash
python video2ppt.py -i /videos/meeting.mp4 --interval 3 --threshold 3
```

---

## 4. 输出目录结构
假设输入文件为 `travel.mp4`：
```
travel_output/
├── frames_raw/            # 原始抽帧，全部保留
├── frames_unique/         # 去重后保留的帧
├── travel_unique.pptx     # 生成的 PPT
└── run.log (可选)         # 运行日志
```
> 若您不需保留 `frames_raw/`，可在操作结束后删除。

---

## 5. 去重阈值详解
- **0**：需完全一致，任何轻微差异都被视为不同。
- **1–2**：严格，适合"几乎相同"画面去重（默认 2）。
- **3–5**：中等宽松，小幅度画面变化也会被视为相同。
- **≥6**：很宽松，可能把不同场景也判为同一张。

如需试验不同效果，可调高或调低 `--threshold` 后重新运行。

---

## 6. 常见问题
1. **提示找不到 ffmpeg**  
   请确认系统可以在终端直接执行 `ffmpeg -version`。若无，参考官方文档安装并将其加入 PATH。
2. **PPT 页面尺寸能否调整？**  
   目前脚本固定 16:9，若需其它比例可在 `video2ppt.py` 中自行修改 `prs.slide_width` / `slide_height`。
3. **感知哈希库无法安装**  
   `imagehash` 依赖 `numpy`、`scipy`，确保编译环境完整或使用官方 wheel。

---

## 7. 更新日志
| 版本 | 日期 | 说明 |
|------|------|------|
| v1.0 | 2025-07-04 | 首次发布，实现抽帧、去重、PPT 生成功能 |

---

> 如有问题或改进建议，欢迎反馈！ 