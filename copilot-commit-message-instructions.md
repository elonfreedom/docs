<!--
 * @Author: elonfreedom elonfreedom@qq.com
 * @Date: 2025-10-23 14:15:41
 * @LastEditors: elonfreedom elonfreedom@qq.com
 * @LastEditTime: 2025-10-23 15:25:21
 * @FilePath: /docs/copilot-commit-message-instructions.md
 * @Description: 
 * 
 * Copyright (c) 2025 by ${git_name_email}, All Rights Reserved. 
-->
# 提交消息生成指令

请严格遵循以下格式生成提交消息：

`<类型>(<范围>): <主题>`

---

## 1. 类型（type）

常用类型包括：

- **feat**: 新功能（feature）
- **fix**: 错误修复（bug fix）
- **docs**: 文档变更（仅文档内容修改）
- **style**: 代码格式变更（不影响功能，如空格、缩进、分号等）
- **refactor**: 代码重构（既不是新增功能，也不是修复 bug）
- **test**: 测试相关（新增、修改测试用例）
- **chore**: 构建流程或辅助工具变更（不影响源代码和测试）

---

## 2. 范围（scope）

- 用于说明 commit 影响的模块或文件夹，如：user、login、api、profile 等。
- 可选，但建议填写，便于追踪。

---

## 3. 主题（subject）

- 简明扼要描述本次提交的主要内容。
- 建议不超过 50 个字符。
- 使用中文，首字母小写，结尾不加标点。
- header注释的更新时间修改不需要统计。
---

## 4. 示例

```
feat(user): 新增用户实名认证功能
fix(api): 修复接口请求参数错误
docs(readme): 完善项目启动说明
style(profile): 优化个人中心页面样式
refactor(order): 重构订单数据结构
test(login): 增加登录模块单元测试
chore(ci): 更新CI构建脚本
```

---

## 5. 其他建议

- 如需补充详细说明，可在主题下方空一行后添加正文内容。
- 多人协作时请统一格式，便于自动化工具解析和版本管理。

---