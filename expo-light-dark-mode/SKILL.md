---
name: expo-light-dark-mode
description: Prevents light/dark appearance mismatches in Expo apps, especially iOS liquid glass headers, status bars, and safe-area backgrounds.
---

# expo-light-dark-mode

Use this skill to keep app theme behavior consistent across Expo Router, status bar, headers, and iOS liquid glass materials.

## When to use

Use this skill when:
- the app supports light/dark mode,
- headers use native-stack glass/translucent styles,
- users report a brief wrong color flash in top bars,
- status bar and header contrast look inconsistent during transitions,
- `userInterfaceStyle` is set to `"automatic"` and the app looks different from expected design intent.

## Instructions

1. Identify the appearance source of truth
   - Check `app.json` `expo.userInterfaceStyle`.
   - `"automatic"` means the app follows device preference.
   - If device is dark but screen design is light, iOS liquid glass can initially tint from dark system appearance and then look wrong during transitions.

2. Understand the liquid glass mismatch pitfall
   - Native glass/translucent header areas are influenced by system appearance and what is behind the header.
   - If your app forces light-looking screens while system preference is dark (or vice versa), the glass region can show an incorrect tint first, then settle, creating a visible "weird" flash.
   - Treat this as a major UX consistency error, not a minor visual detail.

3. Apply a consistent strategy across the whole app
   - Keep status bar, navigator header options, and screen backgrounds aligned with the same theme decision.
   - Avoid mixing transparent headers with undefined top backgrounds.
   - Ensure safe-area top region has an intentional background.

4. If the product should always look bright
   - Set `expo.userInterfaceStyle` to `"light"` in `app.json`.
   - Keep header/status bar settings consistent with light mode.
   - Do not rely on `"automatic"` if you do not actually want device-driven dark behavior.

5. If the product supports both light and dark
   - Keep `"automatic"`, but ensure every route has explicit top background and matching header/status bar contrast.
   - Verify transitions on iOS with both device appearances enabled.

## Quick checks before merge

- [ ] `app.json` appearance setting matches product intent.
- [ ] No screen has theme-opposite header/status bar values.
- [ ] Transparent/glass header flows have explicit safe-area top backgrounds.
- [ ] iOS light and dark transitions do not flash incorrect tint at the top bar.
