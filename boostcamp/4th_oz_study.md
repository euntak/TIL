# Spring Exception


## Spring 3.2 기준으로..

### 3.2 Before

`HandlerExceptionResolver` or the `@ExceptionHandler` annotation. 
Both of these have some clear downsides.

### 3.2 After

we now have the new @ControllerAdvice annotation to address the limitations of the previous two solutions.

## Controller Level

The Controller level `@ExceptionHandler`

```java
public class FooController{
     
    //...
    @ExceptionHandler({ CustomException1.class, CustomException2.class })
    public void handleException() {
        //
    }
}
```

### drawBack

the `@ExceptionHandler` annotated method is **only active for that particular Controller**, not globally for the entire application. Of course adding this to every controller makes it not well suited for a general exception handling mechanism.


## New Solution 3 – The New @ControllerAdvice (Spring 3.2 And Above)

Spring 3.2 brings support for a global @ExceptionHandler with the new @ControllerAdvice annotation. This enables a mechanism that breaks away from the older MVC model and makes use of ResponseEntity along with the type safety and flexibility of @ExceptionHandler:

```java
@ControllerAdvice
public class RestResponseEntityExceptionHandler extends ResponseEntityExceptionHandler {
 
    @ExceptionHandler(value = { IllegalArgumentException.class, IllegalStateException.class })
    protected ResponseEntity<Object> handleConflict(RuntimeException ex, WebRequest request) {
        String bodyOfResponse = "This should be application specific";
        return handleExceptionInternal(ex, bodyOfResponse, 
          new HttpHeaders(), HttpStatus.CONFLICT, request);
    }
}
```

http://springboot.tistory.com/25

http://bigzero37.tistory.com/25


예외 처리 가이드 : http://www.slideshare.net/dhrim/ss-2804901

예외와 로그 코딩 실용 가이드 : http://www.slideshare.net/dhrim/exception-log-practicalcodingguide

https://groups.google.com/forum/#!topic/ksug/e7r1XySpJDw

http://joont.tistory.com/157


http://www.baeldung.com/exception-handling-for-rest-with-spring