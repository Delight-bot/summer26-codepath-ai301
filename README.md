```markdown
# Contribution [1]: [Arabic text displays LTR instead of RTL]

**Contribution Number:** [1]  
**Student:** Delight Nyanhete  
**Issue:** [[GitHub issue link](https://github.com/stoatchat/for-web/issues/964)]  
**Status:** [Phase I Complete]

---

## Why I Chose This Issue

This issue addresses a bug where Arabic text is being displayed left-to-right instead of right-to-left, breaking the reading experience for Arabic-speaking users. Text directionality is a fundamental part of internationalization. Getting it wrong makes the interface effectively unusable for that audience.

This is my very first open source contribution, and rather than let that be a reason to back out, I chose an issue that plays to my existing JavaScript and TypeScript experience while keeping the scope manageable. My real goal here isn't just the fix. I want to learn how to navigate an unfamiliar codebase, understand contribution workflows, and build the confidence to take on bigger issues next time.

---

## Understanding the Issue

### Problem Description

According to the screenshots and help request, Arabic text in the application is being rendered left-to-right instead of right-to-left. Arabic is a RTL language, meaning the text direction and layout need to flow from right to left. Without this, the text appears in the wrong order and the interface becomes difficult or impossible to read for Arabic speakers. The issue also affects the settings page and some strings appear untranslated

### Expected Behavior

Arabic text should render right-to-left across all parts of the interface, including the settings page. The layout and text alignment should respect RTL direction so the app is readable and usable for Arabic-speaking users.

### Current Behavior

Arabic text is displayed left-to-right, which reverses the natural reading direction. The settings page is also affected, and some UI strings are either missing translations or showing in the wrong language entirely

### Reproduction Process

### Environment Setup
Used mise + recursive clone in WSL2 Ubuntu (had to install WSL2 first since I'm
on Windows). Needed to add `local.revolt.chat` to the Windows hosts file for
the dev server to work correctly with cookies/CORS.

### Steps to Reproduce
1. Run `mise dev`, open http://local.revolt.chat:5173, log in
2. Go to Settings → Language → select "عربي" (Arabic)
3. **Expected:** entire UI mirrors right-to-left, including the Settings panel
4. **Actual:** strings translate to Arabic but layout stays left-to-right everywhere

### Reproduction Evidence
- **Branch:** https://github.com/Delight-bot/for-web/tree/fix-issue-964
- **My findings:** Traced the bug to the codebase — `Languages.ts` already
  defines `rtl: true` for Arabic and the `Locale` store validates an
  `options.rtl` flag, but nothing in the app ever reads it. `loadAndSwitchLocale()`
  in `i18n/index.tsx` only swaps translation strings, never sets
  `document.documentElement.dir`. There's even a `ConfigureRTL()` component
  in `Language.tsx` for this — fully commented out and never wired up.

## Solution Approach

### Implementation Plan
**Understand:** Language switching swaps translated strings but never updates
document direction, despite the data model already having an `rtl` flag.

**Match:** `LoadTheme.tsx` (packages/client/components/ui/themes/LoadTheme.tsx)
shows the right pattern — a `createEffect` that reactively pushes global
state onto the document root.

**Plan:**
1. Add a `LoadDirection`-style component mirroring `LoadTheme.tsx` that sets
   `document.documentElement.dir` from `Languages[lang].localeOptions?.rtl`
2. Add `setRtl()` to the `Locale` store for manual override
3. Uncomment and wire up `ConfigureRTL()` in `Language.tsx`
4. Add missing `rtl: true` to `fa`, `ur`, `ckb` in `Languages.ts`
5. Spot-check sidebar/settings/message list under `dir="rtl"` for hardcoded
   left/right CSS

**Review:** Will follow GUIDELINES.md (comment above all Solid components,
no destructuring reactive props, 2-space indent) before opening PR.

**Evaluate:** Manually confirm Arabic flips direction app-wide including
Settings; confirm other languages still render LTR; run `mise check`.

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
```

To complete Phase I, you need to do seven things:
1. Log into your GitHub and Slack accounts so you have access to the tools and systems you'll use for the rest of the program.
2. Create your Contribution README — the living document that will track your entire journey through the program. You'll create a new public GitHub repository and copy in the  [provided template](https://courses.codepath.org/course_files/ai301/contribution_readme.md).
** 3. Find one live GitHub issue from our list that you will work on for the next several weeks.
4. Update your README to demonstrate that you understand what your issue is actually asking for with:

* A link to your chosen GitHub issue in the Issue field
* A 2–4 sentence problem summary explaining what the issue is, why it matters, and why you chose it in Why I Chose This Issue
5. Leave a comment on your chosen issue expressing interest and any initial questions you have. Leave a comment on our course's Google Sheet on your issue's row, too, so we don't recommend your issue to your classmates.
6. Fork your chosen project so you have your own copy of the repository to work in.
7. Submit your check-in form and indicate "Phase I Complete."
No code. No local environment setup. No fork cloning. That all happens in Phase II. Phase I is about getting set up, setting up your README, and choosing well. what do i have to do, i found one , then what
