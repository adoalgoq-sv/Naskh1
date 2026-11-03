
name: Build APK
on:
  push:
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: unzip -q Naskh2.zip
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: 8.7
      - name: Build
        working-directory: Naskh2
        run: |
          gradle assembleDebug --no-daemon --console=plain > build.log 2>&1 || { grep -m1 -A15 "What went wrong" build.log; grep -m5 "^e: " build.log; exit 1; }
      - uses: actions/upload-artifact@v4
        with:
          name: Naskh-APK
          path: Naskh2/app/build/outputs/apk/debug/app-debug.apk
