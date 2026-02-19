---
name: expo-safeareaview-guidlines
description: Applies safe-area layout rules for Expo/React Native screens, especially with React Navigation headers. Use when content overlaps the notch/status bar, spacing feels inconsistent, or `headerTransparent: true` is enabled.
---

# Expo SafeAreaView Guidelines

## When To Use

Use this skill when:
- a stack screen uses `headerTransparent: true`,
- content appears under the notch/status bar unexpectedly,
- top spacing differs between screens,
- safe area decisions are unclear in Expo Router apps.

## Non-Negotiable Rule

If a stack/screen uses `headerTransparent: true`, each page in that flow must handle the top safe area in its content container.

Preferred approach for this project:
- add a `SafeAreaView` inside each page/screen component as the top-level wrapper,
- keep `flex: 1`,
- ensure the visible top background is intentional.

This is required because transparent headers do not create top content spacing for you.

## Standard Layout Pattern

For pages under a transparent header:
1. Page root container: `SafeAreaView` (`flex: 1`).
2. Inner content wrapper for actual UI.
3. Explicit background at the top area (screen or wrapper), not accidental transparent layering.

If a page cannot use `SafeAreaView`, use `useSafeAreaInsets()` and apply `paddingTop: insets.top` manually.

## Implementation Checklist

- [ ] `headerTransparent: true` is present? If yes, safe-area handling exists in each page.
- [ ] Screen root is `SafeAreaView` with `flex: 1`.
- [ ] No missing wrapper on any route in the flow.
- [ ] Background behind top inset is intentional and consistent.
- [ ] No double top spacing from stacked wrappers.

## Common Pitfalls

- Relying on navigator header options to handle content inset for transparent headers.
- Using `contentStyle: { backgroundColor: 'transparent' }` without defining page-level background.
- Having some pages wrapped in `SafeAreaView` and others not, causing jumpy alignment.
- Nesting multiple top-safe-area wrappers and creating extra padding.

## Decision Rules

- **Transparent header flow**: safe area is a screen responsibility.
- **Opaque header flow**: still use safe area for predictable notch handling, but top overlap risk is lower.
- **Mixed backgrounds/images**: prefer explicit per-screen control over implicit behavior.

## QA Pass Before Merge

- Navigate every screen in the flow on iOS and Android.
- Confirm title/back button and first content row do not collide.
- Confirm status bar region remains readable on every route.
- Confirm transitions do not cause temporary top overlap artifacts.
