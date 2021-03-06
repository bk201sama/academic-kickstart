+++
title = "[系统架构设计师考试] day two"
date = 2018-09-11T10:21:28+08:00
draft = false

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ['系统架构设计师']
categories = ['考试']

+++
承接上文
### 各科目涉及的具体知识点

#### 信息系统综合知识
1. 操作系统基本原理
全程指导分了5块讲解:处理器,文件,存储,作业,设备管理.这些基本都是操作系统的职能,那原理自然应该是指这些职能是如何实现的?
  1. 处理器管理  
  从静态观点观点看,进程由程序,数据,进程控制块(PCB:process control block)组成.从动态观点看,进程是计算机状态的一个有序集合. 既然是状态的有序集合,那集合里的元素哪些呢.具备运行条件等待操作系统分配处理器资源,就绪态;占有处理器资源,运行态;不具备运行条件等待某事件发生,等待态(阻塞态).当系统资源(内存)不满足运行时要求,会出现进程挂起,这就又出现了2种状态:静止就绪态和阻塞态,相对应的三态模型中的称活跃就绪态和阻塞态.进程状态转换通过pv操作来控制.pv操作主要就是p操作,v操作,信号量.信号量（Semaphore）由一个值和一个指针组成，指针指向等待该信号量的进程。执行一次p操作,就是请求分配一个资源,s减1,当s<0表示没又资源可用,s的绝对值表示等待该资源的进程数.执行一次v操作,就是释放一个资源,s加1,当s>=0,s表示可用资源数量.在操作系统中,进程间存在互斥(资源共享)和同步(协作)关系.互斥就是指组织多个进程同时访问这些资源的代码段,这个代码段称为临界区,这个资源成为临界资源.由于只允许一个进入,信号量初始值应该为1.同步点就是说进程a应该在进程b完成某项事情之前一直等待,也就是说进程a的p操作一直等待到进程b的v操作结束才开始,那么信号量初始值应该从0开始.  
     + 生产者消费者问题  
     这里面存在一个竞争条件的例子(竞争条件指多个线程或者进程在读写一个共享数据时结果依赖于它们执行的相对时间的情形。):如果只监控fillCount(生产条数),对于producer,当等于1的时候就唤醒consumer,等于cache size的时候就sleep;对于consumer,当等于cache size-1的时候就唤醒producer,等于0的时候就sleep.理想情况下肯定没问题,但是存在时间片(时间片即CPU分配给各个程序的时间，每个线程被分配一个时间段，称作它的时间片，即该进程允许运行的时间，使各个程序从表面上看是同时进行的。如果在时间片结束时进程还在运行，则CPU将被剥夺并分配给另一个进程。).也就是说,这个变量是被2个进程同时访问的,随时都有可能执行一半的时候变量被更改.比如此时变量为0,consumer运行到过了0判断准备sleep的时候,这时候producer生产了1个数据,并讲变量更改为1,并去唤醒consumer,但consumer还没sleep呢,唤醒失败,运行sleep成功.然后producer一发不可收拾,一直生产直到满缓存.分析原因,变量在2个进程中都可以读取和写入.那么引入信号量算法解决.
