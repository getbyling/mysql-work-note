Java 短 url 地址
================
>实现代码
```java
package cn.gaiay.business.groupon.util;

import cn.zm518.common.rest.client.RestClient;
import cn.zm518.common.rest.client.RestClients;
import cn.zm518.common.rest.client.RestRequest;
import cn.zm518.common.rest.client.RestResponse;
import cn.zm518.common.util.StringUtil;
import org.apache.commons.lang.StringUtils;

import java.io.UnsupportedEncodingException;

/**
 * <p>
 * HttpClient 工具类
 * </p>
 *
 * @author lys 2013-05-07
 * @version 1.0
 */

public class ShortUrlUtil {
    /**
     * url 和 接口
     */
    private static String openAPIUrl = "http://api.weibo.com/2/short_url/shorten.json";
    /**
     * appKey
     */
    private static String appKey = "1661250408";

    private final static RestClient client = RestClients.createDefault();

    /**
     * 转换长域名为短域名
     *
     * @param longUrl
     * @return
     */
    public static String convertDomainToShort(String longUrl) {
        //2016.1.27 ZhaoYZ
        //不符合规则的url，直接返回原链接
        if (!ShortUrlUtil.zmUrlFilter(longUrl)) {
            return longUrl;
        }
        RestRequest request = new RestRequest(openAPIUrl);
        request.addParam("source", appKey);
        request.addParam("url_long", longUrl);

        RestResponse<ShortUrlEntity> response = client.get(request, ShortUrlEntity.class);

        if (response.isSuccess()) {
            ShortUrlEntity entityUrls = response.getResult();
            if (entityUrls != null && entityUrls.getUrls() != null && entityUrls.getUrls().size() > 0) {
                ShortUrlEntity entity = entityUrls.getUrls().get(0);
                if (entity.isResult() && StringUtils.isNotBlank(entity.getUrl_short())) {
                    return entity.getUrl_short();
                }
            }
        }
        return longUrl;
    }

    // 接口调用返回结果示例
    /*
	 * {"request":"/sinaurl/public/shorten.json","error_code":"21504","error":"Error: Wrong arguments!"
	 * }{"urls":[{"result":true,"url_short":"http://t.cn/SSh2h7","url_long":
	 * "http://www.gaiay.cn","type":0}]}
	 */

    public static String getOpenAPIUrl() {
        return openAPIUrl;
    }

    public static void setOpenAPIUrl(String openAPIUrl) {
        ShortUrlUtil.openAPIUrl = openAPIUrl;
    }

    public static String getAppKey() {
        return appKey;
    }

    public static void setAppKey(String appKey) {
        ShortUrlUtil.appKey = appKey;
    }


    private static String encode(String str) {
        try {
            return java.net.URLEncoder.encode(str, "utf-8");
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        return str;
    }

    public static void main(String[] args) {

    }

    /**
     * url格式校验,只过滤符合掌门规则url
     * 不是非常严格的正则，后续有扩展，请根据业务自行处理
     *
     * @param url
     * @return
     * @throws
     * @author ZhaoYZ
     * @data 2016年1月26日下午6:12:30
     */
    private static boolean zmUrlFilter(String url) {
        //正则表达式匹配死循环，先简单处理一下,问题待查证
        //2016.2.19 ZhaoYZ 更新
        url = url.toLowerCase();
        if (StringUtil.isNotBlank(url) && (url.startsWith("http://") || url.startsWith("https://"))) return true;
        return false;
    }
}
```

>短链接实体类
```java
package cn.gaiay.business.groupon.util;

import java.util.List;

/**
 * sina api 生成二维码封装实体
 * 
 * @Description:
 * @author ZhaoYZ
 * @date 2015年12月18日 下午8:30:38
 */
public class ShortUrlEntity {

	private String object_id;
	private String object_type;
	private boolean result; // 短链的可用状态，true：可用、false：不可用
	private String url_short;
	private String url_long;
	private int type; // 链接的类型，0：普通网页、1：视频、2：音乐、3：活动、5、投票
	private List<ShortUrlEntity> urls;

	public ShortUrlEntity() {
	}

	public String getObject_id() {
		return object_id;
	}

	public void setObject_id(String object_id) {
		this.object_id = object_id;
	}

	public String getObject_type() {
		return object_type;
	}

	public void setObject_type(String object_type) {
		this.object_type = object_type;
	}

	public boolean isResult() {
		return result;
	}

	public void setResult(boolean result) {
		this.result = result;
	}

	public String getUrl_short() {
		return url_short;
	}

	public void setUrl_short(String url_short) {
		this.url_short = url_short;
	}

	public String getUrl_long() {
		return url_long;
	}

	public void setUrl_long(String url_long) {
		this.url_long = url_long;
	}

	public int getType() {
		return type;
	}

	public void setType(int type) {
		this.type = type;
	}

	public List<ShortUrlEntity> getUrls() {
		return urls;
	}

	public void setUrls(List<ShortUrlEntity> urls) {
		this.urls = urls;
	}

}
```