```java
package com.wh.router;

import com.netflix.zuul.ZuulFilter;
import com.netflix.zuul.context.RequestContext;
import org.apache.commons.lang.StringUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.util.ReflectionUtils;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

public class ErrorFilter extends ZuulFilter {

    private static final String ERROR_STATUS_CODE_KEY = "error.status_code";

    private static Logger log = LoggerFactory.getLogger(ErrorFilter.class);

    public static final String DEFAULT_ERR_MSG = "系统繁忙,请稍后再试";

    @Override
    public String filterType() {
        return "error";
    }

    @Override
    public int filterOrder() {
        return 0;
    }

    @Override
    public boolean shouldFilter() {
        return true;
    }

    @Override
    public Object run() {
        RequestContext ctx = RequestContext.getCurrentContext();
        String message = ctx.getThrowable().getCause().getMessage();
        log.error("zuul-cause: " + message);
        log.error("zuul-serviceId:" + ctx.get("serviceId"));
        HttpServletRequest request = ctx.getRequest();
        try {
            int statusCode = (Integer) ctx.get("responseStatusCode");
            Throwable e = ctx.getThrowable();
            if (e instanceof java.net.ConnectException) {
                message = "连接服务异常";
                log.warn("uri:{},error:{}", request.getRequestURI(), e.getMessage());
            } else if (e instanceof java.net.SocketTimeoutException) {
                message = "连接超时";
                log.warn("uri:{},error:{}", request.getRequestURI(), e.getMessage());
            } else if (e instanceof com.netflix.client.ClientException) {
                message = e.getMessage();
                log.warn("uri:{},error:{}", request.getRequestURI(), e.getMessage());
            } else {
                log.warn("Error during filtering", e);
            }
            if (StringUtils.isBlank(message)) message = DEFAULT_ERR_MSG;
            request.setAttribute("statusCode", statusCode);
            request.setAttribute("msg", message);
        } catch (Exception e) {
            String error = "Error during filtering[ErrorFilter]";
            log.error(error, e);
        }
        RequestDispatcher dispatcher = request.getRequestDispatcher("/error");
        if (dispatcher != null) {
            if (!ctx.getResponse().isCommitted()) {
//          ctx.setResponseStatusCode(ze.getErrorCode());
                try {
                    dispatcher.forward(ctx.getRequest(), ctx.getResponse());
                } catch (ServletException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                    ReflectionUtils.rethrowRuntimeException(e);
                }
            }
        }
        return null;

    }
}
```

```java
package com.wh.router;

import com.netflix.zuul.exception.ZuulException;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.autoconfigure.web.ErrorController;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Enumeration;

@Controller
@RequestMapping("/error")
public class DraiErrorController implements ErrorController {

    private String errorPath;

    @RequestMapping
    @ResponseBody
    public ZuulException jsonException(HttpServletRequest request, HttpServletResponse response) {
        Enumeration<String> attributeNames = request.getAttributeNames();
        while (attributeNames.hasMoreElements()){
            System.out.println(attributeNames.nextElement());

        }
        Object attribute = request.getAttribute("statusCode");
       /* String code = request.getAttribute("javax.servlet.error.errorCode").toString();
        String msg = request.getAttribute("javax.servlet.error.errorMessage").toString();
        return new ZuulException(msg, Integer.valueOf(code), "");*/
        return null;
    }

    @Override
    public String getErrorPath() {
        return null;
    }
}
```

```java
第二种：
package com.wh.router;

import org.springframework.boot.autoconfigure.web.DefaultErrorAttributes;
import org.springframework.web.context.request.RequestAttributes;

import java.util.Map;

public class MyErrorAttribute extends DefaultErrorAttributes {

    @Override
    public Map<String, Object> getErrorAttributes(RequestAttributes requestAttributes, boolean includeStackTrace) {
        Map<String, Object> result = super.getErrorAttributes(requestAttributes, includeStackTrace);
        result.put("data", 222);
        result.put("error", "error");
        result.put("exception", "exception");
        result.put("message", "message");
        return result;
    }
}
```



