---
name: cad-dual-validator
description: CAD双方案交叉验证 - 使用ezdxf程序解析+AI视觉识别两种方法提取DXF图纸数据，交叉验证提高准确性。支持仅程序解析、仅AI视觉和完整验证三种模式。同义词：图纸验证、交叉验证、双方案提取、图纸数据提取、AI识图、视觉识别、DXF数据提取、图纸OCR、double extract、cross validation
---

# CAD Dual Validator -- CAD双方案交叉验证

## 概述

双方案提取+交叉验证系统，通过两种独立方法提取 DXF 图纸数据并互相验证：

- **方案 A**: ezdxf 程序化解析（快速、结构化）
- **方案 B**: DXF 渲染为图像 + AI 视觉识别（语义化、类人理解）
- **交叉验证**: 比对两方案结果，标记差异供人工复核

## 项目位置

```
C:\Users\86183\cad-dual-extract\
```

## 模式选择

| 关键词                  | 模式                  | 说明                  |
| ----------------------- | --------------------- | --------------------- |
| 验证/交叉/对比/两个方案 | Full Cross-Validation | 方案A+B，输出差异报告 |
| 解析/提取/程序/ezdxf    | ezdxf-only (A)        | 仅程序解析，快速      |
| 视觉/AI/识别/看图       | Vision-only (B)       | 仅AI视觉提取          |
| 未明确                  | 默认 Full，询问       |                       |

## 模式 A：ezdxf 程序解析

```bash
cd C:\Users\86183\cad-dual-extract
python main.py <file.dxf> --skip-vision
```

输出：Excel 含文本、管道、设备、尺寸标注。
最快模式，适合结构化 DXF 文件。

## 模式 B：AI 视觉识别

```bash
cd C:\Users\86183\cad-dual-extract
python main.py <file.dxf> --vision-only
```

流程：DXF → 图像瓦片渲染 → AI 视觉提取 → Excel 输出。
适合标注密集、非标准图层命名的图纸。

## 模式 C：完整交叉验证

```bash
cd C:\Users\86183\cad-dual-extract
python main.py <file.dxf> --tile-dpi 150 --verbose
```

两方案同时执行，交叉验证器比对结果，生成验证报告 Excel。

## 统一数据模型

| 类型          | 字段                                          |
| ------------- | --------------------------------------------- |
| TextItem      | content, position(x,y), layer, height         |
| PipeItem      | start(x,y), end(x,y), length, layer, diameter |
| EquipmentItem | name, position(x,y), layer, category          |
| DimensionItem | value, position, type(linear/aligned/angular) |

## 验证报告解读

| 状态         | 含义         | 处理                           |
| ------------ | ------------ | ------------------------------ |
| matched      | 两方案一致   | 高置信度，直接使用             |
| A-only       | 仅程序识别到 | 可能正确但视觉未捕捉，人工确认 |
| B-only       | 仅AI识别到   | 可能捕捉了标注式元素，人工确认 |
| review_items | 结果冲突     | 必须人工复核                   |

## 参数调优

| 参数            | 说明                       | 默认值 |
| --------------- | -------------------------- | ------ |
| `--tile-dpi`    | 渲染 DPI（越高越准但越慢） | 150    |
| `--tile-max-px` | 每瓦片最大像素             | 2048   |
| `--skip-vision` | 跳过方案B                  | 关闭   |
| `--vision-only` | 仅方案B                    | 关闭   |
| `--verbose`     | 详细分类统计               | 关闭   |

## 联动

- 验证后的数据可导入 cad-bom-unified 生成工程量清单
- 可与 cad-reader 的结果比对，三方交叉验证
