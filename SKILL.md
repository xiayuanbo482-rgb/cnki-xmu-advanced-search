---
name: cnki-xmu-advanced-search
description: Search CNKI through the Xiamen University Library proxy, especially for off-campus access, with advanced fields, CSSCI/core-source filters, reliable CAPTCHA detection, and PDF-first authorized downloads. Use when the user identifies Xiamen University, XMU, or the 厦大图书馆 CNKI proxy; use the general CNKI skill otherwise.
---

# 厦大 CNKI 高级检索

Use browser automation to search mainland CNKI through the user's Xiamen University Library access. Prefer the configured Chrome DevTools MCP server so the persistent Chrome profile can retain the user's proxy and CNKI sessions. Fall back to normal browser automation only when that server is unavailable, unless the user explicitly required Chrome DevTools MCP.

## Access through Xiamen University

1. Inspect the existing Chrome tabs before opening another CNKI session. Reuse a tab that is already authenticated through the XMU proxy when practical.
2. When a new session is needed, start from the XMU Library gateway:
   `https://libproxy.xmu.edu.cn/go?url=http://www.cnki.net/`
3. Outside mainland-network access, do not begin with a direct `cnki.net` URL because it may redirect to CNKI Overseas, where the user's XMU institutional login is unavailable.
4. Let the proxy rewrite subsequent CNKI URLs. Do not construct or persist a `yit.libproxy.xmu.edu.cn` session URL manually; its exact form may change.
5. If authentication is required, bring the browser forward and ask the user to sign in. Never request, record, or reuse the user's password, verification code, or CAPTCHA response.
6. Treat a visible “厦门大学” institutional indicator as session evidence, not as permission to download. Full-text downloads still require an explicit user request.

## Build and run the search

- Use CNKI's advanced-search interface inside the proxied mainland session. Re-check the live page structure after each navigation because labels and element identifiers can change.
- Translate the request into only the fields that improve precision: 主题 by default, or 篇名、关键词、摘要、篇关摘、作者、来源 and publication-year range when justified.
- Preserve the user's literal query. When semantic expansion is useful, run it as a clearly identified additional search using relevant synonyms, historical event names, institutions, people, transliterations, or broader concepts. Do not silently replace the exact search with the expansion.
- For CSSCI、北大核心、CSCD、SCI or EI filtering, clear “全部期刊” when required by the page before selecting the requested categories. Multiple requested categories normally use OR logic unless the interface states otherwise.
- Prefer a bounded `evaluate_script` operation for structured form filling and extraction when it is more reliable than many individual clicks. Confirm active fields and source filters from the resulting page rather than assuming the clicks succeeded.
- Distinguish the total hit count from records actually inspected or extracted. For a large set, present a focused first-pass list and ask before paging through many results unless the user's request already authorizes comprehensive collection.

## Decide whether a slider CAPTCHA is real

CNKI commonly leaves hidden verification markup and the text “拖动下方拼图完成验证” in the DOM. That text alone is not evidence that the user can see a slider.

Treat a slider or other verification challenge as active only when the relevant widget is actually visible or an attempted in-scope action is blocked by it. When possible, check all of the following:

- the widget and its ancestors are not `display: none` or `visibility: hidden`;
- effective opacity is greater than zero;
- its bounding rectangle has non-trivial width and height and intersects the viewport;
- an associated dialog, overlay, or slider control is visually or interactively present;
- the search, paging, opening, or download action failed or is waiting for that verification.

If the page merely contains hidden CAPTCHA text while the requested operation succeeds, ignore the placeholder and continue without mentioning a slider. If a visible challenge genuinely blocks progress, stop and ask the user to complete it in Chrome. Never drag, solve, bypass, or automate the CAPTCHA.

## Download full text

Do not start a full-text download, Zotero export, or other external mutation unless the user explicitly requests it.

When the user requests a download:

1. If no format is specified, default to PDF.
2. Resolve the exact article by title and open its detail page in the authenticated XMU-proxy session.
3. Click the detail page link whose visible label is exactly “PDF下载”. Do not use the result-list button labelled only “下载”, because that ambiguous entry commonly returns CAJ.
4. If the detail page offers no PDF but does offer CAJ, report that limitation and ask whether CAJ is acceptable. Do not silently fall back to CAJ unless the user already authorized any available format or explicitly requested CAJ.
5. For multiple articles, repeat the detail-page PDF selection for each record. Avoid duplicate CAJ/PDF downloads unless the user requested both formats.
6. After downloading, verify the actual file format rather than trusting its extension. For PDF, confirm that it is parseable as PDF and obtain a page count when local tools permit.
7. Move files to the user's requested destination only after checking for same-name files. Do not overwrite an existing file without explicit permission. Preserve CNKI's useful `题名_作者` filename where possible.

If login, a visible CAPTCHA, missing institutional entitlement, or a format-selection page blocks the download, stop at that point and explain exactly what the user needs to do.

## Report the outcome

For search results, state:

- exact and expanded query expressions;
- fields, date range, and active source filters;
- total counts and how many records were inspected;
- a compact bibliography with title, author, source, year, and proxied CNKI link when available;
- the current result-page URL and any genuine login, paging, or verification limitation.

For downloads, list each saved file with its actual format, page count when available, and destination. Treat CSSCI/core labels and citation or download counts as information displayed by CNKI, not as independent verification.
