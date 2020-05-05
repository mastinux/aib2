## Bypass Android Root Detection

> app/src/main/java/com/android/insecurebankv2/PostLogin.java

```
void showRootStatus() {
	boolean isrooted = doesSuperuserApkExist("/system/app/Superuser.apk")|| doesSUexist();
	
	if(isrooted==true){
		root_status.setText("Rooted Device!!");
	}
	else{
		root_status.setText("Device not Rooted!!");
	}
}

private boolean doesSUexist() {
	Process process = null;
	try {
		process = Runtime.getRuntime().exec(new String[] { "/system/bin/which", "su" });
		BufferedReader in = new BufferedReader(new InputStreamReader(process.getInputStream()));
		if (in.readLine() != null)
			return true;
		return false;
	} catch (Throwable t) {
		return false;
	} finally {
		if (process != null) process.destroy();
	}
	

private boolean doesSuperuserApkExist(String s) {
	File rootFile = new File("/system/app/Superuser.apk");
	Boolean doesexist = rootFile.exists();

	if(doesexist == true){
		return(true);
	}
	else{
		return(false);
	}
}
```

N.B. il codice non è in grado di fare la root detection su android emulato

\# TODO modifica il codice e poi prova a fare il bypass
