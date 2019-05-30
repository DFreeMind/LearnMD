# Spring Boot

## 常用注解

- `@Controller`：修饰class，用来创建处理http请求的对象
- `@RestController`：Spring4之后加入的注解，原来在`@Controller`中返回json需要`@ResponseBody`来配合，如果直接用`@RestController`替代`@Controller`就不需要再配置`@ResponseBody`，默认返回json格式。
- `@RequestMapping`：配置url映射

### 异常处理

- `@ControllerAdvice`定义统一的异常处理类
- `@ExceptionHandler`用来定义函数针对的异常类型

### 传参获取

- `@PathVariable`
- 除了 `@ModelAttribute` 绑定参数之外，还可以通过 `@RequestParam` 从页面中传递参数 





# Swagger2

## 常用注解

- `@Configuration`注解
- `@EnableSwagger2`注解来启用Swagger2
- `@ApiOperation`注解来给API增加说明
- `@ApiImplicitParams`、`@ApiImplicitParam`注解来给参数增加说明