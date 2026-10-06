### DOMAIN SKILL: REACT NATIVE & EXPO ARCHITECTURE
1. Use StyleSheet.create for all styles; avoid ad-hoc inline objects in render loops to preserve native flat view performance.
2. Remember that New Architecture has Hermes enabled by default; experimental layout animations are no-ops.
3. For push notifications in Expo SDK 53+, inform the user that a development build is required instead of Expo Go.
4. Always wrap top-level mobile views in SafeAreaView from react-native-safe-area-context.
5. In Expo, always test using `npx expo start --go` or `npm start` in the background with detached process flags.
