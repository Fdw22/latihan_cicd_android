# Codingan untuk .travis.yml
---------------------------------------------------------------------------------------------------
language: android
jdk: oraclejdk8
env:
  global:
    - ANDROID_API_LEVEL=29
    - ANDROID_BUILD_TOOLS_VERSION=29.0.2
android:
  licenses:
    - 'android-sdk-preview-license-.+'
    - 'android-sdk-license-.+'
    - 'google-gdk-license-.+'
  components:
    - tools
    - platform-tools
    # The BuildTools version used by your project
    - build-tools-$ANDROID_BUILD_TOOLS_VERSION
    # The SDK version used to compile your project
    - android-$ANDROID_API_LEVEL
    # Additional components
    - extra-google-google_play_services
    - extra-google-m2repository
    - extra-android-m2repository
    - addon-google_apis-google-$ANDROID_API_LEVEL
before_script:
  # Prepare pre-accepted licenses to not be promted at installation
  - yes | sdkmanager "platforms;android-29"
script:
  - chmod +x ./gradlew
  - ./gradlew test
  - ./gradlew assemblerelease

deploy:
  provider: releases
  api-key: $GITHUB_API_KEY
  file: $TRAVIS_BUILD_DIR/app/build/outputs/apk/release/app-release.apk
  skip_cleanup: true
  name: dev-build-$TRAVIS_TAG
  body: Automatic build of $TRAVIS_BRANCH ($TRAVIS_COMMIT) built by Travis CI on $(date +'%F %T %Z').
  prerelease: true
  overwrite: true
  target_commitish: $TRAVIS_COMMIT
  on:
    tags: true

after_deploy:
  - rm -rf $TRAVIS_BUILD_DIR/app/build/outputs
  
  ---------------------------------------------------------------------------------------------------
  
  jika terjadi error pada travis lakukan 

lakukan perintah ini pada git bash ato terminal 

keytool -genkeypair -v -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000

lalu isi smua nya dengan test dan isi pass dan username dengan qwerty

lalu ada file baru pada andro studio 

pindahkan file baru ke apps, dan lakukan konfigurasi pada build gradle properties yg diluar dan tambahkan ini 

MYAPP_UPLOAD_STORE_FILE=my-upload-key.keystore
MYAPP_UPLOAD_KEY_ALIAS=my-key-alias
MYAPP_UPLOAD_STORE_PASSWORD=qwerty
MYAPP_UPLOAD_KEY_PASSWORD=qwerty

lalu ke build gradle dalam app dan tambahkan ini di setelah default config

signingConfigs {
        release {
            if (project.hasProperty('MYAPP_UPLOAD_STORE_FILE')) {
                storeFile file(MYAPP_UPLOAD_STORE_FILE)
                storePassword MYAPP_UPLOAD_STORE_PASSWORD
                keyAlias MYAPP_UPLOAD_KEY_ALIAS
                keyPassword MYAPP_UPLOAD_KEY_PASSWORD
            }
        }
    }

dan tambahkan ini pada buildtype

signingConfig signingConfigs.release 
