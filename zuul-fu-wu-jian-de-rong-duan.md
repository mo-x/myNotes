#### 实现ZuulFallbackProvider接口,可以根据服务的不同 实现多种

```java
@Component
public class HelloProducerFallback implements ZuulFallbackProvider {

    @Override
    public String getRoute() {
        //这里如果为具体的服务名 就只熔断该服务
        return "service-hi";
    }

    public ClientHttpResponse fallbackResponse() {
        return new ClientHttpResponse() {
            @Override
            public HttpStatus getStatusCode() throws IOException {
                return HttpStatus.OK;
            }

            @Override
            public int getRawStatusCode() throws IOException {
                return 200;
            }

            @Override
            public String getStatusText() throws IOException {
                return "OK";
            }

            @Override
            public void close() {

            }

            @Override
            public InputStream getBody() throws IOException {
                JSONObject r = new JSONObject();
                r.put("state", "8888");
                r.put("msg", "系统错误，请求失败");
                return new ByteArrayInputStream(r.toJSONString().getBytes("UTF-8"));
            }
            
            @Override
            public HttpHeaders getHeaders() {
                HttpHeaders headers = new HttpHeaders();
                headers.setContentType(MediaType.APPLICATION_JSON);
                return headers;
            }
        };
    }
```



