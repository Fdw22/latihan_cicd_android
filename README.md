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
