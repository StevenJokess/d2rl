# 分布式RL的基础

1 基本概念
1. 1多进程与多线程
多进程与多线程是使用分布式系统的第一个要面对的基础问题。

首先先说二者的定义：

 一个程序至少有一个进程，一个进程至少有一个线程
 把程序加载在内存中， 进程是操作系统分配资源的最小单元，线程是操作系统调度的最小单元。
 进程是静态概念 用来分配资源，线程是动态概念，用来执行调度。
找了很多说法，还是一头雾水，原因在于着概念一点都不具体啊。。

这篇博客就很生动地描述了什么是进程 什么是线程

进程与线程的一个简单解释 - 阮一峰的网络日志 (ruanyifeng.com)
还有这一篇

熬了两个通宵写的！终于把多线程和多进程彻底讲明白了！ (baidu.com)
基础概念这部分基本是照搬这篇，这篇文章已经被转载得烂大街了，以至于里面的代码都是乱的，而且我也找不到原作者了。
简单来说，当我们启动了一个程序，就可以认为是开启了一个进程，比如开启浏览器，开启pycharm，或者启动你写的程序脚本，那开启浏览器时，可以听音乐看视频，则是一个个线程的概念，进程就是线程的集合，正是由于线程的并发才使得我们可以是整个浏览器同时运行。

1 .2 并行与并发
并发是指同一时刻只能有一条指令执行，但是多个线程的对应的指令被快速轮换地执行。

一个处理器，它先执行线程 A 的指令一段时间，再执行线程 B 的指令一段时间，再切回到线程 A 执行一段时间。

由于处理器执行指令的速度和切换的速度非常非常快，人完全感知不到计算机在这个过程中有多个线程切换上下文执行的操作，这就使得宏观上看起来多个线程在同时运行。但微观上只是这个处理器在连续不断地在多个线程之间切换和执行，每个线程的执行一定会占用这个处理器一个时间片段，同一时刻，其实只有一个线程在执行。



并行指同一时刻，有多条指令在多个处理器上同时执行，并行必须要依赖于多个处理器。不论是从宏观上还是微观上，多个线程都是在同一时刻一起执行的。

并行只能在多处理器系统中存在，如果我们的计算机处理器只有一个核，那就不可能实现并行。

而并发在单处理器和多处理器系统中都是可以存在的，因为仅靠一个核，就可以实现并发。

系统处理器需要同时运行多个线程。如果只有一个核，那只能通过并发来运行这些线程。如果有多个核，当一个核在执行一个线程时，另一个核可以执行另一线程，这两个线程就实现了并行执行，其他的线程也可能和另外的线程处在同一个核上执行，它们之间就是并发执行。具体执行方式，取决于操作系统的调度。

参考 熬了两个通宵写的！终于把多线程和多进程彻底讲明白了！ (baidu.com)
多线程适用场景

使用多线程，处理器就可以在某个线程等待的时候，去执行其他的线程，从而从整体上提高执行效率。



1.3 IO密集和计算密集
网络爬虫就是一个非常典型的例子，爬虫在向服务器发起请求之后，有一段时间必须要等待服务器的响应返回，这种任务就属于 IO 密集型任务。对于这种任务，如果我们启用多线程，处理器就可以在某个线程等待的过程中去处理其他的任务，从而提高整体的爬取效率。

但并不是所有的任务都是 IO 密集型任务，还有一种任务叫作计算密集型任务，也可以称之为 CPU 密集型任务。顾名思义，就是任务的运行一直需要处理器的参与。此时如果我们开启了多线程，一个处理器从一个计算密集型任务切换到切换到另一个计算密集型任务上去，处理器依然不会停下来，始终会忙于计算，这样并不会节省总体的时间，因为需要处理的任务的计算总量是不变的。如果线程数目过多，反而还会在线程切换的过程中多耗费一些时间，整体效率会变低。

所以，如果任务不全是计算密集型任务，我们可以使用多线程来提高程序整体的执行效率。尤其对于网络爬虫这种 IO 密集型任务来说,它把我们等待的时间计算进去了，节省了大部分时间。使用多线程会大大提高程序整体的爬取效率。

2 Python实现多线程
2.1 基本认识
python中实现多线程的方式是threading，是python自带的模块

首先写一个不是多线程的程序来看一下基本运行时间

 import time
 def start():
     for i in range(1000000):
         i+=1
         return  #不使用线程
 if __name__ == '__main__':
     start_time=time.time()
     for i in range(10):
         start()
     print(time.time()-start_time)
参考博文里面的程序，文章显示运行时间6.5s，我运行则是0.36s，看来是我的CPU太好了。
在我的i9-10900k的加持下，我的运行时间是0.369s，接下来实现一个多线程的版本

 import threading, time
 def start():
     for i in range(1000000):
         i += i
     return
 thread_run_time={}
 start_time =time.time()
 for i in range(10):
     thread = threading.Thread(target=start) #target中是要启动的线程的function
     thread.start()
     thread_run_time[i] = thread
 ​
 for i in range(10):
     thread_run_time[i].join() #join()等待线程结束
 print(time.time()-start_time)
这次我的运行时间是0.37s
可以看到，速度上的区别不大。多线程并发不如单线程顺序执行快

这是得不偿失的, 造成这种情况的原因就是 GIL, Python 因为 GIL 存在，同一时期只有一个线程在执行，这样这样就是造成我们开是个线程和一个线程没有太大区别的原因。

2.2 守护线程与非守护线程
不会随着主线程结束而销毁的，这种线程叫做：非守护线程，例如：

 import threading, time
 def target(second):
     print(f'Threading {threading.current_thread().name} is runing')
     print(f'Threading {threading.current_thread().name} sleep {second}s')
     time.sleep(second)
     print(f'Threading {threading.current_thread().name} ended')
     print(f'Threading {threading.current_thread().name} is runing')
 for i in [1, 5]:
     t = threading.Thread(target=target, args=[i])
     t.start()
 print(f'Threading {threading.current_thread().name} is ended')
 Threading Thread-1 is runing
 Threading Thread-1 sleep 1s
 Threading Thread-2 is runing
 Threading Thread-2 sleep 5s
 Threading MainThread is ended
 Threading Thread-1 ended
 Threading Thread-1 is runing
 Threading Thread-2 ended
 Threading Thread-2 is runing
这里需要注意的是，threading中调用了其他函数，如果这个函数需要参数，则使用threading里面args来对函数传参，可以看到主线程运行完并没有销毁其他线程，如果想要主线程等待其他线程结束，则需要加入.join()来实现

守护线程则是主线程运行完就会被销毁，将setDaemon设置为 True 即可。

 import threading, time
 def target(second):
     print(f'Threading {threading.current_thread().name} is runing')
     print(f'Threading {threading.current_thread().name} sleep {second}s')
     time.sleep(second)
     print(f'Threading {threading.current_thread().name} ended')
     print(f'Threading {threading.current_thread().name} is runing')
 for i in [1, 5]:
     t = threading.Thread(target=target, args=[i])
     t.setDaemon(True) #此时就是守护线程
     t.start()
 print(f'Threading {threading.current_thread().name} is ended')
2. 3 互斥锁与递归锁
一个进程中的多个线程是资源共享的，

 import threading, time
 count = 0
 ​
 class MyThread(threading.Thread):
     def __init__(self):
         threading.Thread.__init__(self)
 ​
     def run(self):
         global count
         temp = count + 1
         time.sleep(0.001)
         count = temp
 ​
 threads = []
 for _ in range(1000):
     thread = MyThread()
     thread.start()
     threads.append(thread)
 for thread in threads:
     thread.join()
 # print(len(threads))
 print(f'Final count: {count}')
可以发现，原来使用同时执行1000个线程，加速运行，但最后count并没有达到1000，而是69

我的是7，因为我的cpu还蛮快的。
为了避免数据间被影响，使用锁来对线程进行数据保护，

例如

 import threading, time
 count = 0
 lock=threading.Lock()
 class MyThread(threading.Thread):
     def __init__(self):
         threading.Thread.__init__(self)
 ​
     def run(self):
         global count
         lock.acquire() # 加上锁
         temp = count + 1
         time.sleep(0.001)
         count = temp
         lock.release()  #释放锁
 ​
 threads = []
 for _ in range(1000):
     thread = MyThread()
     thread.start()
     threads.append(thread)
 for thread in threads:
     thread.join()
 # print(len(threads))
 print(f'Final count: {count}')
第一个线程拿到这把锁的 lock.acquire() 了，那另一个线程就会在 lock.acquire() 阻塞了，直到我们另一个线程把 lock.release() 锁释放

死锁：就是前面的线程拿到锁之后，运行完却不释放锁，下一个线程在等待前一个线程释放锁，这种就是死锁。说的直白一点就是，相互等待。就像照镜子一样，你中有我，我中有你。也就是在没有 release 的这种情况。

递归锁 就是以个锁可以再嵌套一个锁

2.4 Python 多线程的问题
由于 Python 中 GIL 的限制，导致不论是在单核还是多核条件下，在同一时刻只能运行一个线程，导致 Python 多线程无法发挥多核并行的优势。

GIL 全称为 Global Interpreter Lock，中文翻译为全局解释器锁，其最初设计是出于数据安全而考虑的。

在 Python 多线程下，每个线程的执行方式如下：

获取 GIL -》执行对应线程的代码 -》 释放 GIL 可见，某个线程想要执行，必须先拿到 GIL，我们可以把 GIL 看作是通行证，并且在一个 Python 进程中，GIL 只有一个。拿不到通行证的线程，就不允许执行。这样就会导致，即使是多核条件下，一个 Python 进程下的多个线程，同一时刻也只能执行一个线程。

不过对于爬虫这种 IO 密集型任务来说，这个问题影响并不大。而对于计算密集型任务来说，由于 GIL 的存在，多线程总体的运行效率相比可能反而比单线程更低。

那如何避免GIL问题呢？答案就是不用多线程，而是使用多进程，用multiprocessing代替Thread

但是多进程的创建和销毁开销是很大的

进程之间无法看到对方数据，需要使用栈或者队列来进行获取

 任务管理器上面超过五六个进程。都是进程的话，怎么能开那么多呢？
 ​
 答：一个 CPU 不止能执行一个进程，进程是并发执行的。假设现在只有一个 CPU ，一个 CPU 里面六个进程，同一时间它只有一个进程在运行。不过我们计算执行速度非常快，这个程序执行完，它就会执行一个上下文切换，执行下一个。
3 Python实现多进程
参考multiprocessing --- 基于进程的并行 — Python 3.10.3 文档
Python多进程编程 - jihite - 博客园 (cnblogs.com)
Python实现多进程的四种方式python脚本之家 (jb51.net)
代码基本来自于python并发编程之多进程(实践篇) - anne199534 - 博客园 (cnblogs.com)
multiprocessing的使用方法和threading非常相似，

首先先说如何创建一个进程

3.1 创建
方法1 是直接对process进行调用

 import multiprocessing
 import time
 ​
 def worker(interval):
     n = 5
     while n > 0:
         print("The time is {0}".format(time.ctime()))
         time.sleep(interval)
         n -= 1
 ​
 if __name__ == "__main__":
     p = multiprocessing.Process(target = worker, args = (3,))
     p2 = multiprocessing.Process(target = worker, args = (4,))
     p.start()
     p2.start()
     print (p.pid)
     print (p.name)
     print (p.is_alive())
 '''
 group参数未使用，值始终为None
 target表示调用对象，即子进程要执行的任务
 args表示调用对象的位置参数元组，args=(1,2,'anne',)
 kwargs表示调用对象的字典,kwargs={'name':'anne','age':18}
 name为子进程的名称
 '''
方法2就是把他写在类里面，用继承的方式使用

 #方法二 继承式调用
 import time
 import random
 from multiprocessing import Process
 ​
 class Run(Process):
     def __init__(self,name):
         super().__init__()
         self.name=name
     def run(self):
         print('%s runing' %self.name)
         time.sleep(random.randrange(1,5))
         print('%s runing end' %self.name)
 ​
 p1=Run('anne')
 p2=Run('alex')
 p1.start() #start会自动调用run
 p2.start()
 print('主线程')
3.2 阻断
使用join可以进行阻断

 from multiprocessing import Process
 ​
 class Run(Process):
     def __init__(self):
         super().__init__()
     def run(self):
         print('%s runing' %self.name)
         count=0
         for i in range(100000000):
             count+=1
         print('%s runing end' %self.name)
 process_list ={}
 if __name__ == '__main__':
     for i in range(10):
         process_list[i] = Run()
     for i in range(10):
         process_list[i].start()  # start会自动调用run
     for i in range(10):
         process_list[i].join()  # start会自动调用run
     print('主线程')
 '''
 Run-1 runing
 Run-2 runing
 Run-3 runing
 Run-4 runing
 Run-5 runing
 Run-6 runing
 Run-7 runing
 Run-8 runing
 Run-9 runing
 Run-10 runing
 Run-3 runing end
 Run-2 runing end
 Run-5 runing end
 Run-9 runing end
 Run-7 runing end
 Run-1 runing end
 Run-8 runing end
 Run-4 runing end
 Run-6 runing end
 Run-10 runing end
 主线程
 Process finished with exit code 0
 '''
同时，也可以看到多进程轻松地拉满了CPU、


3.3 守护与非守护
与threading一致

都是使用deamon

 p=Run('anne')
 p.daemon=True #一定要在p.start()前设置,设置p为守护进程,禁止p创建子进程,并且父进程代码执行结束,p即终止运行
3.4 进程锁
 #由并发变成了串行,牺牲了运行效率,但避免了竞争
 from multiprocessing import Process,Lock
 import os,time
 def work(lock):
     lock.acquire()
     print('%s is running' %os.getpid())
     time.sleep(2)
     print('%s is done' %os.getpid())
     lock.release()
 if __name__ == '__main__':
     lock=Lock()
     for i in range(3):
         p=Process(target=work,args=(lock,))
         p.start()
用法和threading也完全一样。

互斥锁只允许一个线程更改数据，而Semaphore同时允许一定数量的线程更改数据

    sem.acquire()
     time.sleep(random.randint(0,3))
     sem.release()
 if __name__ == '__main__':
     sem=Semaphore(5)
python线程的事件用于主线程控制其他线程的执行，事件主要提供了三个方法 set、wait、clear。

 ​
 #_*_coding:utf-8_*_
 #!/usr/bin/env python
 ​
 from multiprocessing import Process,Event
 import time,random
 ​
 def car(e,n):
     while True:
         if not e.is_set(): #Flase
             print('\033[31m红灯亮\033[0m，car%s等着' %n)
             e.wait()
             print('\033[32m车%s 看见绿灯亮了\033[0m' %n)
             time.sleep(random.randint(3,6))
             if not e.is_set():
                 continue
             print('走你,car', n)
             break
 ​
 def police_car(e,n):
     while True:
         if not e.is_set():
             print('\033[31m红灯亮\033[0m，car%s等着' % n)
             e.wait(1)
             print('灯的是%s，警车走了,car %s' %(e.is_set(),n))
             break
 ​
 def traffic_lights(e,inverval):
     while True:
         time.sleep(inverval)
         if e.is_set():
             e.clear() #e.is_set() ---->False
         else:
             e.set()
 ​
 if __name__ == '__main__':
     e=Event()
 ​
     for i in range(5):
         p = Process(target=police_car, args=(e, i,))
         p.start()
     t=Process(target=traffic_lights,args=(e,10))
     t.start()
 ​
     print('============》')
 ​


3.5通信
mutiprocessing模块为我们提供的基于消息的IPC通信机制：队列和管道。

3.5.1 队列
 '''multiprocessing模块支持进程间通信的两种主要形式:管道和队列
 都是基于消息传递实现的,但是队列接口
 '''
 from multiprocessing import Process,Queue
 import time
 q=Queue(3) #3是队列的大小
 ​
 #put ,get ,put_nowait,get_nowait,full,empty
 q.put(3)
 q.put(3)
 q.put(3)
 print(q.full()) #满了
 ​
 print(q.get())
 print(q.get())
 print(q.get())
 print(q.empty()) #空了
 ​
 队列的使用
生产者消费者模式是通过一个容器来解决生产者和消费者的强耦合问题。生产者和消费者彼此之间不直接通讯，而通过阻塞队列来进行通讯，所以生产者生产完数据之后不用等待消费者处理，直接扔给阻塞队列，消费者不找生产者要数据，而是直接从阻塞队列里取，阻塞队列就相当于一个缓冲区，平衡了生产者和消费者的处理能力。

 from multiprocessing import Process,Queue
 import time,random,os
 def consumer(q):
     while True:
         res=q.get()
         time.sleep(random.randint(1,3))
         print('\033[45m%s 吃 %s\033[0m' %(os.getpid(),res))
 ​
 def producer(q):
     for i in range(10):
         time.sleep(random.randint(1,3))
         res='包子%s' %i
         q.put(res)
         print('\033[44m%s 生产了 %s\033[0m' %(os.getpid(),res))
 ​
 if __name__ == '__main__':
     q=Queue()
     #生产者们:即厨师们
     p1=Process(target=producer,args=(q,))
 ​
     #消费者们:即吃货们
     c1=Process(target=consumer,args=(q,))
 ​
     #开始
     p1.start()
     c1.start()
     print('主')
 ​
这里需要注意q.get()方法会在队列为空时一直等待队列的内容。

此时的问题是主进程永远不会结束，原因是：生产者p在生产完后就结束了，但是消费者c在取空了q之后，则一直处于死循环中且卡在q.get()这一步。

解决方式无非是让生产者在生产完毕后，往队列中再发一个结束信号，这样消费者在接收到结束信号后就可以break出死循环。

 ​
 def consumer(q):
     while True:
         res=q.get()
         if res is None:break #收到结束信号则结束
         time.sleep(random.randint(1,3))
         print('\033[45m%s 吃 %s\033[0m' %(os.getpid(),res))
 ​
 def producer(q):
     for i in range(10):
         time.sleep(random.randint(1,3))
         res='包子%s' %i
         q.put(res)
         print('\033[44m%s 生产了 %s\033[0m' %(os.getpid(),res))
     q.put(None) #发送结束信号
 ​
结束信号不一定要由生产者发，主进程里同样可以发，但主进程需要等生产者结束后才应该发送该信号

     p1.join()
     q.put(None) #发送结束信号
在有多个生产者和多个消费者时，有几个消费者就发几次信号

 if __name__ == '__main__':
     q=Queue()
     #生产者们:即厨师们
     p1=Process(target=producer,args=('包子',q))
     p2=Process(target=producer,args=('骨头',q))
     p3=Process(target=producer,args=('泔水',q))
 ​
     #消费者们:即吃货们
     c1=Process(target=consumer,args=(q,))
     c2=Process(target=consumer,args=(q,))
 ​
     #开始
     p1.start()
     p2.start()
     p3.start()
     c1.start()
 ​
     p1.join() #必须保证生产者全部生产完毕,才应该发送结束信号
     p2.join()
     p3.join()
     q.put(None) #有几个消费者就应该发送几次结束信号None
     q.put(None) #发送结束信号
     print('主')
3.5.2 管道
管道必须产生在Process对象之前，进程之间创建一条管道，并返回元组（conn1,conn2）,其中conn1，conn2表示管道两端的连接对象，参数dumplex: 默认管道是全双工的，如果将duplex射成False，conn1只能用于接收，conn2只能用于发送。

 from multiprocessing import Process,Pipe
 ​
 import time,os
 def consumer(p,name):
     left,right=p
     left.close()
     while True:
         try:
             baozi=right.recv() #右端
             print('%s 收到包子:%s' %(name,baozi))
         except EOFError:
             right.close()
             break
 def producer(seq,p):
     left,right=p
     right.close()
     for i in seq:
         left.send(i)
         time.sleep(1)
     else:
         left.close()
 if __name__ == '__main__':
     left,right=Pipe()
     c1=Process(target=consumer,args=((left,right),'c1'))
     c1.start()
 ​
     seq=(i for i in range(10))
     producer(seq,(left,right))
 ​
     right.close()
     left.close()
 ​
     c1.join()
     print('主进程')
 ​
双向通信：

 from multiprocessing import Process,Pipe
 ​
 import time,os
 def adder(p,name):
     server,client=p
     client.close()
     while True:
         try:
             x,y=server.recv()
         except EOFError:
             server.close()
             break
         res=x+y
         server.send(res)
     print('server done')
 if __name__ == '__main__':
     server,client=Pipe()
 ​
     c1=Process(target=adder,args=((server,client),'c1'))
     c1.start()
 ​
     server.close()
 ​
     client.send((10,20))
     print(client.recv())
     client.close()
 ​
     c1.join()
     print('主进程')
 ​
3.6 进程池
多进程需要注意的问题是：

1）需要并发执行的任务通常要远大于核数

2）一个操作系统不可能无限开启进程，通常有几个核就开几个进程

3）进程开启过多，效率反而会下降

我们可以通过维护一个进程池来控制进程数目

对于远程过程调用的高级应用程序而言，应该使用进程池，Pool可以提供指定数量的进程，供用户调用，当有新的请求提交到pool中时，如果池还没有满，那么就会创建一个新的进程用来执行该请求；但如果池中的进程数已经达到规定最大值，那么该请求就会等待，直到池中有进程结束，就重用进程池中的进程。

 Pool([numprocess  [,initializer [, initargs]]]):创建进程池
    ''' numprocess:要创建的进程数，如果省略，将默认使用cpu_count()的值
     initializer：是每个工作进程启动时要执行的可调用对象，默认为None
     initargs：要传给initializer的参数组
 方法：'''
 ​
 p.apply(func [, args [, kwargs]])
 '''池中执行func(*args,**kwargs),然后返回结果。
 需要强调的是：此操作并不会在所有池工作进程中并执行func函数。如果要通过不同参数并发地执行func函数，必须从不同线程调用p.apply()函数或者使用p.apply_async()'''
 p.apply_async(func [, args [, kwargs]]):
     ''' 在一个池工作进程中执行func(*args,**kwargs),然后返回结果。
  此方法的结果是AsyncResult类的实例，callback是可调用对象，接收输入参数。当func的结果变为可用 时，将理解传递给callback。callback禁止执行任何阻塞操作，否则将接收其他异步操作中的结果。  '''

 p.close()'''关闭进程池'''
 P.join()'''等待所有工作进程退出。此方法只能在close（）或teminate()之后调用
同步调用applay

 from multiprocessing import Pool
 import os,time
 def work(n):
     print('%s run' %os.getpid())
     time.sleep(3)
     return n**2
 ​
 if __name__ == '__main__':
     p=Pool(3) #进程池中从无到有创建三个进程,以后一直是这三个进程在执行任务
     res_l=[]
     for i in range(10):
         res=p.apply(work,args=(i,)) #同步调用有阻塞
         res_l.append(res)
     print(res_l)
 '''
 125892 run
 128728 run
 130348 run
 125892 run
 128728 run
 130348 run
 125892 run
 128728 run
 130348 run
 125892 run
 [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
 ​
 Process finished with exit code 0
 '''
异步调用apply_async

 from multiprocessing import Pool
 import os,time
 def work(n):
     print('%s run' %os.getpid())
     time.sleep(3)
     return n**2
 ​
 if __name__ == '__main__':
     p=Pool(3)
     res_l=[]
     for i in range(10):
         res=p.apply_async(work,args=(i,))#异步无阻塞
         res_l.append(res)
     p.close()
     p.join()
     for res in res_l:
         print(res.get()) #使用get来获取apply_aync的结果
 '''
 119568 run
 130684 run
 118732 run
 118732 run
 119568 run
 130684 run
 130684 run
 119568 run
 118732 run
 118732 run
 0
 1
 4
 9
 16
 25
 36
 49
 64
 81
 ​
 Process finished with exit code 0
 '''
4 torch.multiprocessing
参考torch.multiprocessing - PyTorch中文文档 (pytorch-cn.readthedocs.io)
封装了multiprocessing模块。用于在相同数据的不同进程中共享视图。

一旦张量或者存储被移动到共享单元(见share_memory_()),它可以不需要任何其他复制操作的发送到其他的进程中。

这个API与原始模型完全兼容，为了让张量通过队列或者其他机制共享，移动到内存中，我们可以

由原来的import multiprocessing改为import torch.multiprocessing。

由于API的相似性，我们没有记录这个软件包的大部分内容，我们建议您参考原始模块的非常好的文档。

在实际应用中，我们使用torch.multiprocessing是为了让张量也可以通过多进程的方式进行使用。调用方法和multiprocessing一样
5 总结
缝缝补补差不多就这么些知识，当然多半是抄的


看完了，突然有感触：连这都不知道，估计昨天的面试凉的透透的。

[1]: https://zhuanlan.zhihu.com/p/483796380
