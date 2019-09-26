1.Django官方网站

http://www.djangoproject.com   使用版本1.11.7   LTS：长期支持版本

# Django创建项目的流程

1. 首先是创建一个虚拟环境：（到虚拟环境配置中看）

2. 安装django：

   ```
   pip install django
   	pip install django==1.11.7   一定要使用==
   	豆瓣源下载：pip install django==1.11.7 -i https://pypi.douban.com/simple
   
   查看django是否安装成功
   	pip freeze
   	pip list
   	进入到python环境中
   		import django
   		django.get-version()
   ```

3. 创建django项目

   ```
   终端中创建
   django-admin startproject 项目名字
   		tree命令观察项目结构
   			如果未安装  sudo apt install tree
   ```

   ```
   pycharm中创建
   ```

4. 启动服务器的命令

   ```
   python manage.py runserver
   ```

5. 项目的结构
   ![1569329388596](C:\Users\miyongqing\AppData\Roaming\Typora\typora-user-images\1569329388596.png)

   6.修改虚拟环境的步骤

   file -- setting --project interpreter  --add --exist environment--

   虚拟环境的文件夹 --bin  --python3.6

   7.修改欢迎页面为中文

   ​	在settings中修改LANGUAGE_CODE='zh-hans'

   8.修改为当前系统时间

   ​	在settings中修改TIME_ZONE='Asia/Shanghai'

   9.允许所有主机访问

   ​	在settings 中修改ALLOWED_HOSTS = ['*']

   10.启动服务器设置端口号和主机

   ​		python manage.py runserver

   ​		python manage.py runserver 9000

   ​		python manage.py runserver 0.0.0.0:9000

   11.视图函数的位置

   flask的视图函数是这样

   @blue.route('/index/')

   def index():

   ​	return 'index'

   ​	（1）项目名字下的视图函数

   ​		在urls定义路由路径

   ​			url(r'^路径/',views.视图函数名字不加圆括号)

   ​			在views中定义视图函数

   ​				def index(request):

   ​					return HttpResponse('index')

   ​		如果把所有的视图函数都放在项目下，那么代码看起来就特别的雍肿，所以我们一般的企业级开发 都是把每一个模块封装出一个app

   ​	（2）App

   ​	1.创建App

   ​			django-admin startapp App     第二种/python manage.py startapp App

   ​	2.在App中创建urls

   ​	3.在urls创建urlpatterns = [] 我们称作app下的urls叫做子路由
   
   ​	4、在跟路由(项目下的urls)加载子路由  
   
   ​					url(r'^app/',include('App.urls'))		
   
   ​	5、在子路由中定义定义请求资源路径也就是路由
   
   ​		\```
   ​		url(r'^index/',views.index)
   ​		\```
   
   ​	6、在App下的views下创建视图函数
   
   ​		\```
   ​		def index(request):
   ​		return HttpResponse('index')
   ​		\```
   
   ​	7、访问127.0.0.1：8000/根路由的名字/子路由的名字	
   
   ​					127.0.0.1：8000/app/index  

