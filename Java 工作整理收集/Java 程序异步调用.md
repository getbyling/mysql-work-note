Java 程序异步调用
=================

异步调用实现方法 1
-------------------
>示例代码：
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


异步调用实现方法 2
-------------------
>示例代码：
```java
import java.util.List;
import java.util.Map;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

import org.apache.log4j.Logger;
import org.springframework.stereotype.Component;

/**
 * 社群系统消息透传通知
 */
@Component
public class CircleNotice {

	Logger logger = Logger.getLogger(CircleNotice.class);
	ExecutorService threads = Executors.newCachedThreadPool();

	public int join(final String userId, final List<String> adminIds, final int tid) {
		threads.execute(new Runnable() {
			public void run() {
				for(String adminId : adminIds){
					GetClientUtil.getNoticeClient().groupSystemMsg(adminId);
				}
				AccountProfile account = GetClientUtil.getAccountClient().getUserLocation(userId);
				logger.debug("新人入群："+account.toString());
				logger.debug("开始发送新人入群消息。。。");

				//如有有管理员，则用管理员发，如果没有管理员则用加入者发送
				String sendUserId = adminIds.size() > 1 ? adminIds.get(1) : userId;

				int code = GetClientUtil.getCardMsgClient().sendNewMemberJoin(sendUserId, tid, getJoinChatMsgContent(account.getName()), account.getLogo(), "点击查看名片详情", userId, "");
				logger.debug("新人入群消息发送完成,code="+code);
			}
		});
		return 0;
	}
}
```