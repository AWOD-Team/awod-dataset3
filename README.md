# awod-dataset3 — 合成雾 + 不可再生台账与训练权重

AWOD 项目里**最不可再生**的部分都放在这个仓库：合成雾图像、服务器到期前的实验台账、训练权重。

## 内容

| 目录 / 文件 | 内容 | 说明 |
| --- | --- | --- |
| `OTS_selected/` | 5949 张合成雾图（A=0.95, β=0.2） | 单文件小，直接入 git |
| `backup_tars/E_processed_meta.tar` | 各清洗集标签与筛选台账 | 7 MB，入 git |
| `backup_tars/*.tar` | 其余 4 个实验备份 | **通过 Release 下载**，见下 |

## ⚠️ 大文件走 Release

GitHub 单文件硬限制 100 MB，以下 4 个 tar 无法入 git，请到 **Releases** 页面下载：

| 文件 | 大小 | 内容 |
| --- | --- | --- |
| `A_night_v2.tar` | 672 MB | 夜间主集 2780 图 + 2780 标签 + 清单 |
| `B_fog_v1.tar` | 537 MB | 雾天主集 2681 图 + 2681 标签 + 清单 |
| `C_exp_fog.tar` | 118 MB | 雾天五折：5×best.pt + 5×last.pt + results.csv |
| `D_exp_night.tar` | 103 MB | 夜间五折：5×best.pt + 5×last.pt + test json |

下载后放进本目录的 `backup_tars/` 即可还原完整结构。

## 为什么这些不可再生

服务器 `/data` 已于 **2026-11-06 到期**，上面的实验目录全部消失。以下内容没有第二份：

- **训练权重**：C/D 包里 11 个 `.pt`（两次五折 baseline，重训需 GPU 时间）
- **五折切分结论**：哪些图在哪一折，是审计过的结论，重新随机划分会让历史指标不可比
- **清洗台账**：哪些图被剔除、为什么，逐图记录
- **筛选后的数据集本体**：雾天 2681 张、夜间 2780 张的清洗后版本

## 各 tar 的校验状态

| 文件 | 校验 |
| --- | --- |
| `B_fog_v1.tar` | `SHA256SUMS` **5362/5362 全部通过** |
| `A_night_v2.tar` | 图/标 100% 配对，抽检 400 张全部可解码 |
| `C_exp_fog.tar` | 可正常解包，5 折权重齐全 |
| `D_exp_night.tar` | 可正常解包，5 折权重齐全 |
| `E_processed_meta.tar` | rtts 2419 个标签与 RTTS 原图 2419/2419 对得上 |

## 恢复方式

```bash
tar -xf backup_tars/B_fog_v1.tar     # → processed/fog_2cls_clean_v1/
tar -xf backup_tars/A_night_v2.tar   # → processed/night_2cls_clean_v2/
tar -xf backup_tars/C_exp_fog.tar    # → exp_fog5fold_v2/
tar -xf backup_tars/D_exp_night.tar  # → exp_night5fold_v2/
tar -xf backup_tars/E_processed_meta.tar  # → processed/ 下各清洗集台账
```

## OTS_selected

RESIDE-OTS 合成雾，命名规则 `<源图id>_<A>_<beta>.jpg`，全部为 A=0.95、β=0.2 一档，5949 张。

对应的清晰源图在 [awod-dataset2](https://github.com/AWOD-Team/awod-dataset2) 的 `clear/clear_images/`（5949 张，**id 完全一一对应**）。

合成公式为大气散射模型 `I = J·t + A·(1-t)`，`t = exp(-β·d)`。

## 相关仓库

| 仓库 | 内容 |
| --- | --- |
| [awod-dataset1](https://github.com/AWOD-Team/awod-dataset1) | 真实雾天 RTTS |
| [awod-dataset2](https://github.com/AWOD-Team/awod-dataset2) | DAWN / ExDark / OTS 清晰源 / 早期 417 |
| [awod-dataset3](https://github.com/AWOD-Team/awod-dataset3) | 本仓库 |
| [AWOD-Adverse-Weather-Object-Detection](https://github.com/AWOD-Team/AWOD-Adverse-Weather-Object-Detection) | 代码与实验报告 |
