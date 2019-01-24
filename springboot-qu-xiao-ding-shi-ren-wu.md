```
package com.wh.common;

import com.wh.entity.ActivityInfo;
import com.wh.mapper.ActivityInfoMapper;
import com.wh.model.resp.GetMebInfoResp;
import com.wh.service.MemberMebService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.scheduling.concurrent.ThreadPoolTaskScheduler;
import org.springframework.scheduling.support.CronTrigger;
import org.springframework.stereotype.Component;

import java.util.List;
import java.util.concurrent.ScheduledFuture;

/**
 * 定时任务-修改活动表用户昵称
 *
 */
@Component
public class AutoShelfTask {


    @Autowired
    private ThreadPoolTaskScheduler threadPoolTaskScheduler;

    private ScheduledFuture<?> future = null;

    @Bean
    public ThreadPoolTaskScheduler threadPoolTaskScheduler() {
        ThreadPoolTaskScheduler scheduler = new ThreadPoolTaskScheduler();
        scheduler.setPoolSize(20);
        // TODO    定时器需要执行的方法
        future = threadPoolTaskScheduler.schedule(this::handleNickname, new CronTrigger("0/5 * * * * *"));
        return scheduler;
    }

    /**
     * 停止定时任务
     */
    private void stop() {
        if (future != null) {
            future.cancel(true);
        }
    }


    @Autowired
    private MemberMebService memberMebService;

    @Autowired
    private ActivityInfoMapper activityInfoMapper;

    //    @Scheduled(fixedDelay = 5000)
    private void handleNickname() {
        long l = activityInfoMapper.countBlankNicknameRecord();
        if (l == 0) {
            stop();
        }
        if (l > 0) {
            long count = l / 20L + 1L;
            for (long i = 0; i <= count; i++) {
                List<ActivityInfo> activityInfos = activityInfoMapper.selectBlankNicknameRecord(i, i + 20L);
                activityInfos.forEach(a -> {
                    GetMebInfoResp getMebInfoResp = memberMebService.getMebInfo(a.getMemId(), "", false);
                    if (getMebInfoResp != null) {
                        activityInfoMapper.updateNickname(a.getId(), getMebInfoResp.getNickname());
                    }
                });
            }
        }
    }

}

```



