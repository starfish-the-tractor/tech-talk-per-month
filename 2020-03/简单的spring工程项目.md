#简单的前后端与数据库的交互实例

----------

###本文章旨在让读者能快速上手使用springboot和微信开发者工具开发微信小程序中简单的能与数据库交互的接口

##开发涉及到的工具及环境

+ ###IDEA

+ ###JDK&JRE

+ ###微信开发者工具（建议选择稳定版）
  ###[微信开发者工具下载地址](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html "微信开发者工具")
+ ###Tips:如果后期想要走小程序的方向的话建议阅读下微信小程序开放文档
 ###[微信小程序开放文档](https://developers.weixin.qq.com/miniprogram/dev/framework/)

+ ###Mysql
 ###[Mysql安装及教程](https://www.runoob.com/mysql/mysql-tutorial.html)

##本文实现的接口及大概的效果

	1. queryAllTypes:在数据库中找到所有的商品类型
	2. queryGoodsByTypeId:根据类型罗列出商品

###效果图：
	
![效果图](http://49.234.9.100/666.png)

##配置数据库以及创建表

###略

##后端实现接口

1. ###新建一个MAVEN项目

2. ###配置POM.XML文件导入项目所需的依赖

3. ###逆向工程生成Mybatis所需的java代码

   + ###[逆向工程代码](https://github.com/qwe542521360/Mybatis-Reverse)

   + ###逆向工程使用说明在README.md里

4. ###配置application.properties
		server.port=8080
		server.tomcat.uri-encoding=UTF-8
		# 配置数据源相关 使用 hikaricp 数据源
		spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
		spring.datasource.url=jdbc:mysql://localhost:3306/beehive?useUnicode=true&serverTimezone=UTC&characterEncoding=utf-8&useSSL=false&allowPublicKeyRetrieval=true
		spring.datasource.username=root
		spring.datasource.password=root
		spring.datasource.type=com.zaxxer.hikari.HikariDataSource
		spring.datasource.hikari.connection-timeout=30000
		
		spring.datasource.hikari.minimum-idle=5
		
		spring.datasource.hikari.maximum-pool-size=15
		
		spring.datasource.hikari.auto-commit=true
		spring.datasource.hikari.idle-timeout=600000
		spring.datasource.hikari.pool-name=DatebookHikariCP
		spring.datasource.hikari.max-lifetime=28740000
		spring.datasource.hikari.connection-test-query=SELECT 1
		
		mybatis.type-aliases-package=com.beehive.pojo
		mybatis.mapper-locations=classpath:mapper/*.xml
		mapper.mappers=com.beehive.utils.MyMapper
		mapper.not-empty=false
		mapper.identity=MYSQL

5. ###编写接口以及其实现
		/*接口*/
		@Service
		public interface QueryAllTypes {
    	List<Types> queryAllTypes();
		}
		/*实现*/
		@Service
		public class QueryAllTypesImplements implements QueryAllTypes {

    	@Autowired
    	private TypesMapper typesMapper;
		@Override
    	public List<Types> queryAllTypes() {
        	return  typesMapper.selectAll();
   			}
		}

6. ###编写控制类
		@RestController
		@RequestMapping("type")
		public class QueryAllTypes {
	
	    @Autowired
	    com.beehive.service.api.QueryAllTypes queryAllTypes;
	
	    @GetMapping("/all")
	    public NEXTJSONResult queryAllTypes(){
	        List<Types> typesList = queryAllTypes.queryAllTypes();
	        return NEXTJSONResult.ok(typesList);
	    }
		}
7. ###部署完成   

##前端向后端发送请求

    wx.request({
      url: app.serverUrl + '/type/all', //serverUrl见 json文件
      header: {
        'content-type': 'application/json' // 默认值
      },

+ ###如果仅在本地测试或者网站没有申请备案或者没有升级HTTPS协议的话记得勾选详情-不校验合法域名

+ ###serverUrl：如果是在本地测试，那么这个是内网ip，内网IP查询方式：cmd->输入ipconfig->找到IPv4地址，如果是部署在有备案域名解析的服务器上那就是域名。 


  
	