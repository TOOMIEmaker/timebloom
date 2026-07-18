# Time Bloom

A calming mobile app designed for people with ADHD, autism, and anxiety who experience time blindness. Gentle hourly reminders, mood check-ins, a growing crystal bloom, and dreamy visual themes make time awareness feel comforting rather than stressful.

## Run & Operate

- `pnpm --filter @workspace/time-bloom run dev` — run the Expo dev server
- Scan the QR code in the Expo Go app (iOS or Android) to preview on a real device

## Stack

- pnpm workspaces, Expo SDK 54, React Native 0.81, TypeScript 5.9
- Routing: Expo Router (file-based, tabs)
- State: React Context + AsyncStorage (fully offline)
- Animations: react-native-reanimated 4
- SVG: react-native-svg
- Gradients & blur: expo-linear-gradient, expo-blur

## Where things live

- `artifacts/time-bloom/` — the main Expo app
- `artifacts/time-bloom/app/(tabs)/` — 4 screens: Home, Timeline, Analytics, Settings
- `artifacts/time-bloom/context/AppContext.tsx` — all app state (awareness mode, reminders, moods, settings)
- `artifacts/time-bloom/context/ThemeContext.tsx` — active theme derived from settings
- `artifacts/time-bloom/constants/themes.ts` — 6 theme definitions (Dream Sky, Cherry Blossom, Night Forest, Galaxy, Winter Snow, Ocean)
- `artifacts/time-bloom/components/` — CrystalBloom, ParticleField, FloatingCompanion, ReminderOverlay, GlassCard

## Architecture decisions

- All data stored locally via AsyncStorage — fully offline, no backend required
- Awareness mode uses a 1-second interval ticker in AppContext (instead of push notifications) so reminders work while the app is in the foreground
- ThemeProvider reads `settings.theme` from AppContext — must be mounted inside AppProvider
- ReminderOverlay is rendered at the root layout level (above navigation) as a transparent Modal
- CrystalBloom bloom progress is derived from time of day (full bloom at noon, seed at midnight)

## Product

- **Home**: Live clock, greeting, animated crystal bloom, Start/Stop Awareness Mode button with countdown
- **Timeline**: Daily log of reminder moments with mood badges
- **Analytics**: Weekly mood bar chart, top mood, peak hour, reminder count
- **Settings**: Reminder interval (15–120 min), 8 sound choices, 6 themes, 6 companion characters, sleep hours (DND), accessibility toggles

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- `useAnimatedStyle` must never be called inside `.map()` — extract animated items into separate components
- `isInSleepHours` in AppContext silently skips reminders during configured sleep window
- The awareness mode timer is a `setInterval` (1s) that reads refs — do not rely on state values directly inside the interval callback
