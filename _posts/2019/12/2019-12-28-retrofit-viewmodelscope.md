---
title: coroutine을 이용한 retrofit 통신 방법
last_modified_at: '2019-12-28 15:40:00 +0300'
published : true
excerpt: coroutine(viewModelScope)을 이용하여 retrofit과 통신하는 샘플입니다.
comments : true
toc : true
toc_sticky: true
tags :
    - Kotlin
    - Android
    - viewModelScope
    - Retrofit
---


viewModelScop이라는 단어를 처음 보신다면 영상을 보고 오시는 걸 추천 드립니다.

> [링크](https://www.youtube.com/watch?v=KMb0Fs8rCRs)<br>
[Sample](https://github.com/lagoJin/AAC_Couroutine_Demo/tree/sample)

2019 DevSummit에서 viewModelScope을 발표합니다.

### 1. ViewModelScope

<script src="https://gist.github.com/lagoJin/63cebf0447557707f4c7166400db52a2.js"></script>

ViewModelScope 구현되어 있는 형태를 보면 SupervisorJob() + Dispatchers.Main으로 구성되어 있습니다.

### 2. Service

<script src="https://gist.github.com/lagoJin/65f99087d0b0d0b07cf8262106047799.js"></script>

위 영상을 보고 coroutine을 이용하여 retrofit을 호출하고 싶다는 생각을 하였는데, retrofit이 2.6(2019.06.05)로 넘어가면서 suspend를 지원하게 됩니다!

이제 위와 같은 형태로 Service를 작성할 수 있게 되었습니다!

단 , 이렇게 호출할 경우 error에 대한 처리를 해주어야 하는 이슈가 발생합니다.

### 3. Repository

<script src="https://gist.github.com/lagoJin/d11d740f71a5fe2dead4333193f86f94.js"/></script>

repositofy패턴을 이용할 경우 아래와 같이 repository를 구성할 때 호출하는 함수에서 suspend를 붙여 coroutine을 사용해야 한다는 것을 명시합니다.

### 4. UseCase

<script src ="https://gist.github.com/lagoJin/eac18960c508c45499890be1f0757b3e.js"/></script>

UseCase을 사용하여 호출하게 되었을 때, Kotlin에서 제공하는 runCatching을 이용하여 성공과 실패에 맞춰 처리를 해줍니다. 다만, Adapter를 쓰지 않기에 사용자가 에러 처리에 관한 부분도 처리 해줘야 합니다.

### 5. ViewModel
<script src ="https://gist.github.com/lagoJin/4b65c3785520d259f9071622b7b95538.js"/></script>

 ViewModel에서 useCase에 존재하는 함수를 호출을 하게 되면 viewModelScope을 호출 함수 파라매터로 넣어 useCase에서 Scope을 이용할 수 있도록 합니다.

Dispatchers를 IO로 설정할 경우, livedata의 값 설정은 postValue을 이용하여야 합니다.

### 6. 결론

Retrofit이 2.6으로 업데이트 되어 suspend를 지원하게 되었는데, 이러한 경우 Adapter를 커스텀하여 만들거나 에러 처리에 관한 부분을 사용자가 만들어야 하는 이슈가 있습니다. 현재 일 기준으로 서비스하는 프로젝트에 도입 하기에는 조금 부족한 느낌이 있습니다. 단, 구글에서 지속적으로 coroutine을 aac에 넣으려고 하는 움직임이 지속적으로 보여 공부하면 좋을 것 같습니다.
