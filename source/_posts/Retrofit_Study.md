title: Retrofit2 post 请求发送 json数据
comments: true
permalink: hiperioncn.github.io
date: 2016-05-05 18:43:29
tags: Retrofit
categories: Android
---

先上代码，定义service
```
public interface TestApi {
    //第一种方式 注意此处使用的是Gson的JsonObject对象，而非系统自带JSONObject对象
    @POST("auth/login/")
    Call<JsonObject> userLogin(@Body JsonObject body );
    //第二种方式
    @POST("auth/login/")
    Call<JsonObject> userLogin(@Body User user );
}
```

定义数据传输用User对象
```
class User {
      final String username;
      final String password;

      User(String username, String password) {
          this.username = username;
          this.password = password;
      }
}
```
定义拦截器，实现log打印，由于官方未实现log打印，所以可用okhttp的拦截器实现
```
HttpLoggingInterceptor interceptor = new HttpLoggingInterceptor();
        interceptor.setLevel(HttpLoggingInterceptor.Level.BODY);
```
自定义GsonConverterFactory的相关转换规则
```
Gson gson = new GsonBuilder()
        .setDateFormat("yyyy-MM-dd'T'HH:mm:ssZ")
        .create();
```
创建OkHttpClient对象
```
OkHttpClient client = new OkHttpClient.Builder().addInterceptor(interceptor).build();

```
创建Retrofit对象
```
String baseUrl ="http://192.168.67.6:8000/";
Retrofit retrofit = new Retrofit.Builder()
        .baseUrl(baseUrl)
        .addConverterFactory(GsonConverterFactory.create(gson))
        .client(client)
        .build();
```
使用retrofit创建相应service服务
```
TestApi testApi = retrofit.create(TestApi.class);
```

使用第一种方式的创建Call对象
```
String username ="Hiperion";
String password ="123456";
JsonObject reqJsonObject = new JsonObject();
reqJsonObject.addProperty("username", username);
reqJsonObject.addProperty("password", password);
Call<JsonObject> call = testApi.userLogin(reqJsonObject);
```
使用第二种方式的创建Call对象
```
String username ="Hiperion";
String password ="123456";
User user = new User(username,password);
Call<JsonObject> call = testApi.userLogin(user);
```
异步执行Post请求，并打印结果
```
call.enqueue(new Callback<JsonObject>() {
            @Override
            public void onResponse(Call<JsonObject> call, Response<JsonObject> response) {
                oLog.d("onResponse:" + response.isSuccessful());
                oLog.d("url:" + response.raw().request().url());
                oLog.d("message:" + response.message());
                oLog.d("headers:" + response.headers().toString());
            }
            @Override
            public void onFailure(Call<JsonObject> call, Throwable t) {
                t.printStackTrace();
                oLog.d("onFailure" + t.getMessage());
            }
        });

```
其它相关资料链接</br>
[Future Studio](https://futurestud.io/blog/retrofit-send-objects-in-request-body)
