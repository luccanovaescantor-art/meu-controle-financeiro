name: Gerar APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Baixar projeto
        uses: actions/checkout@v4

      - name: Extrair projeto Android
        run: |
          unzip -q Meu_Controle_Financeiro_Android.zip
          cd controle_financeiro_android

      - name: Configurar Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Configurar Android SDK
        uses: android-actions/setup-android@v3

      - name: Instalar componentes Android
        run: |
          sdkmanager "platforms;android-35" "build-tools;35.0.0"

      - name: Instalar Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.10'

      - name: Compilar APK
        working-directory: controle_financeiro_android
        run: gradle assembleDebug

      - name: Disponibilizar APK
        uses: actions/upload-artifact@v4
        with:
          name: Meu-Controle-Financeiro-APK
          path: controle_financeiro_android/app/build/outputs/apk/debug/app-debug.apk
