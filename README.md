# brewpage-publish

中文 | [English](#english)

## 简介

`brewpage-publish` 是一个 Codex skill，用于把本地 HTML 或 Markdown 文档快速发布到 BrewPage，并返回一个已经验证可访问的公开浏览链接。

适合分享临时报告、演示文档、设计稿、单页 HTML 文档、Markdown 说明等内容给异地同事浏览。

## 作者声明

Original author: Ryan Chen

请在转载、修改、分发或发布本 skill 时保留作者署名。

## 功能

- 发布本地 `.html` 文件到 BrewPage。
- 发布本地 `.md` / `.markdown` 文件到 BrewPage。
- 自动使用 BrewPage JSON API，而不是容易失败的原始 HTML 直传。
- 校验发布后的链接是否真实返回目标页面。
- 在短链接异常时，使用可验证的 `/api/html/{namespace}/{id}` 页面链接。
- 默认为发布页面追加轻量署名水印，用户明确要求时可不加。

## 安装

把整个 `brewpage-publish` 文件夹放到 Codex 的 skills 目录。

Windows:

```text
C:\Users\你的用户名\.codex\skills\brewpage-publish
```

macOS / Linux:

```text
~/.codex/skills/brewpage-publish
```

如果你设置了 `CODEX_HOME`，则放到：

```text
$CODEX_HOME/skills/brewpage-publish
```

建议放好后重启 Codex，或新开一个 Codex 会话。

## 使用方式

在 Codex 中可以这样说：

```text
用 $brewpage-publish 发布这个 HTML 文件，生成可浏览链接。
```

或者：

```text
把 C:\path\to\report.html 发布到 BrewPage，给我一个同事可以浏览的链接。
```

Markdown 也可以：

```text
用 $brewpage-publish 把这个 Markdown 文档发布到 BrewPage。
```

发布页面默认会带作者水印。如果希望不带水印，可以明确说明：

```text
发布到 BrewPage，这次不要加水印。
```

## 注意事项

- BrewPage 适合临时分享，不适合长期正式托管。
- BrewPage 页面会过期，TTL 通常最多为 30 天。
- 不要发布包含公司机密、个人信息、客户数据、密钥、财务、医疗、法律或 HR 信息的内容，除非已经明确确认允许公开分享。
- BrewPage 返回的 `ownerToken` 可用于管理已发布内容，不要公开分享。
- 如果 HTML 引用了本地图片、CSS 或 JS，需要先改成自包含文件、公开 URL，或改用多文件站点上传方式。

## 项目结构

```text
brewpage-publish/
  SKILL.md
  README.md
  agents/
    openai.yaml
```

## 许可建议

推荐按 `CC BY 4.0` 或其他需要保留署名的许可方式分发。

---

## English

## Overview

`brewpage-publish` is a Codex skill for publishing local HTML or Markdown documents to BrewPage and returning a verified public browser link.

It is useful for sharing temporary reports, presentation documents, design artifacts, single-page HTML files, and Markdown notes with remote colleagues.

## Original Declaration

Original author: Ryan Chen

Please retain this attribution when redistributing, modifying, or publishing this skill.

## Features

- Publish local `.html` files to BrewPage.
- Publish local `.md` / `.markdown` files to BrewPage.
- Use the BrewPage JSON API instead of fragile raw HTML uploads.
- Verify that the published URL returns the intended page.
- Fall back to the verifiable `/api/html/{namespace}/{id}` URL when the short link does not serve the document directly.
- Add a lightweight attribution watermark by default, unless the user explicitly asks to omit it.

## Installation

Place the entire `brewpage-publish` folder in your Codex skills directory.

Windows:

```text
C:\Users\YourUserName\.codex\skills\brewpage-publish
```

macOS / Linux:

```text
~/.codex/skills/brewpage-publish
```

If `CODEX_HOME` is configured, use:

```text
$CODEX_HOME/skills/brewpage-publish
```

After installation, restart Codex or open a new Codex session.

## Usage

In Codex, ask:

```text
Use $brewpage-publish to publish this HTML file and give me a browsable link.
```

Or:

```text
Publish C:\path\to\report.html to BrewPage and give me a link my colleagues can open.
```

Markdown is supported too:

```text
Use $brewpage-publish to publish this Markdown document to BrewPage.
```

Published pages include the attribution watermark by default. To omit it, ask explicitly:

```text
Publish this to BrewPage without a watermark.
```

## Notes

- BrewPage is best for temporary sharing, not long-term production hosting.
- BrewPage pages expire; TTL is usually capped at 30 days.
- Do not publish confidential company information, personal data, customer data, secrets, financial data, medical data, legal data, or HR data unless public sharing has been explicitly approved.
- The BrewPage `ownerToken` can manage published content. Do not share it publicly.
- If the HTML references local images, CSS, or JavaScript, make the file self-contained, use public URLs, or publish it as a multi-file site.

## Project Structure

```text
brewpage-publish/
  SKILL.md
  README.md
  agents/
    openai.yaml
```

## License Suggestion

Consider distributing this skill under `CC BY 4.0` or another attribution-required license.
