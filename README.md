# AI Skills Collection

这是一个opencode技能集合，包含各种开发任务的专业技能。

## 可用技能

### 1. printer-driver-dev
- **描述**: 打印机驱动开发专家
- **用途**: V3 GDI打印机驱动、PDF输出、字体嵌入
- **触发条件**: 打印机驱动相关任务

### 2. knowledge-manager
- **描述**: 知识管理专家
- **用途**: 联网获取知识、本地保存、结构化总结
- **触发条件**: 需要联网获取信息时

## 目录结构

```
aiskills/
├── README.md                    # 本文件
├── printer-driver-dev/          # 打印机驱动开发技能
│   └── SKILL.md
└── knowledge-manager/           # 知识管理技能
    └── SKILL.md
```

## 如何使用

### 安装技能

1. 克隆此仓库
2. 在 `opencode.json` 中配置技能路径：
   ```json
   {
     "skills": {
       "paths": ["E:/github/aiskills"]
     }
   }
   ```
3. 重启opencode

### 自动触发

技能会在匹配以下条件时自动激活：
- **printer-driver-dev**: 打印机驱动、GDI、V3驱动、PDF打印
- **knowledge-manager**: webfetch、websearch、研究主题

## 技能格式

每个技能包含一个 `SKILL.md` 文件，格式如下：

```markdown
---
name: skill-name
description: 技能描述
---

# 技能标题

## 核心工作流程
...
```

## 更新日志

- **2026-09-09**: 创建技能集合
