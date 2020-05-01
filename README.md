### Wechat order system based on springBoot


## Environment：        
1. MySQL. You can download this online installer: [https://dev.mysql.com/downloads/windows/installer/8.0.html](https://dev.mysql.com/downloads/windows/installer/8.0.html), Install MySQL Community Edition.
2. Redis. Download address: [https://github.com/servicestack/redis-windows/tree/master/downloads](https://github.com/servicestack/redis-windows/tree/master/downloads), download the latest version of redis -latest.zip, just unzip it. You can see the redis-server.exe file in the root directory after decompression. Double-click to start the redis server.
3. Nginx. Download address: [http://nginx.org/en/download.html] (http://nginx.org/en/download.html). After downloading the compressed zip package, there is a nginx.exe file in the root directory after decompression. Double-click to start the nginx server.
4. IDEA. Download address: [https://www.jetbrains.com/idea/download/#section=windows](https://www.jetbrains.com/idea/download/#section=windows)
4. JDK1.8 +, maven, IDEA.

> Note: IDEA do not download the Community version, download the Ultimate version.
I use the version of 5.7.21 for the MySQL database. The table-building statement of this project seems to be incompatible with the version of 5.6. It is recommended to install the version above 5.7.
Recommend a relatively easy-to-use MySQL client: [Navicat for MySQL] (https://www.navicat.com/en/download/navicat-for-mysql).
Redis client graphical interface: [Redis Desktop Manager] (https://redisdesktop.com/download). It is better to change the Maven remote warehouse to Alibaba Cloud warehouse. There are introduction and modification methods online, which is very simple.

## How to Run：        
1. Use the command `git clone to clone the project locally.
2. Import the project into IDEA. In IDEA, File-> open ..., and then select the project folder (springboot-project). If you are using spring boot for the first time, this process may take a long time, you need to download many dependent jar packages.
4. Install the lombok plugin for IDEA. In IDEA, File-> Settings ...-> Plugin, search for lombok, install. In the project wiki introduction log, it is mentioned why this plugin is installed.
3. The configuration file of the project is in the resources directory, application.yml file. Modify the MySQL database connection information. My database account passwords are root, 123456, change to yours.
4. Run the sql script of the table building statement in the MySQL database terminal (or use the Navicat for MySQL graphical tool just downloaded), the table building statement of this project is sqmax.sql under the project root path
5. Start redis. In the root directory of Redis just unzipped, double-click redis-server.exe to run the redis service.
6. Finally, you can start the project. Run the main class SellApplication in IDEA in the manner of Spring Boot. It can be seen that this is different from our traditional web project startup method. We have not configured a server such as tomcat, because Spring Boot has introduced the server into the starting dependency.
7. After the above steps, our project should be ready to start. Visit: `http: //127.0.0.1: 8080 / sell / seller / product / list`, you can come to our seller-side merchant management system interface. The effect is as follows:

![](https://i.postimg.cc/ZnsmMkWM/PC.png)


## Visit the buyer's front-end interface
1. The front and back ends of the project are completely separated. The code of the buyer's front end is in another warehouse. Use `git clone to download the front end project, where the project root path The dist directory under (vuejs-project) is the compiled code of the front end.
2. Modify the nginx configuration file so that nginx can find the front-end code. There is a nginx.conf file in the conf directory under the nginx root directory, which is the configuration file we want to modify, which has the following paragraph:

```xml
 server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   F:\vuejs-project\dist; #前端资源路径
            index  index.html index.htm;
        }
	location /sell/ {
		proxy_pass http://127.0.0.1:8080/sell/;
	}

```

The above `F: \ vuejs-project \ dist;` should be the dist directory of the front-end project you just git cloned.

4. Double-click nginx.exe to start the nginx server. If it has been started, enter the nginx root directory from the command line and enter `nginx -s reload` to restart the nginx server.
5. Browser access: `http: //127.0.0.1 / # / order /`, this is a blank interface, press F2 to open the browser ’s developer tools, enter `document.cookie = in the browser console 'openid = abc123'`
Add cookies to the domain name. Visit again: `http: // 127.0.0.1`, then you can access the front-end interface. as follows:

![](https://i.postimg.cc/MG0S8fcR/weixin.png)

6. For the internal access of the WeChat public account on the mobile terminal, an intranet penetration tool is also used. Since the WeChat cannot directly access the ip address, the domain name is also purchased, and it involves very complicated WeChat debugging. It will not be introduced here. You can use postman to simulate WeChat ordering. For the access interface, see the class beginning with Buyer under the controller package.
7. If you want to check the access effect of WeChat, you can visit this link on the WeChat client: `http: // sell.springboot.cn /`. (Note that this is a demo of the project that Brother has launched)
If you use a computer to access, you can first visit: [http://sell.springboot.cn/#/order/](http://sell.springboot.cn/#/order/);
Then, press `F12` to open the developer tools of the browser, click on the console, and enter in the console:` document.cookie = 'openid = abc123'`;
Then revisit: [http://sell.springboot.cn] (http://sell.springboot.cn), you can see the front-end effect.
