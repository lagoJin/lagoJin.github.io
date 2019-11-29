---
title: Retrofit2 + Okhttp란 ?
last_modified_at: '2019-11-28 14:09:00 +0300'
published : true
excerpt: 소개해드릴것은 kotlin에서 sqaure사의 okhttp + retrofit2의 사용법입니다.
comments : true
tags :
    - Kotlin
    - retrofit2
    - OkHttpClient
    - Square
---


### okhttp

- 최신 http통신에 대해 간편히 하고 데이터와 미디어를 교환 하는 방법이며 구성에 도움을 주는 라이브러리이다.

### retorfit2

- Retrofit2는 Square사에서 만든 http통신을 간편하게 만들어주는 라이브러리 입니다.
- 구글 샘플에서도 네트워크 통신에 대한 예제를 보여줄 때는 retrofit2를 이용하는 것을 보면 비공식적으로 인정했다고 생각합니다.

빌더 패턴을 적용하여 특정 값에 대해 설정하기 편하게 라이브러리가 구성되어 있습니다.
``` Kotlin
//로그 설정
val interceptor = HttpLoggingInterceptor().apply {
            level = HttpLoggingInterceptor.Level.BODY
        }
```

//okhttp 생성
``` Kotlin
val client = OkHttpClient.Builder().addInterceptor(interceptor as HttpLoggingInterceptor).build()

//retorift2 생성
Retrofit.Builder()
        .baseUrl("url")
        .client(client)
        .addConverterFactory(GsonConverterFactory.create()
        .build()
        .create(GithubAPI::class.java)
```
### Interface

http통신에 요청하는 어노테이션을 이용하여 작성하고, 해당 메소드에 매개변수에 값을 넣어 작성한다.
```Kotlin
interface GithubAPI {

    @GET("/search/users")
    fun getSearchUsers(@Query("q") q : String, @Query("sort") sort: String,
    		@Query("order") order: String): Call<RepoSearchResponse>
    }

```


### dataClass
``` Kotlin
data class RepoSearchResponse(
    val incomplete_results: Boolean,
    val items: List<User>,
    val total_count: Int
)
```

### 사용

아래와 같이 enqueue하여 response와 failure를 override하여 코드를 작성해준다.  응답이 정상적으로 넘어 왔는지에 대한 isSuccessful을 확인하여 해당 데이터를 넘겨준다.(return 형태로도 사용 가능)

``` Kotlin
private val githubAPI: GithubAPI by inject()

githubAPI.getSearchUsers(query, page, perPage).enqueue(
            object : Callback<RepoSearchResponse> {
                override fun onResponse(
                    call: Call<RepoSearchResponse>,
                    response: Response<RepoSearchResponse>
                ) {
                    if (response.isSuccessful) {
                        val users = response.body()?.items ?: emptyList()
                        onSuccess(users)
                    }
                }

                override fun onFailure(call: Call<RepoSearchResponse>, t: Throwable) {
                    Logger.d("fail to get Data")
                    onError(t.message ?: "unknown error")
                }
            }
        )
```

샘플 코드: [깃허브 SearchAPI 샘플](https://github.com/lagoJin/GithubSearch)
