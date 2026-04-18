## 创建自己的skills
参考[https://www.runoob.com/claude-code/skill-creator-usage.html]
### 安装
```bash
npx skills add https://github.com/anthropics/skills --skill skill-creator
```

```bash
claude install anthropics/skills/skill-creator
```
### 调用
```bash
/skill-creator
```

### 工作流程
想清楚需求
    ↓
起草 SKILL.md
    ↓
设计测试用例
    ↓
运行测试（有 Skill vs 没有 Skill，对比效果）
    ↓
评估结果（看报告 + 打分）
    ↓
根据反馈修改 SKILL.md
    ↓
重复，直到满意
    ↓
打包成 .skill 文件