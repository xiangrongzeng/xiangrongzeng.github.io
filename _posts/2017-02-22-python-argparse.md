# Python argparse用法

标签（空格分隔）： 未分类

---

使用Python的argparse库，可以在命令行运行Python脚本时，很方便的传入参数。
使用argparse大致分为一下几步：

- import argparse
- 创建一个parser
- 添加若干个参数（就是你要从命令行输入的那些参数）
- 解析参数

下面举例说明。
假设我们现在已经写好了一个Python程序nn.py，它包括train()和test()两个函数，我想在命令行中运行nn.py时，根据我输入的参数来决定nn.py执行train()还是test()。比如下面的例子1表示训练，0表示测试。argparse还自带了一个参数 -h，用于显示帮助信息。

     $ python nn.py -c 1 
     
     $ python nn.py -h

对应的代码如下：

```python

import argparse     # import

#   创建parser
parser = argparse.ArgumentParser(description='Tell the program what to do')

#   添加一个参数，其中'-c'是定位符，type表示参数类型（这里是int型），default表示默认值，help后面跟的是帮助信息
parser.add_argument('-c', dest='is_train', type=int, default=0, help='name of the runner')

#   这里还可以添加任意个参数
#parser.add_argument(...)
#parser.add_argument(...)

#   解析参数
args = parser.parse_args()
#   获取每个参数的值。这里注意，我们在上面添加参数时，给dest赋值为'is_train'，这里我们就可以通过args.is_train来获取这个参数的值。
trainable = bool(args.is_train)

...

#   具体到这个例子中，我们输入的参数1被解析后转为了bool值，bool(1)是True，故将会执行trian()函数，如果我们输入0，则会执行test()函数。
if trainable:
    train() 
else:
    test()

...

```

基本用法就是这么多，更多内容可以查看[官方文档](https://docs.python.org/2/library/argparse.html)

下面补充一下add_argument()方法中的常用参数

- 在执行程序的时候，定位参数必选，可选参数可选。
    - 定位参数：parser.add_argument("-echo",help="echo the string")
    - 可选参数：parser.add_argument("--verbosity", help="increase output verbosity")
- dest：如果提供dest，例如dest="a"，那么可以通过args.a访问该参数
- default：设置参数的默认值
- type：把从命令行输入的结果转成设置的类型
- choice：允许的参数值
    - parser.add_argument("-v", "--verbosity", type=int, choices=[0, 1, 2], help="increase output verbosity")
- help：参数命令的介绍





