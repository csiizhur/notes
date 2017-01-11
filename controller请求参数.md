#SpringMvc不使用mvc:annotation-driven
##controller的时间类型参数接收
1.@RequestBody形式，即将参数写进请求体里面，使用application/json这样的的mediaType发请求。应使用Jackson的序列化和反序列化来处理。
2.表单或者QueryString形式。应使用spring mvc自身的内置日期处理。
###普通类型请求数据
1.先使用@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")在Controller的方法参数或VO的属性使用.
2.如果 不使用mvc:annotation-driven ,那么使用数据绑定来处理@DateTimeFormat这样的注解.配置例子如下:
```xml
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>   
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">   
	<property name="webBindingInitializer" >   
		<bean class="org.springframework.web.bind.support.ConfigurableWebBindingInitializer">   
			<property name="conversionService" ref="conversionService"/>   
		</bean>   
	</property>   
	<property name="messageConverters">   
		<list>   
			<bean id="stringHttpMessageConverter" class = "org.springframework.http.converter.StringHttpMessageConverter"/>   
			<bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">   
				<property name="supportedMediaTypes">   
					<list>   
						<value>application/json;charset=UTF-8</value>   
						<value>text/html;charset=UTF-8</value>   
					</list>   
				</property>   
			</bean>   
		</list>   
	</property>   
</bean>   
<bean id="conversionService" class="org.springframework.format.support.DefaultFormattingConversionService"/>
```
###Json类型的请求数据
* 可以扩展 JsonSerializer，来实现一个格式化时间的DateJsonSerializer，并在注解中引用这个类
1.继承定义序列化和反序列化类.例子:
```java
public class DateJsonSerializer  extends  JsonSerializer<Date> {  
     public static final SimpleDateFormat format = new SimpleDateFormat( "yyyy-MM-dd HH:mm:ss" );  
     @Override   
     public void serialize(Date date, JsonGenerator jsonGenerator, SerializerProvider serializerProvider)  throws  IOException, JsonProcessingException {  
        jsonGenerator.writeString(format.format(date));  
    }  
}
```
```java
public class DateJsonDeserializer  extends  JsonDeserializer<Date> {  
     public static final SimpleDateFormat format = new SimpleDateFormat( "yyyy-MM-dd HH:mm:ss" );  
     @Override   
     public Date deserialize(JsonParser jsonParser, DeserializationContext deserializationContext)  throws  IOException, JsonProcessingException {  
         try  {  
             return format.parse(jsonParser.getText());  
        }  catch  (ParseException e) {  
             throw new RuntimeException(e);  
        }  
    }  
}
* 在Jackson2中增加的注解 @JsonFormat 可以方便的格式化时间字段。
```
2.在VO使用@JsonSerialize(using = DateJsonSerializer.class)和@JsonDeserialize(using = DateJsonDeserializer.class)注解(属性或方法都可以,序列化标注在get方法,反序列化标注在set方法).
用于序列化和反序列化的注解类：@JsonSerialize和@JsonDeserialize
```java
//注意：该类必须实现 java.io.Serializable
@JsonSerialize(using = DateJsonSerializer.class)
@JsonDeserialize(using = DateJsonDeserializer.class)
private Date releaseDate;
```
##SpringBoot controller接收时间类型无法映射到Date类型（时区）
* spring 容器在启动的时候会把映射转化注册到容器里面。随着容器的启动而生效。有时候会缺少我们所需要的映射这样的话我们就需要自己给容器添加一个bean 来完成我们自己的映射 :
```java
@Bean
public Converter<String, Date> addNewConvert() {
	return new Converter<String, Date>() {
    	@Override
        public Date convert(String source) {
        	SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
            Date date = null;
            try {
            	date = sdf.parse((String) source);
            } catch (ParseException e) {
                e.printStackTrace();
            }
            return date;
    	}
    };
}
```
* SpringBoot controller接收时间类型2
	自定义转换器Convert
```java
public class TimestampConverter implements Converter<String, Timestamp> {
    @Override
    public Timestamp convert(String s){
        Timestamp ts = new Timestamp(System.currentTimeMillis());
        try {
            ts = Timestamp.valueOf(s);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return ts;
    }
}
```
注册到spring管理中
```java
@Configuration
public class WebMvcConfig{

    @Autowired
    private RequestMappingHandlerAdapter handlerAdapter;

    /**
     * 增加字符串转日期的功能
     */
    @PostConstruct
    public void initEditableValidation() {
        ConfigurableWebBindingInitializer initializer = (ConfigurableWebBindingInitializer) handlerAdapter
                .getWebBindingInitializer();
        if (initializer.getConversionService() != null) {
            GenericConversionService genericConversionService = (GenericConversionService) initializer
                    .getConversionService();
            genericConversionService.addConverter(new TimestampConverter());
        }

    }
}
```
##返回Jackson数据
* 默认情况下Jackson将 Java.util.Date 序列化为 epoch timestamp，并且时区使用的是 GMT标准时间，而非本地时区。
* 在使用该架构的时候 我们发现有个8小时的时间差。解决方案  在 application.properties 文件里面添加  
```properties
	spring.jackson.time-zone=GMT+8
```
* 如果 从controller  返回出来的时间数据需要直接成 固定的String 格式 需要在application.properties 添加如下配置
```properties
	spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
```

##使用Jackson的@JsonFormat注解时出现少一天
```java
@JsonFormat(shape = JsonFormat.Shape.STRING,pattern="yyyy-MM-dd",timezone="GMT+8")
public Date getRegistDate() {
	return this.registDate;
}
```
加上时区即可,中国是东八区

##FastJson中@JSONField注解使用
* FastJson在进行操作时，是根据getter和setter的方法进行的，并不是依据Field进行
* 当作用在setter方法上时，就相当于根据 name 到 json中寻找对应的值，并调用该setter对象赋值。
当作用在getter上时，在bean转换为json时，其key值为name定义的值。实例如下：
```java
public class Product {  
  
    private String productName;  
    private String desc;  
    private String price;  
      
    @JSONField(name="name")  
    public String getProductName() {  
        return productName;  
    }  
      
    @JSONField(name="NAME")  
    public void setProductName(String productName) {  
        this.productName = productName;  
    }  
      
    @JSONField(name="desc")  
    public String getDesc() {  
        return desc;  
    }  
      
    @JSONField(name="DESC")  
    public void setDesc(String desc) {  
        this.desc = desc;  
    }  
      
    @JSONField(name="price")  
    public String getPrice() {  
        return price;  
    }  
      
    @JSONField(name="PRICE")  
    public void setPrice(String price) {  
        this.price = price;  
    }  
      
    public String toString() {  
        return JSONObject.toJSONString(this);  
    }  
      
}
```
测试代码：
```java
@Test  
    public void test() {  
        Product product = new Product();  
        product.setProductName("产品");  
        product.setDesc("这是一个产品");  
        product.setPrice("22.3");  
          
        String jsonStr = JSONObject.toJSONString(product);  
        System.out.println("转换为json:" + JSONObject.toJSONString(product));  
          
        jsonStr = jsonStr.toUpperCase();  
        System.out.println(jsonStr);  
          
        product = JSONObject.toJavaObject(JSONObject.parseObject(jsonStr), Product.class);  
        System.out.println(product.toString());  
    }
```
输出：
```java
转换为json:{"desc":"这是一个产品","name":"产品","price":"22.3"}  
{"DESC":"这是一个产品","NAME":"产品","PRICE":"22.3"}  
{"desc":"这是一个产品","name":"产品","price":"22.3"}  
```
