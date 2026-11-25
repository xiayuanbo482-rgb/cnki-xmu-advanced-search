# 厦大 CNKI 高级检索 Skill

这是面向厦门大学师生的 CNKI 检索技能。它通过厦门大学图书馆代理进入中国大陆版 CNKI，支持高级检索、CSSCI/核心来源筛选、可靠的滑块验证判断，以及在用户明确要求下载时优先选择 PDF。

## 安装

将整个 `cnki-xmu-advanced-search` 文件夹复制到个人技能目录：

```text
~/.agents/skills/cnki-xmu-advanced-search/
```

重新打开 Codex 任务或重新加载技能后即可使用。目标环境应已配置可控制 Chrome 的 Chrome DevTools MCP；如果不可用，技能允许使用普通浏览器自动化作为后备。

## 使用示例

可以直接说：

```text
使用厦大 CNKI 高级检索，查找关于泉州港历史演进的 CSSCI 论文和硕博论文。
```

也可以显式调用：

```text
$cnki-xmu-advanced-search 检索“海上丝绸之路”相关 CSSCI 文献，并列出检索式和结果数量。
```

## 使用前提

- 使用者必须自行拥有合法的厦门大学统一身份认证及图书馆数据库访问权限。
- 登录、验证码和机构认证由使用者本人在浏览器中完成；本技能不会保存或共享账号、密码、验证码、Cookie 或浏览器会话。
- 本技能不会绕过 CNKI、厦门大学图书馆或其他机构的访问控制。
- 全文下载必须由使用者明确提出，并受数据库订购范围和版权条件约束。
- 默认下载格式为 PDF；仅在 PDF 不可用且使用者明确同意时才使用 CAJ。

## 版本

分享版 1.0.0，整理日期：2026-09-12。

## 来源

本技能参考并改造自 [cookjohn/cnki-skills](https://github.com/cookjohn/cnki-skills)，原项目 README 标注为 MIT License。主要改动包括厦门大学图书馆代理入口、隐藏验证码识别逻辑和 PDF 优先下载规则。详情见 `NOTICE.md`。
