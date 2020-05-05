## Developer Backdoor

Codice Vulnerabile:

> app/src/main/java/com/android/insecurebankv2/DoLogin.java

```
HttpPost httppost2 = new HttpPost(protocol + serverip + ":" + serverport + "/devlogin");

...

if (username.equals("devadmin")) {
	httppost2.setEntity(new UrlEncodedFormEntity(nameValuePairs));
	// Execute HTTP Post Request
	responseBody = httpclient.execute(httppost2);
} else {
	httppost.setEntity(new UrlEncodedFormEntity(nameValuePairs));
	// Execute HTTP Post Request
	responseBody = httpclient.execute(httppost);
}
```
