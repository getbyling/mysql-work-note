Java 短 url 地址
================
>实现代码
```java
package cn.gaiay.business.groupon.util;

import cn.zm518.common.util.StringUtil;
import com.google.gson.Gson;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.client.RestTemplate;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;

public class ShortUrlUtil {

    private static RestTemplate restTemplate = new RestTemplate();

    /**
     * url 和 接口
     */
    private static String openAPIUrl = "http://api.weibo.com/2/short_url/shorten.json?source={source}&url_long={url_long}";
    /**
     * appKey
     */
    private static String appKey = "1661250408";

    /**
     * 转换长域名为短域名
     *
     * @param longUrl
     * @return
     */
    public static String convertDomainToShort(String longUrl) {

        String shortUrl = longUrl;

        //不符合规则的url，直接返回原链接
        if (!ShortUrlUtil.zmUrlFilter(longUrl)) {
            return longUrl;
        }

        Map<String, Object> uriVariables = new HashMap<String, Object>();
        uriVariables.put("source", appKey);
        uriVariables.put("url_long", longUrl);

        ResponseEntity<String> responseEntity = restTemplate.getForEntity(openAPIUrl, String.class, uriVariables);
        if (responseEntity.getStatusCode() == HttpStatus.OK) {
            String body = responseEntity.getBody();
            Gson gson = new Gson();

            Map resultMap = gson.fromJson(body, Map.class);
            if (resultMap != null && resultMap.size() > 0) {
                ArrayList listUrl = (ArrayList) resultMap.get("urls");
                Map mapUrl = (Map) listUrl.get(0);
                shortUrl = mapUrl.get("url_short").toString();
            }
        }

        return shortUrl;
    }

    private static boolean zmUrlFilter(String url) {
        url = url.toLowerCase();
        if (StringUtil.isNotBlank(url) && (url.startsWith("http://") || url.startsWith("https://"))) {
            return true;
        }
        return false;
    }

    public static void main(String[] args) {
        ShortUrlUtil.convertDomainToShort("http://blog.csdn.net/mitkey/article/details/53956520");
    }
}
```