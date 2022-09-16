
# 🔑 LINUX 환경에서 Ajax Patch Method 동작 안함



## Local 환경


- Local 환경에서는 Ajax 를 통해 Patch 통신이 너무 잘되었다.



## DEV 서버 환경 (Linux)


- Local 에서 잘 돌아가던 로직이 DEV 서버 환경에서 ``not allowed method 'patch'`` 라는 메세지와 함께 작동하지 않았다.

- 더 공부해봐야겠으나 아래 Stack overflow 에서 해답을 찾았다.

``` java
/**
The $.ajax method does support HTTP PATCH.

The problem you are seeing is that the ajax method looks for PATCH in the Access-Control-Allow-Methods response header of the options preflight check. Either this header is missing from your response, or the PATCH method was not included in the value of this header. In either case, the problem is in the server, not in your client-side code.

Here's an example using Java:
*/

response
    .addHeader("Access-Control-Allow-Methods", "GET, POST, PATCH, PUT, DELETE");

```


> https://stackoverflow.com/questions/13642044/does-the-jquery-ajax-call-support-patch (참고)





