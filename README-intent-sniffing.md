## Intent Sniffing

Dopo il decompiling dell'applicazione

> AndroidManifest.xml

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
	
	...
}	
```

Exploit:

- \# TODO

N.B. posso accedere a `Change Password` lanciando `$ adb shell am start -n com.android.insecurebankv2/.ChangePassword`
