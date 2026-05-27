# CAD 双方案验证 - 统一数据模型

## TextItem（文字标注）

| 字段     | 类型           | 说明           |
| -------- | -------------- | -------------- |
| content  | str            | 文字内容       |
| position | (float, float) | 插入点坐标     |
| layer    | str            | 所在图层       |
| height   | float          | 文字高度       |
| source   | str            | 来源标注 (A/B) |

## PipeItem（管道段）

| 字段        | 类型           | 说明           |
| ----------- | -------------- | -------------- |
| start       | (float, float) | 起点坐标       |
| end         | (float, float) | 终点坐标       |
| length      | float          | 计算长度       |
| layer       | str            | 所在图层       |
| diameter    | str (optional) | 管径标注       |
| system_type | str (optional) | 系统类型       |
| source      | str            | 来源标注 (A/B) |

## EquipmentItem（设备/图块）

| 字段     | 类型           | 说明           |
| -------- | -------------- | -------------- |
| name     | str            | 图块名称       |
| position | (float, float) | 插入点坐标     |
| layer    | str            | 所在图层       |
| category | str (optional) | 分类           |
| count    | int            | 数量           |
| source   | str            | 来源标注 (A/B) |

## DimensionItem（尺寸标注）

| 字段     | 类型           | 说明                   |
| -------- | -------------- | ---------------------- |
| value    | float          | 标注值                 |
| position | (float, float) | 标注位置               |
| type     | str            | linear/aligned/angular |
| source   | str            | 来源标注 (A/B)         |

## ExtractionResult（提取结果汇总）

| 字段       | 类型                | 说明               |
| ---------- | ------------------- | ------------------ |
| file       | str                 | 源文件路径         |
| texts      | list[TextItem]      | 文字列表           |
| pipes      | list[PipeItem]      | 管道列表           |
| equipment  | list[EquipmentItem] | 设备列表           |
| dimensions | list[DimensionItem] | 尺寸列表           |
| approach   | str                 | "ezdxf" / "vision" |

## 交叉验证结果

| 字段         | 说明                   |
| ------------ | ---------------------- |
| matched      | 两方案完全一致的项目   |
| a_only       | 仅方案 A (ezdxf) 识别  |
| b_only       | 仅方案 B (vision) 识别 |
| conflicts    | 两方案都识别但值冲突   |
| review_items | 需要人工复核的项目     |
