# threadpoolMsg
使用线程池发送短信，客户提供的短信接口一次只能发一个手机号。
直接上代码，两个类，一个是发送短信的线程类a，一个是执行类b。
a、
package com.melo.thread;

import java.util.concurrent.TimeUnit;

/**
 * 发送手机号的线程类
 * Created by Ablert
 * on 2018/6/4.
 * @author Ablert
 */
 
public class SendMsgThread implements Runnable {

    /** 发送的手机号 **/
    
    private String telephone;

    public SendMsgThread(String telephone) {
        this.telephone = telephone;
    }

    @Override
    public void run() {
        try {
            TimeUnit.SECONDS.sleep(5);
            Thread.currentThread().setName(telephone);
            System.out.println(Thread.currentThread().getName() + "给 " + telephone +"发送短信");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

b、
package com.melo.executor;

import com.melo.thread.SendMsgThread;

import java.util.concurrent.LinkedBlockingQueue;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

/**
 * 执行线程
 * Created by Ablert
 * on 2018/6/4.
 * @author Ablert
 */
 
public class ExecutorPoolTask {

    public static void main(String[] args) {
        ThreadPoolExecutor executor = null;
        //开启的发送短信的线程数为处理器的2倍
        int corePoolSize = Runtime.getRuntime().availableProcessors() * 2;
        System.out.println("CPU处理器为：" + corePoolSize/2 + "个");
        executor = new ThreadPoolExecutor(corePoolSize/2, corePoolSize, 3, TimeUnit.SECONDS,new LinkedBlockingQueue<>());
        String[] phoneArr = {"13621072323", "13621072312", "136210721323", "13621076523", "13621074523", "13621067523", "13621072328", "13621072723"};
        for (int i=0; i<phoneArr.length; i++) {
            executor.execute(new SendMsgThread(phoneArr[i]));
        }
        executor.shutdown();
    }
}

