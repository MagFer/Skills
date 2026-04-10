---
name: marketing-agent
description: Generate marketing content for the Impulse app — video scripts, social media copy, App Store descriptions, and press kit materials. Use when asked to write promotional content, plan video shoots, draft social posts, or update App Store metadata.
---

# Marketing Agent

## Overview
Create marketing content for the Impulse (Net Worth) app by pulling context from the codebase, design assets, and changelog to produce on-brand, accurate promotional materials.

## Workflow

### 1) Gather context
- Check recent changes: `git log --oneline -20` in the iOS repo for new features.
- Read the marketing repo's `CLAUDE.md` for brand guidelines and tone.
- Review existing marketing materials in `/Users/{user}/Projects/Marketing/net-worth-marketing/` for consistency.
- Check App Store screenshots in `/Users/{user}/Projects/Design/Pencil/net-worth-design/AppStore/`.

### 2) Identify key selling points
- Map features to user benefits (e.g., "batch refresh" → "real-time portfolio updates").
- Prioritize what's new or unique vs. competitors.
- Focus on emotional hooks: control, clarity, confidence about finances.

### 3) Generate content by type

#### Video Scripts
- Open with a hook (problem or aspiration) in the first 3 seconds.
- Show the app solving the problem — reference real screens.
- End with a clear CTA (download, try free, etc.).
- Include shot-by-shot breakdowns with timing.
- Save to `videos/scripts/` with descriptive filenames.

#### Social Media Posts
- Platform-appropriate length and tone.
- Twitter/X: concise, punchy, 1-2 sentences + CTA.
- Instagram: visual-first, caption supports the image.
- LinkedIn: professional angle — financial literacy, personal finance management.
- Save to `social/` grouped by campaign or date.

#### App Store Description
- Lead with the primary value proposition.
- Use bullet points for feature highlights.
- Include social proof / stats if available.
- Respect App Store character limits (subtitle: 30 chars, description: 4000 chars).
- Save to `app-store/`.

#### Press Kit
- One-paragraph app summary.
- Feature bullet list.
- Founder/team background.
- Media contact info.
- Save to `press-kit/`.

### 4) Review checklist
- [ ] Accurate — every feature mentioned actually exists in the app.
- [ ] On-brand — matches tone guidelines (clear, confident, approachable).
- [ ] No jargon — "track everything in one place" not "multi-asset portfolio aggregator".
- [ ] CTA present — every piece ends with a next step for the reader/viewer.
- [ ] Localization-ready — avoid idioms that don't translate well.

## Brand Quick Reference
- **Name**: Impulse
- **Tagline**: Track your net worth in one place
- **Key features**: Stocks, crypto, ETFs, real estate, loans, manual entries, multi-currency, privacy-first
- **Tone**: Clear, confident, approachable
- **Supported languages**: English, Spanish, German, French, Italian, Catalan

## Resources
- `references/video-scripting-guide.md`: Structure, pacing, and best practices for app promo videos.
