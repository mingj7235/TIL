
# π LINUX νκ²½μμ Ajax Patch Method λμ μν¨



## Local νκ²½


- Local νκ²½μμλ Ajax λ₯Ό ν΅ν΄ Patch ν΅μ μ΄ λλ¬΄ μλμλ€.



## DEV μλ² νκ²½ (Linux)


- Local μμ μ λμκ°λ λ‘μ§μ΄ DEV μλ² νκ²½μμ ``not allowed method 'patch'`` λΌλ λ©μΈμ§μ ν¨κ» μλνμ§ μμλ€.

- λ κ³΅λΆν΄λ΄μΌκ² μΌλ μλ Stack overflow μμ ν΄λ΅μ μ°Ύμλ€.

``` java
/**
The $.ajax method does support HTTP PATCH.

The problem you are seeing is that the ajax method looks for PATCH in the Access-Control-Allow-Methods response header of the options preflight check. Either this header is missing from your response, or the PATCH method was not included in the value of this header. In either case, the problem is in the server, not in your client-side code.

Here's an example using Java:
*/

response
    .addHeader("Access-Control-Allow-Methods", "GET, POST, PATCH, PUT, DELETE");

```


> https://stackoverflow.com/questions/13642044/does-the-jquery-ajax-call-support-patch (μ°Έκ³ )





