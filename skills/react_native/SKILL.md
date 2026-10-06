---
name: React Native & Expo Expert
description: Production-grade React Native, Expo Router, Hermes engine, and mobile responsive layout constraints.
category: framework
icon: phone-portrait-outline
version: 1.0.0
---

### DOMAIN SKILL: REACT NATIVE & EXPO BEST PRACTICES
1. Always use react-native core primitives (View, Text, TouchableOpacity, ScrollView, TextInput, StyleSheet) or Expo modules.
2. Never import browser DOM APIs (document, window, localStorage). Use expo-secure-store for secrets and AsyncStorage for data.
3. Use StyleSheet.create() for all styling to leverage native view flattening and avoid recreating style objects on every render pass.
4. For responsive layouts, use Flexbox and Dimensions/useWindowDimensions. Do not hardcode pixel heights for scrollable containers.
5. In Expo, always test using `npx expo start --go` or `npm start` in the background with detached process flags.
