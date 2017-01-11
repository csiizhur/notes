# SpringMvc不使用mvc:annotation-driven
## controller的时间类型参数接收
1. @RequestBody形式，即将参数写进请求体里面，使用application/json这样的的mediaType发请求。应使用Jackson的序列化和反序列化来处理。
2. 表单或者QueryString形式。应使用spring mvc自身的内置日期处理。
### 普通类型请求数据
1.先使用@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")在Controller的方法参数或VO的属性使用.
2.如果 不使用mvc:annotation-driven ,那么使用数据绑定来处理@DateTimeFormat这样的注解.配置例子如下:
```
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
***
### Json类型的请求数据
1.继承定义序列化和反序列化类.例子:
`
public class DateJsonSerializer  extends  JsonSerializer<Date> {  
     public   static   final  SimpleDateFormat format =  new  SimpleDateFormat( "yyyy-MM-dd HH:mm:ss" );  
     @Override   
     public   void  serialize(Date date, JsonGenerator jsonGenerator, SerializerProvider serializerProvider)  throws  IOException, JsonProcessingException {  
        jsonGenerator.writeString(format.format(date));  
    }  
}
`
`
public class  DateJsonDeserializer  extends  JsonDeserializer<Date> {  
     public   static   final  SimpleDateFormat format =  new  SimpleDateFormat( "yyyy-MM-dd HH:mm:ss" );  
     @Override   
     public  Date deserialize(JsonParser jsonParser, DeserializationContext deserializationContext)  throws  IOException, JsonProcessingException {  
         try  {  
             return  format.parse(jsonParser.getText());  
        }  catch  (ParseException e) {  
             throw   new  RuntimeException(e);  
        }  
    }  
}
`  
2.在VO使用@JsonSerialize(using = DateJsonSerializer.class)和@JsonDeserialize(using = DateJsonDeserializer.class)注解(属性或方法都可以,序列化标注在get方法,反序列化标注在set方法).
`
@JsonSerialize(using = DateJsonSerializer.class)
@JsonDeserialize(using = DateJsonDeserializer.class)
private Date releaseDate;
`