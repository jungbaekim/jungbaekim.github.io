---
layout: post
title:  "Android 개통여부 확인"
date:   2020-02-28 11:35:26 +0900
categories: android
---

# 안드로이드 개통여부 확인


모바일 개발을 하다보면 특수한 상황에 따라 개통된 폰인지 확인해야 하는 경우가 있다  
단순 개통여부만 확인 해야 한다면 아래와 같이 사용할 수 있다

```kotlin
fun isActivatePhone(context: Context) : Boolean {
  val tm = context.getSystemService(Context.TELEPHONY_SERVICE) as TelephonyManager
  if (tm.simState == TelephonyManager.SIM_STATE_ABSENT) {
    //USIM 확인 안됨
    return false
  }

  val com = tm.simOperator
  if (com == null || com.length <= 0) {
    // 통신사 조회 안됨
    return false
  }
  return true
}
```

위 코드는 AndroidManifest.xml 파일에서 권한을 요청할 필요가 없어 간단하다.
