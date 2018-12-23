```
需要说明的是：

如果某个进程接受 kill pid，必须在该进程函数的最外层捕获： except GracefulExitException，

同时该进程函数内部调用捕获了 except Excetion，则必须在之前捕获GracefulExitException，

即：(2) 必须在(3)之前。否则同时去掉(2)和(3)。


    def do_some_func():
      try:
          time.sleep(10)
 
            blablabla
 
        (1) except SomeError:
            # 捕获特定的异常
            pass
 
        (2) except GracefulExitException:
            # 接收 kill pid
            pass
 
        (3) except:  # 不建议捕获默认异常, 否则必须在此之前捕获：GracefulExitException
            pass
 
 
 
    def some_process_main(gee):
 
        try:
 
            while not gee.is_stop():
                do_some_func()
 
        except GracefulExitException:
            # 接收到了 kill pid, 设置中止循环
            gee.notify_stop()
            pass
        except:
            pass

```