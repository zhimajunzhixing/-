---
title: 定时任务（spring）
date: 2022-11-11T16:29:08Z
lastmod: 2022-11-12T21:54:37Z
---

# 定时任务（spring）

* Cron表达式

  * 格式

    * A B C D E F
    * 7个字母分别表示秒、分、小时、日、月、日期的定时规则（字符串）
    * Seconds: 可出现", - * /"四个字符，有效范围为0-59的整数  
      Minutes: 可出现", - * /"四个字符，有效范围为0-59的整数  
      Hours: 可出现", - * /"四个字符，有效范围为0-23的整数  
      DayofMonth: 可出现", - * / ? L W C"八个字符，有效范围为0-31的整数  
      Month: 可出现", - */"四个字符，有效范围为1-12的整数或JAN-DEc  
      DayofWeek: 可出现", - * / ? L C #"四个字符，有效范围为1-7的整数或SUN-SAT两个范围。1表示星期一， 以此类推  
      Year: 可出现", - * /"四个字符，有效范围为1970-2099年
* 流程

  * 声明定时任务

    * ```java
      //将定时任务类注册到容器中
      @Component
      //声明类支持定时任务
      @EnableScheduling
      public class HelloSchedule {

          //声明定时任务与执行规则，corn的值为corn表达式
          @Scheduled(cron = "59 * * * * 6")
          public void hello() throws InterruptedException {
              log.info("hello......");
          }
      }
      ```
  * 设置任务异步执行

    * why

      * 默认是阻塞
    * how

      * 业务运行以异步的方式提交到线程池
      * 定时任务线程池
      * 设置任务异步执行

        * ```java
          //声明支持异步执行
          @EnableAsync
          @EnableScheduling
          public class HelloSchedule {
              //声明异步执行
              @Async
              @Scheduled(cron = "59 * * * * 6")
              public void hello() throws InterruptedException {
                  log.info("hello......");
              }
          }
          ```

  ‍

‍
