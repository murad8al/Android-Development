# check wifi (turned on or of)

## androidManifest.xml

```Kotlin
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
```

## MainActivity


```Kotlin
lateinit var wifiManager: WifiManager
wifiManager.isWifiEnabled = true
wifiManager.isWifiEnabled = false
```

## mobile data


```java
public boolean getMobileDataState() {
      try {
         TelephonyManager telephonyService = (TelephonyManager) getSystemService(Context.TELEPHONY_SERVICE);
         Method getMobileDataEnabledMethod = Objects.requireNonNull(telephonyService).getClass().getDeclaredMethod("getDataEnabled");
         return (boolean) (Boolean) getMobileDataEnabledMethod.invoke(telephonyService);
      } catch (Exception ex) {
         Log.e("MainActivity", "Error getting mobile data state", ex);
      }
      return false;
   }
```


