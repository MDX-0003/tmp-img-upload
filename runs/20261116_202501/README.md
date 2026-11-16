# temple_orbit_360_half_hold / temple_hold_video / 20261116_202501

[▶ 回放视频（30 fps 封装，29.5 秒）](https://github.com/MDX-0003/tmp-img-upload/blob/media/runs/20261116_202501/replay.mp4)

## 这次 run 是什么

- 用例 `temple_hold_video`，档位 `visual`，轨迹 `temple_orbit_360_half_hold`；屏幕百分比 100%，渲染 1782x775。
- 采样 2040 帧（warmup 20 帧丢弃），整段屏幕录制 30 fps。
- 轨迹含 4 段 5 秒停顿（四个机位），用于观察停手后的收敛。
- PICOSplat `e1d96e2`，Docs `613478d`。

## 时间与帧率

- 时间与帧率取自同 CI 的 clean run `20261116_202410`（无截图、无录像）：2040 个采样帧共 **30.90 s**，
  `FrameTimeMs` p50 **12.4 ms** / p95 32.4 ms，即 p50 **81 fps**（均值 68 fps）。
- 截图/录像那次 run 自身墙钟 29.51 s。
  与同轨迹、不录像的 run（`20261116_202410`，p50 12.4 ms）相比，录像把 p50 抬高了 **+0.2 ms**；这个开销已计入上面的实际数字，视频只用于看画面，不用于性能结论。

## 覆盖：想要的 splat 实际画了多少

- 覆盖数字取自不录像的 `20261116_202410`（同轨迹、同帧号），采样 2040 帧。
- 全帧平均 `DrawnSplats` **26.0M** / `WantedSplats` **26.6M** = **99.1%**。
- 每帧平均 **26** 个前沿节点未驻留（p95 240，前沿规模均值 504）。
- `UncoveredFrontierNodes` 最大 **0**：没有整块区域连粗层都缺；画面上看到的黑色斑点是黑色背景从欠采样的 splat 缝里透出来。

## 停手收敛（每段停顿后 1–5 秒）

| 停顿 | 停手时刻 | t=0 | t=1s | t=2s | t=3s | t=4s | t=5s |
| ---: | ---: | --- | --- | --- | --- | --- | --- |
| 1 | +0.69s | 41% / miss 457 | 73% / miss 301 | 91% / miss 192 | 103% / miss 79 | 102% / miss 34 | 101% / miss 0 |
| 2 | +11.19s | 105% / miss 150 | 103% / miss 0 | 100% / miss 0 | 100% / miss 0 | 100% / miss 0 | 100% / miss 0 |
| 3 | +16.90s | 76% / miss 296 | 101% / miss 87 | 98% / miss 25 | 100% / miss 0 | 100% / miss 0 | 100% / miss 0 |
| 4 | +25.81s | 88% / miss 202 | 100% / miss 0 | 100% / miss 0 | 100% / miss 0 | 100% / miss 0 | 100% / miss 0 |

每格是“画出的 splat 占想要的比例 / 未驻留前沿节点数”。收敛看这两列在停手后是否停止变化；`UncoveredFrontierNodes` 全程为 0 表示没有裸洞，只有密度差。

## 文件

- `replay.mp4`：整段屏幕录制，播放速度是录像那次 run 的真实墙钟。桌面抓屏在编辑器负载下约 26 个唯一帧/秒，按 30 fps CFR 封装；判闪烁时按这个上限看。性能数字仍看上面的 clean/收敛 run。
- `frames.csv`：逐帧数据（时刻、帧时间、帧率、drawn/wanted、未驻留、uncovered）。
