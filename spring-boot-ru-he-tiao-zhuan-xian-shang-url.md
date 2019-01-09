```java
@Controller //注意此处
public class ActivitiesController {

    public static final String url = "http://m.7daysinn.cn/maserati/ext/static/activities/groupCouponsTemplate.html?id=B4300FB17C757C218C8F69FB8A50308A";

    @RequestMapping("/activities")
    public String test(HttpServletRequest request, HttpServletResponse response, String channel) throws ServletException, IOException {
        if (StringUtils.isBlank(channel)) {
            return "无权限访问";
        }
        switch (channel) {
            case "1":
            case "2":
            case "3":
                Map<String, String> map = new HashMap<>(1);
                map.put(channel, url);
                MonitorLogger.info(JSON.toJSONString(map));
                //跳转指令
                return "redirect:" + url;
            default:
                break;
        }
        return "无权限访问";
    }
}
```



