# scda
#微服务单工程框架
#目录结构
#apps 应用 dav+antd nmp管理
#microservices 微服务 Spring boot+cloud maven管理
    sc-common 公共服务（框架）  
    sc-security 认证授权模块（框架）  
    sc-modules 业务模块（业务工程）
        demo 示例工程  

#本地开发
    1.复制demo示例工程
    2.修改resource下的配置文件

#请求数据验证
    1. 在实体类中直接加注解，例如  
        @NotBlank(message = "编码不能为空")  
        @IsNumber(message = "编码必须为数字")  
        private String code;  
    2. 在对应的restful方法上加上注解，校验结果会在bindingResult中，例如  
    public ResponseVo add(@Valid @RequestBody DemoVo demoVo, BindingResult bindingResult)  
    3. 自定义数据验证规则，上图 @IsNumber(message = "编码必须为数字")就是自定义的验证规则，具体参考scda\micro-services\sc-common\src\main\java\com\scda\common\valid目录
      
#日志Slf4j使用
    1. 在类上面添加注解@Slf4j  
    2. 在该类内的任意地方即可使用log对象  
#cache缓存使用（基于redis）
    1.开启缓存（默认是开启）  
        micro-services\sc-common\src\main\java\com\scda\common\config\CacheConfig.java  
        中通过是否注释掉@EnableCaching来控制  
    2.使用  
        2.1 @Cacheable 读取，如果缓存没有就执行业务代码（默认是结果非空就放到缓存），如果有就直接缓存取  
        2.2 @CachePut 手动更新，手动触发更新缓存内容  
        2.3 @CacheEvict 手动删除  
    3.注解常用参数说明  
        cacheNames或value 表示命名空间,最终的redis key=命名空间:缓存key  
        key 缓存key  
            支持EL表达式，使用方法参数时我们可以直接使用“#参数名”或者“#p参数index”,例如参数只有id，key="#id"或key="#p0"  
            提供root对象   
        condition 参数满足条件才放到缓存  
            支持EL表达式  
         unless 结果满足条件不放到缓存  
            支持EL表达式   
        具体详情参见https://docs.spring.io/spring/docs/4.3.22.RELEASE/spring-framework-reference/htmlsingle/#cache  
#动态多数据源使用和关闭
    1. 关闭（默认）  
        注释掉micro-services\sc-common\src\main\java\com\scda\common\db\dyndata\DynDataSourceRegister.java类中的  
        //@Configuration  
        注释掉micro-services\sc-common\src\main\java\com\scda\common\db\dyndata\DynDataSourceAspect.java类中的  
        //@Aspect  
        //@Component  
        //@Order(1)  
    2. 放开关闭中提到的注解  
    3. 默认是支持两个数据源的，如果需要多个可以在micro-services\sc-common\src\main\java\com\scda\common\db\dyndata\DynDataSourceAspect.java类中的修改doBefore方法  
    4. 以上方式可以用mycat中间件方案来统一管理，优点与编码解耦  


#认证授权模块sc-security
    使用说明见micro-services\sc-security\readme.md  