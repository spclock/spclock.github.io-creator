---
title: "Quartz"
date: 2022-02-10T22:56:23+08:00
author: Spc
# cover: /img/设计模式.png
categories: ["定时任务"] 
tags: ["quratz"]
# showcase: true
draft: false
---



对hashmap的认识有多少点进来看看吧



<!--more-->

# quartz

## Quartz API

Quartz API的关键接口是：

- Scheduler - 与调度程序交互的主要API。
- Job - 你想要调度器执行的任务组件需要实现的接口
- JobDetail - 用于定义作业的实例。
- Trigger（即触发器） - 定义执行给定作业的计划的组件。
- JobBuilder - 用于定义/构建 JobDetail 实例，用于定义作业的实例。
- TriggerBuilder - 用于定义/构建触发器实例。
- Scheduler 的生命期，从 SchedulerFactory 创建它时开始，到 Scheduler 调用shutdown() 方法时结束；Scheduler 被创建后，可以增加、删除和列举 Job 和 Trigger，以及执行其它与调度相关的操作（如暂停 Trigger）。但是，Scheduler 只有在调用 start() 方法后，才会真正地触发 trigger（即执行 job）





导入包

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.1</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>


<!--定时任务依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-quartz</artifactId>
        </dependency>
	</dependencies>
```

设计模式：
builder
factory
组件模式



主要模块：

jobDetail    trigger  schedule



**trigger**:quartz中的触发器用来告诉调度程序作业什么时候触发，trigger用来触发执行job的

通用属性：

jobkey

startTime   startAt

endTime   endAt



simpleTrigger：在一个指定时间段内执行一次作业任务，或者在指定的时间间隔内多次执行作业任务

需要注意：

* 重复次数可以为0、正整数和REPEAT_INDEFINITELY（-1）
* 重复执行时间间隔必须为0或长整型
* **一定指定了endTime参数，那么它会覆盖重复次数参数的效果**

**job&jobDetail**

生命周期：任务调度器执行job 创建 用完就销毁

jobDetail ：job实例提供很多设置属性，以及JobDataMap成员变量





```java
public class QuartzDemo {

    public static void main(String[] args) throws SchedulerException {

        HashMap<String, String> stringStringHashMap = new HashMap<>();
        stringStringHashMap.put("spc", "hello quartz");
        JobDetail jobDetail = JobBuilder.newJob(HelloJob.class)
                .withIdentity("jobKey")
                .usingJobData(new JobDataMap(stringStringHashMap))
                .build();

        System.out.println(jobDetail.getDescription());
        System.out.println(jobDetail.getKey().getName());
        System.out.println(jobDetail.getKey().getGroup());
        System.out.println(jobDetail.getJobClass().getName());


        SimpleTrigger trigger = TriggerBuilder.newTrigger()
                .withIdentity("triggerKey", "trigger")
                .startNow()
                .withSchedule(
                        SimpleScheduleBuilder
                                .simpleSchedule()
                                .withIntervalInMilliseconds(0)
                                .withIntervalInSeconds(5)
                                .repeatForever()
                ).build();


        StdSchedulerFactory sfact = new StdSchedulerFactory();
        Scheduler scheduler = sfact.getScheduler();
        scheduler.start();
        scheduler.scheduleJob(jobDetail, trigger);
    }
```



```java
public class HelloJob implements Job {
    private String SPC;

    public String getSPC() {
        return SPC;
    }

    public void setSPC(String SPC) {
        this.SPC = SPC;
    }

    @Override
    public void execute(JobExecutionContext jobExecutionContext) throws JobExecutionException {

        Date data = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-HH-dd HH:mm:ss");
        System.out.println(simpleDateFormat.format(data));

        System.out.println("hello world");

//        JobDataMap mergedJobDataMap = jobExecutionContext.getMergedJobDataMap();
        //你可以用jobExecutionContext.getMergedJobDataMap()来获取jobDetail的值
        //也可以在继承job的子类上定义同名的属性get set方法（不分大小写）就能自动获取到属性值
        System.out.println("spc value: "+ SPC);
    }
}
```





创建

jobBuilder-> 创建 JobDetail

TriggerBuilder->Trigger

当触发器被触发的时候会 重新job



scheduler

job  （job，trigger）绑定在一起的

* 添加 scheduleJob
* 更新 rescheduleJob
* 删除 deleteJob
* 暂停 pauseJob
* 恢复 resumeJob
* 马上执行 triggerJob
* .....

自身

* 开启 start
* 关闭 shutdown
* .....

例子:

```java
    public static void createScheduleJob(Scheduler scheduler, QuartzBean quartzBean) {
        try {
            //获取到定时任务的执行类  必须是类的绝对路径名称
            //定时任务类需要是job类的具体实现 QuartzJobBean是job的抽象类。
            Class<? extends Job> jobClass = (Class<? extends Job>) Class.forName(quartzBean.getJobClass());
            // 构建定时任务信息
            JobDetail jobDetail = JobBuilder.newJob(jobClass).withIdentity(quartzBean.getJobName()).build();
            // 设置定时任务执行方式
            CronScheduleBuilder scheduleBuilder = CronScheduleBuilder.cronSchedule(quartzBean.getCronExpression());
            // 构建触发器trigger 日历的概念
            CronTrigger trigger = newTrigger().withIdentity(quartzBean.getJobName()).withSchedule(scheduleBuilder).build();
            scheduler.scheduleJob(jobDetail, trigger);
        } catch (ClassNotFoundException e) {
            System.out.println("定时任务类路径出错：请输入类的绝对路径");
        } catch (SchedulerException e) {
            System.out.println("创建定时任务出错：" + e.getMessage());
        }
    }
```



job抽象类QuartzJobBean

QuartzJobBean类中execute方法会调用executeInternal方法

```java
public class MyTask1 extends QuartzJobBean {

    @Override
    protected void executeInternal(JobExecutionContext jobExecutionContext) throws JobExecutionException {
    }
}
```



## 域取值

下表为Cron表达式中六个域能够取的值以及支持的特殊字符。（或者7个 第七个是年份）

| 域   | 是否必需 | 取值范围                                                     | 特殊字符      |
| :--- | :------- | :----------------------------------------------------------- | :------------ |
| 秒   | 是       | [0, 59]                                                      | * , - /       |
| 分钟 | 是       | [0, 59]                                                      | * , - /       |
| 小时 | 是       | [0, 23]                                                      | * , - /       |
| 日期 | 是       | [1, 31]                                                      | * , - / ? L W |
| 月份 | 是       | [1, 12]或[JAN, DEC]                                          | * , - /       |
| 星期 | 是       | [1, 7]或[MON, SUN]。若您使用[1, 7]表达方式，`1`代表星期一，`7`代表星期日。 | * , - / ? L # |

还有一个可不填的 年份



## 特殊字符

Cron表达式中的每个域都支持一定数量的特殊字符，每个特殊字符有其特殊含义。

| 特殊字符 | 含义                                                         | 示例                                                         |
| :------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `*`      | 所有可能的值。                                               | 在月域中，`*`表示每个月；在星期域中，`*`表示星期的每一天。   |
| `,`      | 列出枚举值。                                                 | 在分钟域中，`5,20`表示分别在5分钟和20分钟触发一次。          |
| `-`      | 范围。                                                       | 在分钟域中，`5-20`表示从5分钟到20分钟之间每隔一分钟触发一次。 |
| `/`      | 指定数值的增量。                                             | 在分钟域中，`0/15`表示从第0分钟开始，每15分钟。在分钟域中`3/20`表示从第3分钟开始，每20分钟。 |
| `?`      | 不指定值，仅日期和星期域支持该字符。                         | 当日期或星期域其中之一被指定了值以后，为了避免冲突，需要将另一个域的值设为`?`。 |
| `L`      | 单词Last的首字母，表示最后一天，仅日期和星期域支持该字符。**说明** 指定 `L`字符时，避免指定列表或者范围，否则，会导致逻辑问题。 | 在日期域中，`L`表示某个月的最后一天。在星期域中，`L`表示一个星期的最后一天，也就是星期日（`SUN`）。如果在`L`前有具体的内容，例如，在星期域中的`6L`表示这个月的最后一个星期六。 |
| `W`      | 除周末以外的有效工作日，在离指定日期的最近的有效工作日触发事件。`W`字符寻找最近有效工作日时不会跨过当前月份，连用字符`LW`时表示为指定月份的最后一个工作日。 | 在日期域中`5W`，如果5日是星期六，则将在最近的工作日星期五，即4日触发。如果5日是星期天，则将在最近的工作日星期一，即6日触发；如果5日在星期一到星期五中的一天，则就在5日触发。 |
| `#`      | 确定每个月第几个星期几，仅星期域支持该字符。                 | 在星期域中，`4#2`表示某月的第二个星期四。                    |



JobExecutionContext 的参数在那里传







# Quartz定时任务Job中无法注入spring bean的解决方案

使用spring 结合quartz进行定时任务开发时，如果直接在job内的execute方法内使用service 或者mapper对象，执行时，出现空指针异常。



### 问题原因

job对象在spring容器加载时候，能够注入bean，但是调度时，job对象会重新创建，此时就是导致已经注入的对象丢失，因此报空指针异常。

```
@Configuration
public class TaskSchedulerConfig implements ApplicationListener<ContextRefreshedEvent> {

    @Override
    public void onApplicationEvent(ContextRefreshedEvent event) {
        System.out.println("任务已经启动..." + event.getSource());
    }


    @Component("quartzJobFactory")
    private class QuartzJobFactory extends AdaptableJobFactory {
        //这个对象Spring会帮我们自动注入进来,也属于Spring技术范畴.
        @Autowired
        private AutowireCapableBeanFactory capableBeanFactory;

        protected Object createJobInstance(TriggerFiredBundle bundle) throws Exception {
            //调用父类的方法
            Object jobInstance = super.createJobInstance(bundle);
            //进行注入,这属于Spring的技术,不清楚的可以查看Spring的API.
            capableBeanFactory.autowireBean(jobInstance);
            return jobInstance;
        }
    }

    @Bean(name = "scheduler")
    public Scheduler scheduler(QuartzJobFactory quartzJobFactory) throws Exception {

        SchedulerFactoryBean factoryBean = new SchedulerFactoryBean();
        factoryBean.setJobFactory(quartzJobFactory);
        factoryBean.afterPropertiesSet();
        Scheduler scheduler = factoryBean.getScheduler();
        scheduler.start();
        return scheduler;
    }




    @Bean
    public QuartzInitializerListener executorListener() {
        return new QuartzInitializerListener();
    }
```







# quartz job 异常处理



2种处理方式：

**1.捕获并解决异常，立即重新执行作业**

**2.捕获异常，取消所有触发器**



为了共享在同一个 JobDetail 中的 JobDataMap，我们需要在上面这个 job 实现类上加入 `@PersistJobDataAfterExecution` 和 `@DisallowConcurrentExecution` 注解，详见 [Quartz 2 定时任务（二）：多线程并发执行与数据共享](https://link.segmentfault.com/?url=https%3A%2F%2Fmy.oschina.net%2Fu%2F3436580%2Fblog%2F882907)。

```java
@PersistJobDataAfterExecution
@DisallowConcurrentExecution
public class BadJob1 implements Job {

	@Override
	public void execute(JobExecutionContext context)
			throws JobExecutionException {
		SimpleDateFormat dateFormat = new SimpleDateFormat(
				"yyyy-MM-dd HH:mm:ss");

		JobKey jobKey = context.getJobDetail().getKey();
		JobDataMap dataMap = context.getJobDetail().getJobDataMap();

		int flag = dataMap.getInt("flag");
		System.out.println("---" + jobKey + "，执行时间："
				+ dateFormat.format(new Date()) + ", flag： " + flag);

		// 由于零错误除以此作业将生成的异常的例外（仅在第一次运行）
		try {
			int result = 4815 / flag;

		} catch (Exception e) {
			System.out.println("--- Job1 出错!");

			// 修复分母，所以下次这个作业运行它不会再失败
			JobExecutionException e2 = new JobExecutionException(e);
			dataMap.put("flag", "1");

			//方法一 这个工作会立即重新启动（不能与方法二同时使用）
			e2.setRefireImmediately(true);
            
            //方法二 true 表示 Quartz 会自动取消所有与这个 job 有关的 trigger，从而避免再次运行 job
   		    e2.setUnscheduleAllTriggers(true);

			throw e2;
		}

		System.out.println("---" + jobKey + "，完成时间："
				+ dateFormat.format(new Date()));
    }


}
```

