### 6. 用户-注册-控制器层

**(a) 统一异常处理**

创建`cn.tedu.store.controller.BaseController`，作为所有控制器类的父类，并在其中添加统一异常处理的方法。

	public abstract class BaseController{
	
		@ExceptionHandler(ServiceException.class)
		@ResponseBody
		public JsonResult handleException(Throwable e){
			JsonResult<void> jr=new JsonResult();
			
			if(e instanceof UsernameDuplicateException){
				jr.setState(1);
				jr.setMessage(e1.getMessage());
			}eles if(e instanceof InsertException){
				jr.setState(2);
				jr.setMessage(e1.getMessage());
			}
	
			return jr;
		}
	}


**(b) 设计请求**

设计“用户注册”的请求方式。

	请求路径：/users/reg
	请求参数：User user
	请求方式：POST
	响应数据：JsonResult<Void>

**(c) 实现请求**

首先，开发`cn.tedu.store.util.JsonResult`类，封装服务器给浏览器的响应数据。


	public class JsonResult<T>{
		
		private Integer state;
		private String message;
		private T data;
	
	}

开发`cn.tedu.store.controller.UserController`类，响应用户的请求，继承`cn.tedu.store.controller.BaseController`

	@RestController
	@RequestMapping("users")
	public class UserController extends BaseController{
	
		@Autowired
		private IUserService service;
	
		@RequestMapping("reg")
		public JsonResult<Void> reg(User user){
			JsonResult<Void> jr=new JsonResult();
			service.reg(user);
			jr.setState(0);
			return jr;
		}
	} 

完成后，通过**启动类**启动当前项目，之后在浏览器上通过`http://localhost:8080/users/reg?username=boot&password=1234`来进行测试。

### 7. 用户-注册-前端界面

从`http://doc.tedu.cn`中下载**学子商城v2**的html内容，解压缩后，将5个文件夹拷贝到**store**项目的`java/main/resources`目录下的`static`文件夹中。

在`web/register.html`页面中添加JS的代码，基于jQuery向服务器发送AJAX请求，进行用户注册。

注意：JS的内容与页面表单组件的id应该匹配

测试：注册一个不存在的用户，查看是否能注册成功，成功后再次注册，查看是否正确提示错误信息。

