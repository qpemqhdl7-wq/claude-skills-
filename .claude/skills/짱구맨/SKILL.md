---
name: 짱구맨
description: 홈페이지·랜딩페이지·회사 소개 사이트를 처음부터 끝까지 만드는 원스톱 워크플로. ui-ux-pro-max 디자인 데이터로 스타일·색상·폰트를 정하고, Motion 애니메이션을 넣어 페이지를 구현한 뒤 모바일/데스크톱 스크린샷으로 검수까지 한다. Use when the user asks to build, make, redesign, or improve a homepage, landing page, website, 홈페이지, 랜딩페이지, 웹사이트, or 사이트.
argument-hint: "[무슨 사이트인지 한 줄 설명, 예: 강남 필라테스 스튜디오 홈페이지]"
---

# Homepage Builder

사용자 요청: $ARGUMENTS

Build a polished, responsive homepage by combining the `ui-ux-pro-max` design data with a build → screenshot → fix loop. Talk to the user in the language they use (Korean by default).

## 0. Locate the design search tool

The `ui-ux-pro-max` skill ships a Python search script. Find it once and reuse the path:

```bash
UIPRO=""
for p in "${CLAUDE_PLUGIN_ROOT:-}/.claude/skills/ui-ux-pro-max/scripts/search.py" \
         ".claude/skills/ui-ux-pro-max/scripts/search.py" \
         "$HOME/.claude/skills/ui-ux-pro-max/scripts/search.py"; do
  [ -f "$p" ] && UIPRO="$p" && break
done
[ -z "$UIPRO" ] && UIPRO=$(find / -path "*ui-ux-pro-max/scripts/search.py" 2>/dev/null | head -1)
echo "$UIPRO"
```

Run it with `python3` (fall back to `python` or `py -3`). If it cannot be found, continue with the `ui-ux-pro-max` skill's guidance and tell the user the search data was unavailable.

## 1. Brief (ask only what is missing)

From `$ARGUMENTS` and the conversation, pin down:

- **무엇**: business / product type and industry
- **누구**: target visitors
- **목표 행동**: the one main CTA (예약, 문의, 구매, 가입, 다운로드…)
- **분위기**: 2–3 style keywords (미니멀, 고급스러운, 발랄한, 신뢰감…)
- **콘텐츠**: name, slogan, services, contact info, images/logo if they have them

If the business type or main CTA is unknown, ask in ONE short message (max 3 questions). Anything else: choose sensible defaults, state them, and proceed. Use realistic placeholder copy marked clearly as placeholder — never invent reviews, awards, or statistics presented as real.

## 2. Stack

Detect from the project (`package.json` → next / react / vue / svelte / astro / nuxt). If the folder is empty or has no web project, default to **a single static `index.html` with Tailwind (CDN)** — no build step, opens by double-click — and say so. Map to `--stack`: `html-tailwind`, `nextjs`, `react`, `vue`, `svelte`, `astro`, `nuxtjs`, `shadcn`.

## 3. Design system (required)

1. If `design-system/*/MASTER.md` already exists, read it and reuse it. Do not regenerate with `--force` without the user's permission.
2. Otherwise generate and save one (English keywords give the best matches):

```bash
python3 "$UIPRO" "<product type> <industry> <style keywords>" --design-system --persist -p "<Site Name>" --output-dir "<project root>"
python3 "$UIPRO" "landing hero conversion" --stack <stack>
python3 "$UIPRO" "<industry> landing page sections" --domain landing
```

3. Summarize the chosen direction to the user in 4–5 lines (스타일, 메인 색상, 폰트, 섹션 구성). Don't wait for approval unless they asked to review first.

For Korean text, pair the recommended Latin font with a Korean webfont (e.g. Pretendard, Noto Sans KR, or Nanum Myeongjo for serif moods) and set `word-break: keep-all` on body text.

## 4. Build

Typical section order (adapt to the brief and the `landing` search result):
Header/nav → Hero (headline, sub-copy, primary CTA, visual) → 서비스/특징 → 신뢰 요소 (process, 소개, FAQ) → 가격 or 포트폴리오 if relevant → Final CTA → Footer (연락처, 지도/주소, SNS, copyright).

Rules (from ui-ux-pro-max priorities):
- Colors and fonts as CSS variables / Tailwind theme tokens from MASTER.md — no stray hex values
- Mobile-first, no horizontal scroll at 360px, touch targets ≥ 44px
- Text contrast ≥ 4.5:1, visible focus rings, alt text, semantic landmarks, one `<h1>`
- SVG icons (Lucide/Heroicons), never emoji as icons
- Images: real assets from the user when given; otherwise neutral placeholders with correct aspect ratio (reserve space, `loading="lazy"` below the fold)
- Animate with **Motion** (see section 4b): subtle 150–300ms UI motion, 0.4–0.8s section reveals, always respect `prefers-reduced-motion`
- `<title>`, meta description, Open Graph tags, favicon

## 4b. Animation with Motion (default library)

Use [Motion](https://motion.dev) (formerly Framer Motion) for all animation instead of hand-written CSS keyframes or other libraries, unless the project already uses a different one.

**React / Next.js projects:** install it if `motion` is not already in `package.json`:

```bash
npm install motion
```

```jsx
import { motion, AnimatePresence, MotionConfig } from "motion/react"
```

- Next.js App Router: put Motion components in files marked `"use client"` (or import from `"motion/react-client"` in server components).
- Wrap the app once in `<MotionConfig reducedMotion="user">` so reduced-motion settings are honoured everywhere.

| Use | Pattern |
|---|---|
| Hero enter | `<motion.h1 initial={{ opacity: 0, y: 24 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.6, ease: "easeOut" }} />` |
| Buttons / cards | `<motion.button whileHover={{ scale: 1.03 }} whileTap={{ scale: 0.97 }} />` |
| Section reveal on scroll | `<motion.section initial={{ opacity: 0, y: 32 }} whileInView={{ opacity: 1, y: 0 }} viewport={{ once: true, amount: 0.3 }} transition={{ duration: 0.6 }} />` |
| Staggered list | parent `variants` with `transition: { staggerChildren: 0.08 }`, children `variants={{ hidden: { opacity: 0, y: 16 }, show: { opacity: 1, y: 0 } }}` |
| Layout change (tabs, filters, accordion) | `<motion.div layout />` |
| Exit (modal, mobile menu, toast) | `<AnimatePresence>{open && <motion.div key="menu" initial={{ opacity: 0 }} animate={{ opacity: 1 }} exit={{ opacity: 0 }} />}</AnimatePresence>` |
| Scroll progress bar | `const { scrollYProgress } = useScroll()` → `<motion.div style={{ scaleX: scrollYProgress }} />` |

**Static `index.html` (no build step):** use the vanilla build from the CDN, pinned:

```html
<script src="https://cdn.jsdelivr.net/npm/motion@14.9.0/dist/motion.js"></script>
<script>
  const { animate, inView, stagger, hover, press } = Motion
  const reduce = matchMedia("(prefers-reduced-motion: reduce)").matches
  if (!reduce) {
    animate(".hero > *", { opacity: [0, 1], y: [24, 0] }, { duration: 0.6, delay: stagger(0.1) })
    inView("[data-reveal]", (el) => { animate(el, { opacity: [0, 1], y: [32, 0] }, { duration: 0.6 }) }, { amount: 0.3 })
    hover(".btn", (el) => { animate(el, { scale: 1.03 }); return () => animate(el, { scale: 1 }) })
  }
</script>
```

Elements revealed by script must stay visible if JavaScript fails: don't hide them with CSS `opacity: 0` up front — let the animation's starting keyframe do it.

Motion rules:
- Animate only `transform` (x, y, scale, rotate) and `opacity`; never width/height/top/left (use `layout` instead).
- One or two motion ideas per page, used consistently. Reveal each section once (`once: true`); no looping attention-grabbers, no parallax on body text.
- The page must look complete with motion off — check the screenshots both ways.

## 5. Verify with screenshots

Serve or open the page and capture it at **390px (mobile)** and **1440px (desktop)** with Playwright (Chromium). Look at the screenshots yourself and fix: overflow, cramped spacing, low contrast, broken alignment, awkward Korean line breaks, CTA not visible above the fold. Repeat until clean. Then run the pre-delivery checklist in the `ui-ux-pro-max` skill's `references/pro-rules.md`.

## 6. Hand off

Tell the user briefly (in their language): what was built, file paths, how to open/run it, the design direction chosen, and which placeholder content they should replace. Offer 2–3 concrete next steps (예: 서브페이지 추가, 문의 폼 연결, 배포).
