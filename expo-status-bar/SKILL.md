---
name: expo-status-bar
description: Diagnoses and fixes Expo/React Navigation status bar mismatches. Use when status bar text/icons are hard to read, not matching header/background, flickering between screens, or behaving unexpectedly with transparent headers and safe areas.
---

# Expo Status Bar

## When To Use

Use this skill when:
- `StatusBar style="auto"` looks wrong for a screen.
- Transparent headers make the top area hard to read.
- Status bar behavior differs between routes or during navigation.
- A root `StatusBar` is not matching what appears on specific screens.

## Core Rule

Treat status bar behavior as the result of three layers:
1. **Owner**: who controls style (`expo-status-bar` component vs navigator `statusBar*` options).
2. **Top background**: what color/content is actually behind the status bar.
3. **Safe area layout**: whether the top inset is intentionally handled.

Do not assume `auto` reads your screen background. It follows platform/theme behavior and can still look visually incorrect when the top region is not what you expected.

## Troubleshooting Workflow

### 1) Identify the single source of truth

Check if both of these are set:
- `<StatusBar ... />` in root or screens
- navigator `statusBarStyle`, `statusBarHidden`, or related screen options

If both exist, pick one owner for that flow and remove conflicting overrides.

### 2) Check transparent header interactions

If `headerTransparent: true`, content can render behind header/status bar.
Then verify:
- a top-safe-area strategy exists (screen-level `SafeAreaView` or `useSafeAreaInsets` top padding),
- the top region has a deliberate background color (not accidental transparency).

### 3) Verify background under the status bar

Inspect the top-most rendered container on each affected screen:
- If top is light, prefer dark status bar content.
- If top is dark, prefer light status bar content.
- If dynamic or image-heavy, avoid `auto`; set explicit per-screen style.

### 4) Audit route-level differences

Compare affected and unaffected screens for:
- different wrappers (`SafeAreaView`, themed containers),
- different header options,
- conditional rendering during transitions.

### 5) Lock a strategy per flow

Pick one:
- **Global strategy**: root `<StatusBar style="...">` only, no route overrides.
- **Per-route strategy**: explicit screen styles based on known background.

Avoid mixing strategies unless there is a clear reason.

## Fix Patterns

### Pattern A: Global owner

- Keep one root `<StatusBar ... />`.
- Remove navigator `statusBar*` options.
- Ensure every screen's top area has predictable contrast.

### Pattern B: Per-screen owner

- Set explicit style per route when backgrounds differ.
- Keep top safe-area/background consistent with each explicit style.

### Pattern C: Transparent header flow

- Keep header transparent for visual design.
- Ensure top inset handling in content.
- Set status bar style intentionally for the onboarding/auth flow if needed.

## Quick Checklist

- [ ] Only one status bar owner per flow.
- [ ] `headerTransparent` screens handle top inset intentionally.
- [ ] Top region behind status bar has intentional background.
- [ ] `auto` is only used where it consistently looks correct.
- [ ] No hidden per-screen override conflicts.
