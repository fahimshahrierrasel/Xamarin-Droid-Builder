# Xamarin-Droid-Builder
Build and Generate Android APK for `Xamarin Form` and `Xamarin Android`.

## Usage
First Need to set 2 variable in secrets in Github project settings.
Variables Names are

`SOLUTION_NAME` : Solution Name or the root folder name.

`ANDROID_PROJECT_NAME` : Android Project name or the folder name of the android project usually its end with `.Android` as postfix.

Main Workflow:
```
on: push
name: Build and Generate Android Apk
env:
  SOLUTION_NAME: ${{secrets.SOLUTION_NAME}}
  ANDROID_PROJECT_NAME: ${{secrets.ANDROID_PROJECT_NAME}}
jobs:
  build:
    name: Build APK
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - name: Building APK
        uses: ./action
        with:
          args: /r:True /t:SignAndroidPackage /p:AndroidSdkDirectory=/android/sdk ${{env.SOLUTION_NAME}}/${{env.ANDROID_PROJECT_NAME}}/${{env.ANDROID_PROJECT_NAME}}.csproj
      - name: Artifacts
        run: |
          mkdir -p apks
          cp ${{env.SOLUTION_NAME}}/${{env.ANDROID_PROJECT_NAME}}/bin/Debug/*.apk apks
      - name: Upload Artifacts
        uses: actions/upload-artifact@v1
        with:
          name: apks
          path: apks
```
