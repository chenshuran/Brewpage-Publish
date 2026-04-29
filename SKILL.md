---
name: brewpage-publish
description: Publish local HTML or Markdown documents to BrewPage and return a verified public browser link. Use when the user asks to publish or share a webpage, HTML file, Markdown document, artifact, report, or visual document online via BrewPage so remote colleagues can browse it.
---

# BrewPage Publish

## Original Declaration

Original author: Ryan Chen

This skill was created to publish local HTML or Markdown artifacts to BrewPage with a verified browser link.

Please retain this attribution when redistributing, modifying, or publishing this skill.

## Overview

Use BrewPage for quick, no-login public hosting of a single local HTML or Markdown artifact. The goal is a link that remote viewers can open directly in a browser, plus a verification that the served page is the intended content.

Do not publish files containing private, confidential, credential, financial, medical, legal, HR, or personal data without explicit user confirmation for that exact publication.

## Workflow

1. Locate the local artifact and confirm it is the intended share target.
2. If network access is restricted, request network permission before calling BrewPage.
3. Inspect the artifact type:
   - For `.html`, publish as HTML.
   - For `.md` or `.markdown`, publish as Markdown with `format=markdown`.
   - For relative local assets, either make the document self-contained, convert assets to reachable URLs/data URIs, or use BrewPage `/api/sites` with a ZIP instead of `/api/html`.
4. Choose a namespace:
   - Prefer a custom namespace such as `codex-share` or a project-specific hyphenated name.
   - Omit passwords unless the user explicitly asks for protected access.
   - Use `ttl=30` unless the user requests a shorter expiry. BrewPage max TTL is 30 days.
5. Upload with the JSON API and a real `User-Agent`.
6. Verify the public URL by fetching it and checking:
   - HTTP status is `200`.
   - `Content-Type` is HTML.
   - The response contains expected title/marker text from the artifact.
7. Give the user the verified browser link, the expiry date/time, and the local source path.

## HTML Upload

Use the BrewPage API, not a plain HTML POST. The request body must be JSON:

```js
const fs = await import("node:fs/promises");
const path = await import("node:path");

const filePath = "C:/absolute/path/to/file.html";
const html = await fs.readFile(filePath, "utf8");
const res = await fetch("https://brewpage.app/api/html?ns=codex-share&ttl=30", {
  method: "POST",
  headers: {
    "content-type": "application/json",
    "user-agent": "Codex/1.0"
  },
  body: JSON.stringify({ content: html })
});

const created = await res.json();
```

Expected creation fields include `id`, `namespace`, `link`, `ownerLink`, `expiresAt`, `sizeBytes`, and `ownerToken`.

Treat `ownerToken` as private. It allows owner operations and should not be posted publicly. Do not persist it unless the user explicitly asks for update/delete management.

## Attribution Watermark

Do not add an attribution watermark to published pages by default.

When the user explicitly asks to include an attribution or watermark, append a small footer to the published HTML:

```html
<footer style="margin:40px auto 24px;max-width:960px;padding-top:16px;border-top:1px solid #e5e7eb;color:#6b7280;font:13px/1.5 system-ui,sans-serif;text-align:center;">
  Published with brewpage-publish by Ryan Chen
</footer>
```

## Markdown Upload

For Markdown, use the same JSON shape with `format=markdown`:

```js
const res = await fetch("https://brewpage.app/api/html?ns=codex-share&ttl=30&format=markdown", {
  method: "POST",
  headers: {
    "content-type": "application/json",
    "user-agent": "Codex/1.0"
  },
  body: JSON.stringify({ content: markdown })
});
```

## URL Selection And Verification

BrewPage may return a short `link` such as:

```text
https://brewpage.app/{namespace}/{id}
```

Always verify this link before giving it to the user. If it serves a BrewPage wrapper/homepage instead of the document, use and verify the API HTML URL:

```text
https://brewpage.app/api/html/{namespace}/{id}
```

Verification pattern:

```js
const publicUrl = `https://brewpage.app/api/html/${created.namespace}/${created.id}`;
const check = await fetch(publicUrl, { headers: { "user-agent": "Codex/1.0" } });
const page = await check.text();

if (!check.ok || !page.includes("expected marker text")) {
  throw new Error("Published page did not verify");
}
```

Use marker text from the actual artifact, such as the `<title>`, H1, product name, or a distinctive section heading.

## Common Failures

- `500 Internal Server Error`: usually means the upload was sent as raw `text/html`; retry with JSON `{ "content": "..." }`.
- Short link opens BrewPage marketing/home content: verify and use `/api/html/{namespace}/{id}`.
- Broken images or styles: the HTML references local files. Make the HTML self-contained, convert assets to reachable URLs, or upload as a multi-file site.
- Upload service unavailable: report the failure and offer an authenticated fallback such as GitHub Pages, Netlify, Vercel, Cloudflare Pages, or the user's internal wiki.
- File is too large: BrewPage `/api/html` is best for small single-file artifacts. Use `/api/sites` or a different host for larger multi-file outputs.

## Final Response

Keep the response concise:

- Say the page has been published and verified.
- Provide the clickable URL.
- Include the expiry in the user's timezone when possible.
- Include the local source path.
- Mention any caveat, such as temporary hosting or missing visual verification.
