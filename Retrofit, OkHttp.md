# Retrofit

Retrofit is an HTTP client for Java and Android allowing us to interact with web services.

#### Advantages of Retrofit

1. Easy troubleshooting
2. Less boiler plate code
3. Manages resources efficiently like background threads, async calls and queues, automatic JSON parsing using Gson and
   other libraries, automatic error handling callbacks & built in user authentication support.

<br/>

#### Retrofit Interface

It is used to define end points using special retrofit annotations.

```kotlin
interface ApiService {

    @GET("users/{username}")
    suspend fun getUser(@Path("username") username: String): User

    @GET("group/{id}/users")
    suspend fun groupList(@Path("id") groupId: Int, @Query("sort") sort: String): List<User>

    @POST("users/new")
    suspend fun createUser(@Body user: User): User
}
```

Notice that each endpoint specifies an annotation of the HTTP method (GET, POST, etc.) and method that will be used to
dispatch the network call. Note that the parameters of this method can also have special annotations:

| Annotation | Description                                                                                                    |
|------------|----------------------------------------------------------------------------------------------------------------|
| @Path      | variable substitution for the API endpoint (i.e. username will be swapped for {username} in the URL endpoint). |
| @Query     | specifies the query key name with the value of the annotated parameter.                                        |
| @Body      | payload for the POST call (serialized from a Java object to a JSON string)                                     |
| @Header    | specifies the header with the value of the annotated parameter                                                 |

<br/>

#### Changing the base URL

Normally, the base URL is defined when you instantiated an Retrofit instance. Retrofit 2 allows you to override the base
URL specified by changing it in the annotation (i.e. if you need to keep one specific endpoint using an older API
endpoint)

```kotlin
@POST("https://api.github.com/api/v3")
```

There are also others that allow you to modify the base URL using relative paths (and not the fully qualified URL) as
discussed in this blog article.

<br/>

#### Adding headers

Notice that there is a `@Headers` and `@Header` annotation. The Headers can be used to provide pre-defined ones:

```kotlin
@Headers({ "Cache-Control: max-age=640000", "User-Agent: My-App-Name" })
@GET("/some/endpoint")
```

We can also add headers as a parameter to the endpoint:

```kotlin
@Multipart
@POST("/some/endpoint")
suspend fun someEndpoint(@Header("Cache-Control") maxAge: Int): SomeResponse
```

<br/>

#### Form data

If we wish to submit form-encoded data, we can use the FormUrlEncoded annotation. The `@Field` annotation will denote
what
payload will be submitted as form data.

```kotlin
@FormUrlEncoded
@POST("/some/endpoint")
suspend fun someEndpoint(@Field("code") code: String): SomeResponse
```

<br/>

#### Multipart forms

If we need to upload images or files, we need to send by using Multipart forms. We will to mark the endpoint with
`@Multipart`, and label at least one parameter with `@Part`.

```kotlin
@Multipart
@POST("some/endpoint")
suspend fun uploadImage(@Part("description") description: String, @Part("image") image: RequestBody): Response
```

<br/>

#### Retrofit Builder

The Retrofit builder generates an implementation of the GitHubService interface.

```kotlin
Retrofit.Builder()
    .baseUrl("https://www.example.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .build()


val apiService: ApiService = retrofit.create(ApiService::class.java) // here ApiService is interface
```

In the above example convertor factory used to automatically convert API response into the model class.

<br/>

#### OkHttp Interceptors

Interceptors are a powerful mechanism that can monitor, rewrite, and retry the API call. So basically, when we do some
API call, we can monitor the call or perform some tasks.

There are predefined interceptors like `HttpLoggingInterceptor` which logs request and response.

```kotlin
val okHttpClient = OkHttpClient.Builder()
    .connectTimeout(60, TimeUnit.SECONDS)
    .readTimeout(60, TimeUnit.SECONDS)
    .writeTimeout(60, TimeUnit.SECONDS)
    .addInterceptor(HttpLoggingInterceptor().setLevel(HttpLoggingInterceptor.Level.BODY))
    .build()

Retrofit.Builder()
    .baseUrl("https://www.example.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .client(okHttpClient)
    .build()


val apiService: ApiService = retrofit.create(ApiService::class.java) // here ApiService is interface
```

<br/>

#### Custom Interceptors

To create the interceptor, we need to create a class by implementing the `Interceptor` interface like below:

```kotlin
class MyInterceptor : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        /**
         * Our API Call will be intercepted here
         */
    }
}
```

Here, in the `intercept()`, we can perform any action which we want inside it.

And to use the interceptor, we can use like below:

```kotlin
val builder = OkHttpClient().newBuilder()
    .addInterceptor(MyInterceptor())
return builder.build()
```