#如何设置网页标题图标 FavIcon


很多时候我们看到浏览器的选项卡标题前有时会有一个图标，感觉很不错，那么实现的原理就是，在head标签对之间加入<link rel="shortcut icon" href="favicon.ico" /> ，将link rel设置为shortcut icon这个值，然后在href设置好路径就可以了，如favicon.html的代码如下

`<html>  
<head>  
<link rel="shortcut icon" href="favicon.ico"  type="image/x-icon"/>   
<title>Designed By Androidyue</title>  
</head>  
</html>  `


注意有些网页在客户端不能显示该图标，这是放到服务器端运行基本可以显示了！

再次提醒：图标的路径一定要正确

分割线
-----
如何获取其他网站的icon呢，我要做友情链接啊，那用java吧，httpclient4.2版本,jdk1.8

	package mytest;
	
	import org.apache.http.HttpEntity;
	import org.apache.http.HttpResponse;
	import org.apache.http.client.methods.HttpGet;
	import org.apache.http.client.methods.HttpPost;
	import org.apache.http.impl.client.DefaultHttpClient;
	
	import java.io.File;
	import java.io.FileOutputStream;
	import java.io.InputStream;
	
	/**
	 * Created by mike on 15-6-27.
	 */
	public class GetIcon {
	    public static void main(String[] args) {
	        DefaultHttpClient httpClient = new DefaultHttpClient();
	        String url = "http://www.oschina.net/favicon.ico";
	        HttpGet postMethod = new HttpGet(url);
	        String postResult = "";
	        try {
	            HttpResponse response = httpClient.execute(postMethod);
	            HttpEntity entity = response.getEntity();
	            InputStream in = entity.getContent();
	
	            String destFileName = "/home/mike/favicon.ico";
	            File file = new File(destFileName);
	            try {
	                FileOutputStream fout = new FileOutputStream(file);
	                int l = -1;
	                byte[] tmp = new byte[1024];
	                while ((l = in.read(tmp)) != -1) {
	                    fout.write(tmp, 0, l);
	                    // 注意这里如果用OutputStream.write(buff)的话，图片会失真，大家可以试试
	                }
	                fout.flush();
	                fout.close();
	            } finally {
	                // 关闭低层流。
	                in.close();
	                postMethod.abort();
	                httpClient.getConnectionManager().closeExpiredConnections();
	            }
	           // System.out.println(postResult);
	        }catch (Exception e){
	            e.printStackTrace();
	        }
	    }
	}
	


