# CrediYa-Mx
Es una app de préstamos personales sin checar número de credito. Fácil, rápido y seguro 
name: Build Android App

on:
  push:
    branches: [ main, master ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Set up JDK 17
      uses: actions/setup-java@v4
      with:
        distribution: 'temurin'
        java-version: '17'

    - name: Decode Keystore
      run: |
        echo "${{ secrets.KEYSTORE_BASE64 }}" | base64 --decode > app/crediya-key.jks

    - name: Build with Gradle
      run: ./gradlew bundleRelease
      env:
        KEYSTORE_PASSWORD: ${{ secrets.KEYSTORE_PASSWORD }}
        KEY_ALIAS: ${{ secrets.KEY_ALIAS }}
        KEY_PASSWORD: ${{ secrets.KEY_PASSWORD }}

    - name: Upload Artifact
      uses: actions/upload-artifact@v4
      with:
        name: release-aab
        path: app/build/outputs/bundle/release/app-release.aab
        
