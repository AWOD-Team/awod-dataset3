# awod-dataset3 — 服务器实验备份（仅 Release）

本仓库**不再存放文件内容**，只作为 [v1-backup Release](https://github.com/AWOD-Team/awod-dataset3/releases/tag/v1-backup) 的挂载点。

## 为什么清空

2026-11-21 起，本仓库原有的 `OTS_selected/`（5949 张合成雾）已移除——OTS 合成雾不进入最终数据集，判定依据见下。同时 `backup_tars/` 里的 5 个 tar 也全部移出 git。

**但所有 tar 都保留在 Release 里，一个没少。**

## 下载位置

到 [Releases → v1-backup](https://github.com/AWOD-Team/awod-dataset3/releases/tag/v1-backup) 下载：

| 附件 | 大小 | 内容 |
| --- | --- | --- |
| `A_night_v2.tar` | 672 MB | 夜间主集 2780 图 + 2780 标签 + 五折清单 |
| `B_fog_v1.tar` | 537 MB | 雾天主集 2681 图 + 2681 标签 + 五折清单（SHA256SUMS 5362/5362 通过） |
| `C_exp_fog.tar` | 118 MB | 雾天五折 5×best.pt + 5×last.pt + results.csv + 曲线 |
| `D_exp_night.tar` | 103 MB | 夜间五折 5×best.pt + 5×last.pt + test json |
| `E_processed_meta.tar` | 7 MB | 各清洗集标签与筛选台账（rtts 2419 / nod 2669 / night 111） |

## 为什么这些不可再生

服务器 `/data` 已于 **2026-11-06 到期**，上面内容无第二份：

- **训练权重**：C/D 包里 11 个 `.pt`（两次五折 baseline，重训需 GPU 时间）
- **五折切分结论**：哪些图在哪一折，审计过的结论，重新随机划分会让历史指标不可比
- **清洗台账**：哪些图被剔除、为什么，逐图记录
- **筛选后的数据集本体**：雾天 2681 张、夜间 2780 张

## 恢复方式

```bash
tar -xf B_fog_v1.tar          # → processed/fog_2cls_clean_v1/
tar -xf A_night_v2.tar        # → processed/night_2cls_clean_v2/
tar -xf C_exp_fog.tar         # → exp_fog5fold_v2/
tar -xf D_exp_night.tar       # → exp_night5fold_v2/
tar -xf E_processed_meta.tar  # → processed/ 下各清洗集台账
```

`data.yaml` 与各折 `*.txt` 里是服务器绝对路径 `/data/awod_data/...`，迁到新机器需批量替换。

## 关于 OTS 的判定

RESIDE-OTS 合成雾（`<源图id>_<A>_<beta>.jpg`，5949 源图 × 10 档参数）**不进入最终数据集**，原因：

1. 早期项目曾把 417 张（A=0.95, β=0.2）当作真实雾使用，后经 md5 逐字节比对，确认是 OTS 合成雾，已全部剔除。
2. 合成雾→真实雾存在明显域偏差：实验中「仅用合成雾训练」比「真实雾训练」低约 0.19 mAP50。
3. 结论是合成雾只能当训练增强，当前配方下混合训练也无正收益（混合 vs 基线差异在五折标准差内）。

历史记录仍保留在主仓库：`reports/ots417_pairs.csv`、`reports/data_audit.json` 的 `ots417` 段。

## 相关仓库

| 仓库 | 内容 |
| --- | --- |
| [awod-dataset1](https://github.com/AWOD-Team/awod-dataset1) | RTTS 真实雾天全量（4322 图 + VOC 标注） |
| [awod-dataset2](https://github.com/AWOD-Team/awod-dataset2) | DAWN / ExDark / 早期 417 |
| [awod-dataset3](https://github.com/AWOD-Team/awod-dataset3) | 本仓库（仅 Release 挂载点） |
| [AWOD-Adverse-Weather-Object-Detection](https://github.com/AWOD-Team/AWOD-Adverse-Weather-Object-Detection) | 代码与实验报告 |
