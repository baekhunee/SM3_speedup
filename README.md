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
