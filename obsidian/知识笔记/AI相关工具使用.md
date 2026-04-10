## OpenCode

全局配置目录：`~/.config/opencode/`

### opencode配置文件

全局配置文件目录：~/.config/opencode/opencode.json

### SKILL

全局skill配置目录：`~/.config/opencode/skills/{skill_name}/SKILL.md`

skill模板：

注意md文件中的name必须和skill目录下的目录名字对应。

opencode通过description来决定是否使用这个skill

```markdown
---
name: git-release
description: Create consistent releases and changelogs
license: MIT
compatibility: opencode
metadata:
  audience: maintainers
  workflow: github
---
## 我的功能
*   根据已合并的 Pull Request 自动起草发布说明。
*   根据语义化版本规范提议版本号变更。
*   生成可直接复制的 `gh release create` 命令。
## 何时使用我
在准备进行带 Git 标签的正式发布时使用我。如果目标版本控制方案不明确，我会主动询问以进行澄清。
```

## AI基础知识

* 机器学习：仅使用CPU即可进行，数据量小
  
  * 深度学习：机器学习的子集，需要大量数据，无需人工对数据进行标注，能根据数据自行提取特征归纳总结，计算量大，需要GPU

监督学习：数据带标签，根据数据以及标签来学习出一个模型。当新数据接入时，模型能预测出对应的结果

无监督学习：处理没有任何标签的数据。算法的目标是发现数据内部隐藏的结构和模式来进行特征分类

卷积神经网络：计算机视觉、CNN

循环神经网络：计算机语音和自然语言

## Prompt
1. 立角色（让AI去扮演一个什么样的角色）
2. 述问题（描述问题背景、实际情况）
3. 定目标（希望ai为我做什么）
4. 补要求（有什么需要ai特别注意）
