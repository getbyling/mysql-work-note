Java 程序异步调用
=================

异步调用实现方法 1
-------------------
>示例代码：
```java
public class testClass{
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

异步调用实现方法 3
-------------------
>示例代码：
```java
package cn.zm518.shequn.members.writer.biz;

import cn.zm518.shequn.members.entity.CircleVipMember;
import cn.zm518.shequn.members.writer.service.BrushTeamMemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

/**
 * 异步执行 -- 刷云信社群 vip 会员数据
 *
 * @author lzg
 * @date 2017年11月11日
 * @mail linzhongguang@gaiay.cn
 */
@Component
public class BrushTeamMemberRunnable implements Runnable {

    Thread brushThread;

    @Autowired
    BrushTeamMemberService brushTeamMemberService;
    @Autowired
    MemberUpdateTeamNickBiz memberUpdateTeamNickBiz;

    /**
     * 执行取刷社群 vip 会员数据
     */
    public void run() {
        int page = 1;           //起始页
        int pageSize = 200;     //每页条数
        //定义社群 vip 会员数据
        List<CircleVipMember> listVipMember = new ArrayList<CircleVipMember>();
        do {
            int start = (page - 1) * pageSize;  //计算起始位置

            //取取刷社群 vip 会员数据
            listVipMember = brushTeamMemberService.getVipMember(start, pageSize);
            //遍历数据
            for (int index = 0; index < listVipMember.size(); index++) {
                CircleVipMember circleVipMember = listVipMember.get(index);
                memberUpdateTeamNickBiz.setZXAccountVip(circleVipMember.getCircleId(), circleVipMember.getUserId(), "1");
            }
            page = page + 1;    //下一页

            try {
                long millis = 60 * 1000;    //休息 1 分钟
                brushThread.sleep(millis);
            } catch (InterruptedException e) {
            }
        } while (listVipMember.size() > 0);
    }

    /**
     * 启动执行取刷社群 vip 会员数据
     */
    public void startExecBrushVip() {
        brushThread = new Thread(this);
        brushThread.start();
    }
}
```

>调用方法：
```java
package cn.zm518.shequn.members.writer.api;

import cn.jedisoft.framework.annotations.POST;
import cn.jedisoft.framework.annotations.Path;
import cn.jedisoft.framework.result.ApiResult;
import cn.jedisoft.framework.result.JsonResult;
import cn.zm518.shequn.constants.CodeMsg;
import cn.zm518.shequn.members.writer.biz.BrushTeamMemberRunnable;
import cn.zm518.shequn.util.ResultUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import javax.servlet.http.HttpServletRequest;

/**
 * 刷云信社群 vip 会员数据
 *
 * @author lzg
 * @date 2017年10月25日
 * @mail linzhongguang@gaiay.cn
 */
@Path("/api/v2/w/circle/member/up-vip-member/?$")
@Component
public class BrushTeamMemberManager {

    @Autowired
    BrushTeamMemberRunnable brushTeamMember;

    /**
     * 刷云信社群 vip 会员数据
     */
    @POST
    public ApiResult update(HttpServletRequest request) {

        //开始执行刷云信社群 vip 会员数据
        brushTeamMember.startExecBrushVip();

        return new JsonResult(ResultUtils.buildResult(CodeMsg.SUCCESS));
    }

}
```