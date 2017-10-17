Java 程序异步调用
=================
>异步调用实现方法：
```java
/**
 * 异步调用 - 自动关注社群
 *
 * @param circleId 社群Id
 * @param userId   用户Id
 */
public void autoFollowerCircleCallback(String circleId, String userId) {

    new Thread(new Runnable() {
        @Override
        public void run() {
            //调用自动关注社群
            try {
                logger.debug("自动关注社群 calbackurl=[circleId:" + circleId + ", userId:" + userId + "]");
                //自动关注社群
                WxPlatformResult result = wxFactoryApi.getPlatform().doAttentionCircle(circleId, userId);
                if (result.getCode().equals("0") == true) {
                    logger.debug("自动关注社群成功");
                } else {
                    logger.debug("自动关注社群失败");
                }
            } catch (Exception e) {
                // 订单状态为支付成功
                logger.info("自动关注社群-异常: circleId:" + circleId + ", userId:" + userId + "，错误：" + e.getMessage());
            }
        }
    }).start();
}
```

>单元测试方法：
```java

import cn.gaiay.business.groupon.service.GrouponShareService;
import cn.gaiay.framework.model.ResultModel;
import com.alibaba.fastjson.JSON;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.context.web.WebAppConfiguration;

@SuppressWarnings("all")
@RunWith(SpringJUnit4ClassRunner.class)
@ActiveProfiles("dev")
@WebAppConfiguration
@ContextConfiguration(locations = {"/application.xml"})
public class GrouponShareServiceImplTest {
    
    Logger logger = LoggerFactory.getLogger(GrouponShareServiceImplTest.class);

    @Autowired
    GrouponShareServiceImpl grouponShareServiceImpl;

    @Test
    public void followerCircleCallback() throws Exception {

        String circleId = "08cc4b1557738083d-7ff4";
        String userId = "17b4a015f058abef2-8004";

        grouponShareServiceImpl.autoFollowerCircleCallback(circleId, userId);

        Thread.sleep(500);
        String aa = "斯蒂芬";
    }

}
```
>单元测试注意：
```text
    Thread.sleep(500);
    String aa = "斯蒂芬";
```
