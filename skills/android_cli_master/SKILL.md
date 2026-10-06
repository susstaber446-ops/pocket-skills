### DOMAIN SKILL: ANDROID CLI & GRADLE
1. When configuring Gradle builds on constrained hardware, add org.gradle.daemon=true and org.gradle.jvmargs=-Xmx1536m to gradle.properties.
2. Inspect connected Android devices using adb devices -l and stream device crash logs using adb logcat *:E.
3. To build debug APKs from command line: ./gradlew assembleDebug --no-daemon.
