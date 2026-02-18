---
name: pressto-button
description: Tells you how to use the pressto button in react-native/expo apps
---

# Setup

Add `PressablesConfig` with haptics to your root layout:

```tsx
import { PressablesConfig } from "pressto";
import * as Haptics from "expo-haptics";

export default function RootLayout() {
  return (
    <PressablesConfig
      globalHandlers={{
        onPressIn: () => Haptics.selectionAsync(),
      }}
    >
      <Stack>
        <Stack.Screen name="(tabs)" options={{ headerShown: false }} />
      </Stack>
    </PressablesConfig>
  );
}
```

# Usage

```tsx
import { PressableScale, PressableOpacity } from "pressto";

// Scales down on press
<PressableScale onPress={() => {}}>
  <Text>Scale</Text>
</PressableScale>

// Fades when pressed
<PressableOpacity onPress={() => {}}>
  <Text>Opacity</Text>
</PressableOpacity>
```

Supports standard Pressable props.

## Notes

Do not add extra animations beyond what Pressto provides. Custom animations can cause text to appear blurry when pressing. Use only the built-in scale/opacity effects.