## Weak Cryptography

- `$ adb shell`

- `# cd /data/data/com.android.insecurebankv2/shared_prefs`

- `# cat mySharedPreferences.xml`

```
<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<map>
    <string name="superSecurePassword">vK3CrqW6+zg1s9V0YZp0Uw==</string>
    <string name="EncryptedUsername">amFjaw==</string>
</map>`
```

- `$ echo amFjaw== | base64 -d`

```
jack
```

> app/src/main/java/com/android/insecurebankv2/CryptoClass.java

```
String key = "This is the super secret key 123";
byte[] ivBytes = {0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00};

public static byte[] aes256encrypt(byte[] ivBytes, byte[] keyBytes, byte[] textBytes){
	AlgorithmParameterSpec ivSpec = new IvParameterSpec(ivBytes);
	SecretKeySpec newKey = new SecretKeySpec(keyBytes, "AES");
	Cipher cipher = null;
	cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
	cipher.init(Cipher.ENCRYPT_MODE, newKey, ivSpec);
	return cipher.doFinal(textBytes);
}
```


- `$ echo -n "This is the super secret key 123" | od -A n -t x1 | tr -d ' ' | tr -d '\n'`

```
546869732069732074686520737570657220736563726574206b657920313233
```

- `$ openssl enc -d -base64 -aes-256-cbc -K 546869732069732074686520737570657220736563726574206b657920313233 -iv 0 -in test.enc -out test.txt`

```
Jack123$
```

