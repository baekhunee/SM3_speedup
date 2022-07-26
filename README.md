# 实验名称
SM3 speedup

# 实验简介
尝试使用多线程，循环展开等方式对SM3进行加速

# 实验完成人
权周雨 

学号：202000460021 

git账户名称：baekhunee

# 优化过程

## 重新实现SM3

最初实现SM3时，使用的是string类，并且自定义了很多函数用于进制转换，位运算等操作，在转换的过程中势必会浪费很多不必要的时间，因而使用unsigned int类型重新实现SM3。

### SM3_initial 单次运行时间
![image](https://user-images.githubusercontent.com/105578152/181023240-99a62f9d-b2fe-4a74-a2d2-341f1c6d7518.png)

### SM3_speedup 单次运行时间
![image](https://user-images.githubusercontent.com/105578152/181023681-adc84fa9-b477-43eb-8414-94c2a9286db1.png)

通过以上两图对比可以看出，仅是重新实现SM3，单次运行时间就已缩短近3000倍

## 多线程

下面使用多线程进一步优化SM3

与使用多线程优化SM4不同，SM4是对称加密算法，我们可以将彼此之间没有数据依赖的部件分配到多个线程并行计算。而SM3本质上是求哈希值，因而整体优化思路是测量计算1000次所需的时间，将任务分配到多个线程并行处理。

定义以下两个函数(以四线程为例)：
```
void Thread4(unsigned char* message, uint len, unsigned char* Hash)
{
	for (int i = 0; i < 250; i++)
		SM3(message, len, Hash);
}

void thread4(unsigned char* message, uint len, unsigned char* Hash)
{
	thread* threads = new thread[thread_num];
	int i = 0;
	for (i = 0; i < thread_num; i++)
		threads[i] = thread(Thread4, ref(message), len, Hash);
	for (i = 0; i < thread_num; i++)
		threads[i].join();
}
```

主函数中调用：
```
thread4(message, len, digest);
```

### 测试截图

![image](https://user-images.githubusercontent.com/105578152/181026368-e5ec5f1b-f90d-4e16-82ad-8c8546652263.png)

可以看出，使用四线程对SM3进行优化，速度再次提升了3倍左右。测试还发现，使用8个线程并没有进一步提升速度，个人认为原因在于与计算相比，创建新线程会产生更大的开销，因而影响了速度的提升。

