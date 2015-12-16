##MVC
MVC是Model-View-Controller

- **Model:**表示数据和业务处理
    - 对应的组建是javabean
    - 模型分为业务模型和数据模型
- **View:**用户看到并与之交互的界面
- **Controller:**接受用户的输入并调用模型和视图去完成用户的请求
    - 对应的组件是Servlet


##Spring MVC的处理过程
1. 客户端请求提交到DispatcherServlet
2. DispatcherServlet查询一个或者多个HandlerMapping，找到处理请求的Controller
3. DispatcherServlet将请求提交到Controller
4. Controller调用业务逻辑处理后，返回ModelAndView
5. DispatcherServlet查询一个或者多个ViewResolver视图解析器，找到ModelAndView指定的视图
6. 视图负责将结果显示到客户端

##Spring MVC开发实现步骤
1. 新建web project
2. 添加spring支持
3. 修改web.xml,配置DispatcherServlet
4. 实现Model层
5. 实现Controller层
6. 实现View层
7. 修改applicationContext.xml