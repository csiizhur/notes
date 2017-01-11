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
```
2.在VO使用@JsonSerialize(using = DateJsonSerializer.class)和@JsonDeserialize(using = DateJsonDeserializer.class)注解(属性或方法都可以,序列化标注在get方法,反序列化标注在set方法).
```java
@JsonSerialize(using = DateJsonSerializer.class)
@JsonDeserialize(using = DateJsonDeserializer.class)
private Date releaseDate;
```
##SpringBoot controller接收时间类型无法映射到Date类型
*spring 容器在启动的时候会把映射转化注册到容器里面。随着容器的启动而生效。有时候 会缺少我们所需要的映射这样的话我们就需要自己给容器添加一个bean 来完成我们自己的映射 :
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
##返回json数据
*在使用该架构的时候 我们发现有个8小时的时间差。
   解决方案  在 application.properties 文件里面添加  
```properties
spring.jackson.time-zone=GMT+8
```
   如果 从controller  返回出来的时间数据需要直接成 固定的String 格式 需要在application.properties 添加如下配置
```properties
   spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
```