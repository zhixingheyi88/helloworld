【1】
##导入##

1、自己到入jar包，别漏了okio：

okhttp-3.3.0.jar
okio-1.8.0.jar

2、maven方式：
<dependency>
  <groupId>com.squareup.okhttp3</groupId>
  <artifactId>okhttp</artifactId>
  <version>3.3.0</version>
</dependency>

3、gradle方式：

compile 'com.squareup.okhttp3:okhttp:3.3.0'

【2】
public static void main(String[] args) {
    
		String url="https://www.baidu.com";
		OkHttpClient okhttpClient=new OkHttpClient();
	///////////////////////////////////////////////////////////////////////////////	
	/*	Get请求
	 * Request request=new Request.Builder()
				.url(url)
				.build();
		Call call=okhttpClient.newCall(request);
	    try {
			Response response=	call.execute();
			System.out.println("return::::"+response.body().string());
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}*/
		///////////////////////////////////////////////////////////////////////////////////////////
		/*//Post请求
		RequestBody body=new FormBody.Builder()
				.add("test", "test")
				.build();
		
		Request request=new Request.Builder()
				.url(url)
				.post(body)
				.build();
		Call call=okhttpClient.newCall(request);
	    try {
			Response response=	call.execute();
			System.out.println("return::::"+response.body().string());
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}*/
		////////////////////////////////////////////////////////////////////////
		//下面是已文件的形式进行请求传递
		File file=new File("");
		RequestBody requestBody = new MultipartBody.Builder()
			    .setType(MultipartBody.FORM)			    			    
			    .addFormDataPart("file", file.getName(), RequestBody.create(MediaType.parse("image/png"), file))
			    .build();
		
		Request request=new Request.Builder()
				.url(url)
				.post(requestBody)
				.build();
		
		Call call=okhttpClient.newCall(request);
	    try {
			Response response=	call.execute();
			System.out.println("return::::"+response.body().string());
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
