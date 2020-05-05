---
title: Android StrictMode? 그게뭔데 ? 
last_modified_at: '2020-05-05 15:00:00 +0300'
published : true
excerpt: Android 앱을 더 잘 만들기위한 방법을 소개합니다.
comments : true
toc : true
toc_sticky: true
tags :
    - Kotlin
    - Android
    - StrictMode
---

### 1. 개요

Android 앱을 개발할 때, 사용자를 위해 다양한 요소를 고려해야합니다. 그 중 UI에 관해서 장시간이 걸리게 되면 사용자는 앱을 이탈하는 경우가 생깁니다. 과부화가 걸리는 부분을 찾아주는 강력한 기능이지만, 잘 모르고 있는 StrictMode에 대해 안내해드리겠습니다. 👀

### 2. 코드

*AppliCation*, *Activity*에 onCreate()에 아래와 같이 작성하면 기능을 실행시킬 수 있습니다.
빌더패턴으로 구성되어 있으며, 필요한 부분에 따라 선택적으로 구현을 할 수 있습니다.

#### 선택 감시

```kotlin
    /* Thread 선택 감시 */
    StrictMode.setThreadPolicy(
        StrictMode.ThreadPolicy.Builder()
            .detectCustomSlowCalls()
            .detectDiskReads()
            .detectDiskWrites()
            .detectNetwork()
            .detectResourceMismatches()
            .detectUnbufferedIo()
            .penaltyDeath()
            .penaltyDeathOnNetwork()
            .penaltyDialog()
            .penaltyDropBox()
            .penaltyFlashScreen()
            .paentlyLog()
            .build()
    )
    /*  Virtual Machine 선택 감시 */
    StrictMode.setVmPolicy(
        StrictMode.VmPolicy.Builder()
            .detectActivityLeaks()
            .detectCleartextNetwork()
            .detectContentUriWithoutPermission()
            .detectCredentialProtectedWhileLocked()
            .detectFileUriExposure()
            .detectImplicitDirectBoot()
            .detectLeakedClosableObjects()
            .detectLeakedRegistrationObjects()
            .detectLeakedSqlLiteObjects()
            .detectNonSdkApiUsage()
            .detectUntaggedSockets()
            .penaltyDeath()
            .penaltyDeathOnCleartextNetwork()
            .penaltyDeathOnFileUriExposure()
            .penaltyDropBox()
            .penaltyLog()
            .build()
    )

```

#### 간소화

```kotlin
    /* Thread 감시 */
    StrictMode.setThreadPolicy(
        StrictMode.ThreadPolicy.Builder()
            .detectAll()
            .paentlyLog()
            .build()
    )

    /*  Virtual Machine 감시 */
    StrictMode.setVmPolicy(
        StrictMode.VmPolicy.Builder()
            .detectAll()
            .penaltyLog()
            .build()
    )

```

### 3. 결론

- 현재 구글에서 제공중인 API이며 StackOverFlow에 올라오는 이슈에 관해 지속적으로 모니터링중에 있다고 합니다. 다만, 해당 로그가 발생하는 부분에서는 확정적인 부분이 아니며 참고하는 용도로만 사용하는 것을 권장하고 있습니다.

### 4. 참고자료

- [공식문서](https://developer.android.com/reference/android/os/StrictMode)

- [StrictMode Android Developers](https://www.youtube.com/watch?v=oGrXdxpWgyY&feature=emb_title)

- [StrictMode Google Developers](https://www.youtube.com/watch?v=oGrXdxpWgyY&feature=emb_title)