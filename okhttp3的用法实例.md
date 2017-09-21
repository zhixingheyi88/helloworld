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
【3】下面是请求文件下载的servlet中的代码
public class UserfilesDownloadServlet extends HttpServlet {

	private static final long serialVersionUID = 1L;
	private Logger logger = LoggerFactory.getLogger(getClass());

	public void fileOutputStream(HttpServletRequest req, HttpServletResponse resp) 
			throws ServletException, IOException {
		String filepath = req.getRequestURI();	
		String url=Global.getConfig("appRequestProject")+filepath;
		System.out.println("url:::::::::"+url);
		OkHttpClient okhttpClient=new OkHttpClient();
		// 创建表单构建器  
	       FormBody.Builder builder = new FormBody.Builder();  
	      // 创建请求体，通过构建器请求体  
	      RequestBody body = builder.build();  
	      Request request = new Request.Builder().url(url).post(body).build();  
		
		try {
			 Response response = okhttpClient.newCall(request).execute();  
			FileCopyUtils.copy(response.body().byteStream(), resp.getOutputStream());
			resp.setHeader("Content-Type", "application/octet-stream");
			return;
		} catch (Exception e) {
			/*req.setAttribute("exception", new FileNotFoundException("请求的文件不存在"));
			req.getRequestDispatcher("/WEB-INF/views/error/404.jsp").forward(req, resp);*/
			resp.getWriter().append("无法加载文件");
		}
	}

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		fileOutputStream(req, resp);
	}

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		fileOutputStream(req, resp);
	}

