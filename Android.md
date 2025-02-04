# 네이티브 모듈을 활용한 사용자 상태 수집 및 백그라운드 실행

## 개요
이 프로젝트에서는 네이티브 모듈을 활용하여 사용자 상태를 수집하고 백그라운드에서도 실행될 수 있도록 구현하였다. 
네이티브 모듈을 통해 **배터리 상태**, **네트워크 연결 상태**, **화면 상태 (Screen On/Off)** 를 지속적으로 모니터링하며, 이를 React Native와 연동하여 사용자 상태를 관리한다. 
또한, **Foreground Service** 를 활용하여 백그라운드에서도 기능이 지속 실행될 수 있도록 설계하였다.

## 주요 기능 및 구현 방식

### 1. Foreground Service를 활용한 지속적인 사용자 상태 모니터링
Android에서는 앱이 백그라운드로 전환되면 일반적인 서비스는 종료될 수 있다. 이를 방지하기 위해 **Foreground Service**를 사용하여 앱이 종료되지 않도록 유지하였다.

#### **ForegroundService.kt 구현**
```kotlin
startForeground(NOTIFICATION_ID, notification) // Foreground Service 실행

private val handler = Handler(Looper.getMainLooper())
private val updateInterval = 10000L // 10초 간격으로 업데이트
handler.postDelayed(object : Runnable {
    override fun run() {
        updateUserStatus()
        handler.postDelayed(this, updateInterval)
    }
}, updateInterval)
```

### 2. 배터리, 네트워크, 화면 상태 수집
사용자의 상태를 지속적으로 모니터링하기 위해 **UserStatusProvider.kt**를 구현하였다.

#### **배터리 상태 수집 (getBatteryStatus)**
```kotlin
val batteryIntent = context.registerReceiver(null, IntentFilter(Intent.ACTION_BATTERY_CHANGED))
val level = batteryIntent?.getIntExtra(BatteryManager.EXTRA_LEVEL, -1) ?: -1
val isCharging = batteryIntent?.getIntExtra(BatteryManager.EXTRA_PLUGGED, -1) in listOf(
    BatteryManager.BATTERY_PLUGGED_AC, BatteryManager.BATTERY_PLUGGED_USB, BatteryManager.BATTERY_PLUGGED_WIRELESS
)
```

#### **네트워크 상태 수집 (getNetworkStatus)**
```kotlin
val connectivityManager = context.getSystemService(Context.CONNECTIVITY_SERVICE) as ConnectivityManager
val network = connectivityManager.activeNetwork
val networkCapabilities = connectivityManager.getNetworkCapabilities(network)
val isConnected = networkCapabilities?.hasCapability(NetworkCapabilities.NET_CAPABILITY_INTERNET) == true
```

#### **화면 상태 감지 (getScreenStatus)**
```kotlin
val powerManager = context.getSystemService(Context.POWER_SERVICE) as PowerManager
val isScreenOn = powerManager.isInteractive
```

### 3. React Native와의 연동
React Native에서 네이티브 모듈과 통신하기 위해 **DeviceEventManagerModule**을 사용하였다. 이를 통해 앱이 실행 중이든 백그라운드에 있든 상태 변경 이벤트를 RN에 전달할 수 있다.

#### **React Native로 상태 이벤트 전송 (sendEventToReactNative)**
```kotlin
private fun sendEventToReactNative(data: Map<String, Any?>) {
    val reactContext = (application as MainApplication).reactNativeHost.reactInstanceManager.currentReactContext
    reactContext?.let {
        val writableMap = Arguments.createMap()
        data.forEach { (key, value) ->
            when (value) {
                is String -> writableMap.putString(key, value)
                is Boolean -> writableMap.putBoolean(key, value)
                is Int -> writableMap.putInt(key, value)
                is Double -> writableMap.putDouble(key, value)
            }
        }
        it.getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter::class.java)
            .emit("UserStateUpdate", writableMap)
    }
}
```

### 4. React Native에서 이벤트 수신 및 상태 관리
React Native에서는 **NativeEventEmitter**를 사용하여 네이티브 모듈로부터 상태 업데이트 이벤트를 받는다.

#### **이벤트 리스너 등록 (SampleHomeScreen.tsx)**
```tsx
useEffect(() => {
  const eventEmitter = new NativeEventEmitter(ForegroundServiceModule);
  const subscription = eventEmitter.addListener('UserStateUpdate', (data) => {
    console.log('상태 업데이트 이벤트 수신:', data);
    setUserState(data.userState, data.code);
    setBatteryStatus(data.batteryLevel, data.isCharging);
    setScreenStatus(data.screenStatus);
    setNetworkConnected(data.networkStatus);
  });
  return () => {
    subscription.remove();
  };
}, []);
```

### 5. 백그라운드에서 서비스 유지
서비스가 강제 종료되지 않도록 **onTaskRemoved** 와 **onDestroy** 에서 재시작 로직을 추가하였다.

```kotlin
override fun onTaskRemoved(rootIntent: Intent?) {
    super.onTaskRemoved(rootIntent)
    restartService()
}

private fun restartService() {
    val restartServiceIntent = Intent(applicationContext, ForegroundService::class.java).apply {
        setPackage(packageName)
    }
    val restartServicePendingIntent = PendingIntent.getService(
        this, 1, restartServiceIntent, PendingIntent.FLAG_IMMUTABLE
    )
    val alarmManager = getSystemService(Context.ALARM_SERVICE) as AlarmManager
    alarmManager.set(
        AlarmManager.ELAPSED_REALTIME,
        SystemClock.elapsedRealtime() + 1000,
        restartServicePendingIntent
    )
}
```

## 결론
이 프로젝트에서는 **Foreground Service**를 활용하여 사용자 상태(배터리, 네트워크, 화면)를 실시간으로 감지하고, 이를 React Native와 연동하는 기능을 구현하였다. 

- 네이티브 모듈을 활용하여 **백그라운드에서도 지속적인 모니터링이 가능**하도록 설계
- 이벤트 기반으로 **React Native와 원활한 데이터 교환 가능**
- 이를 통해 **백그라운드에서도 중요한 사용자 상태 변화를 감지**하고, 적절한 알림을 제공하며, React Native에서 UI 업데이트가 가능하게 만들었다.
