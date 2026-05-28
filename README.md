# Frontend AI Agent Rules - Free

English version: [README.en.md](README.en.md)

Bộ rule miễn phí cho **Claude Code, Codex, Cursor, Gemini, Windsurf, Continue và GitHub Copilot** khi làm việc trong frontend codebase React, Next.js, Vite và TypeScript.

Mục tiêu: giúp AI coding agent sửa code đúng scope, tôn trọng project patterns, không tự chạy command nặng, không thêm dependency bừa và giảm noise trong pull request.

## GitHub repo setup

Nếu public thư mục này lên GitHub, dùng cấu hình sau để dễ được tìm thấy hơn:

```txt
Repository name:
frontend-ai-agent-rules

Description:
Practical frontend AI agent rules for Claude Code, Codex, Cursor, Gemini, Windsurf and GitHub Copilot.

Topics:
ai-agent, ai-coding, claude-code, codex, cursor, gemini, windsurf, copilot, frontend, react, nextjs, typescript, rules
```

## Keywords

Frontend AI agent rules, Claude Code rules, Codex rules, Cursor rules, Gemini coding rules, Windsurf rules, GitHub Copilot instructions, AI coding guidelines, React AI rules, Next.js AI rules, TypeScript frontend rules.

## Bao gồm

- `CLAUDE.md` - guideline chính cho AI agent.
- `RULE.md` - rule chi tiết về command, scope, architecture và code quality.

## Cách dùng nhanh

Copy 2 file này vào project frontend:

```txt
project-root/
  CLAUDE.md
  docs/
    RULE.md
```

Nếu dùng Claude Code hoặc Codex, đặt `CLAUDE.md` ở project root. Nếu dùng Cursor, có thể copy nội dung `RULE.md` vào `.cursorrules`.

## Dùng khi nào?

- Bạn dùng Claude/Codex/Cursor để sửa frontend code hằng ngày.
- AI hay sửa lan sang file không liên quan.
- AI hay tự refactor hoặc thêm dependency khi chưa được duyệt.
- Team muốn chuẩn hóa cách AI agent làm việc trước khi mở PR.
- Project có React, Next.js, Vite, TypeScript hoặc monorepo frontend.

## Bản Pro có gì thêm?

- Template cho Cursor, Gemini, Windsurf, Continue, GitHub Copilot.
- Setup guide đa agent.
- Git flow và release process.
- Checklist trước merge và checklist hoàn thành task bằng AI.
- Example cho Next.js, Vite và monorepo.
- Prompt customize rule theo project thật.
- Prompt review rule AI hiện có.

Gợi ý: dùng bản Free cho cá nhân, dùng bản Pro nếu muốn setup chuẩn cho nhiều project hoặc team.
