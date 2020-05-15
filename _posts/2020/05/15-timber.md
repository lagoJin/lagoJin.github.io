---
title: Crashlytics + Timber?
last_modified_at: '2020-05-15 21:00:00 +0300'
published : true
excerpt: Timber와 Crashlytics를 이용하여 로그를 효율적으로 남기는 방법을 소개합니다.
comments : true
toc : true
toc_sticky: true
tags :
    - Kotlin
    - Android
    - Timber
    - Crashlytics
---

### 1. 개요

Crashlytics 사용하면서, 디버그 상황에서나, 에러 로그에 관해 아쉽다고 생각한 적이 있었다면 정독해주시면 되겠습니다.
Timber 이용하여 효율적으로 앱을 개발하는 방법을 소개합니다.

[Timber](https://github.com/JakeWharton/timber)

[Crashlytics](https://firebase.google.com/docs/crashlytics/?gclid=Cj0KCQjw-_j1BRDkARIsAJcfmTERajKGAjmsdzoQu5vkHzYXcAnA7Nz3bDMnYMOJRW35tKiwuZWCJO0aAmwnEALw_wcB)

### 2. 코드

- 구현

```kotlin
class CrashlyticsTree : Timber.Tree() {

    override fun log(priority: Int, tag: String?,message: String, t: Throwable?) {
        if (priority == Log.VERBOSE || priority == Log.DEBUG || priority == Log.INFO) {
            return
        }

        Crashlytics.setInt(CRASHLYTICS_KEY_PRIORITY, priority)
        Crashlytics.setString(CRASHLYTICS_KEY_TAG, tag)
        Crashlytics.setString(CRASHLYTICS_KEY_MESSAGE, message)

        if (t == null) {
            Crashlytics.logException(Exception(message))
        } else {
            Crashlytics.logException(t)
        }
    }

    companion object {
        private const val CRASHLYTICS_KEY_PRIORITY = "priority"
        private const val CRASHLYTICS_KEY_TAG = "tag"
        private const val CRASHLYTICS_KEY_MESSAGE = "message"
    }
}

```

- 사용방법

```kotlin
class BaseApplication() : Application(){
    override fun onCreate(){
        if(BuildConfig.Debug){
            Timber.plant(Timber.debugTree())
        }else{
            Timber.plant(CrashlyticsTree())
        }
    }
}
```

- 위와 같은 형태로 사용을 하게 되면, Timber.e(throwable) 통하여 에러처리 하였을 때, Debug 상태에서는 Log 창에만 남게 되고, Debug 상태가 아닐 시에는 Crashlytics Report 받아볼 수 있습니다.

### 3. 결론

개발을 하면서 Error Report 관해 고민을 하는 상황이 많았었는데, 위와같은 형태로 사용을 하게 되면 Debug 구분을 하기 때문에 명확하게 Error Report 남길 수 있게 되어 좋다고 판단한다.
