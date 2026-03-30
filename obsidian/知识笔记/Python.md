## 调试

调试py文件：`python -m pdb xxxx.py`

## 时间

获取时间戳：

```python
import os
import time
current_time = time.time()    # 获取当前时间戳

ctime = os.path.getmtime("xxx")   # 获取某一文件的修改时间戳
ctime = os.path.getctime("xxx")   # 获取某一文件的创建时间戳

```

## subprocess模块

```python
import subprocess

str = subprocess.getoutput(cmd)  # 接收字符串格式的命令，执行命令并返回执行结果，其功能类似于os.popen(cmd).read()和commands.getoutput(cmd)。会等程序执行完后获取结果  无法执行复杂shell命令，它会将整个字符串当做一个目录去寻找对应二进制，因此此情景可用Popen来执行

subprocess.run()   # 执行指定的命令，返回命令执行状态，其功能类似于os.system(cmd)，但是其会等待命令执行完。可使用shell调用

p = subprocess.Popen("xxx", shell=True, stdout=subprocess.PIPE)    # 执行指定命令，异步调用，使用shell脚本来调用命令，输出重定向到PIPE中   重定向后需要使用p.stdout.close()来关闭iobuffer，如果使用stdout=PIPE重定向到管道时，输出太多时可能会阻塞程序运行
p.terminate()
p.wait()     # 终止命令时需要等待命令成功终止，

# 获取执行结果，需使用stdout=subprocess.PIPE重定向输出，可替代p.wait()防止PIPE阻塞
out, err = p.communicate()
print(out.decode())   # 执行输出内容
```



## re模块

```python
import re

findall(string[, pos[, endpos]])  # pos和endpos为可选的指定字符串搜索起始位置和结束位置
result = re.findall("[\W]+", text)  # 使用正则表达式匹配text内的字符串

#正则表达式中用()包含的已匹配部分可以用group(x)函数取出 x从1开始为第一个括号里的匹配内容
```

* compile函数

  ```python
  pattern = re.compile(r'\S*_t')  # 编译出一个正则表达式
  result = pattern.findall(text)  # 使用正则表达式匹配字符串 findall表示匹配所有符合表达式的字符串 返回一个列表
  ```



## unittest测试框架

python内建测试框架

`import unittest` 引入unittest

运行单个测试用例：`python -m unittest -v log_dt.LogTest.test_ChangeLogLevel`   -v 后为文件名.类名.成员函数名

unittest中最核心的四个概念是：test case，test suite，test runner，test fixture

<font size="5">**test case**</font>

一个class继承unittest.TestCase就是一个测试用例，此类中每个以test开头的方法（不以test开头会不被unittest识别），在load时便会生成一个TestCase实例。test cast可以被load到test suite中

test case下加`"""用例描述"""`，在unittest.main()中参数设置为2（verbosity=2）时可将用例描述打印出来

<font size="5">**test suite**</font>

suite解决了以下问题：1. 用例的执行顺序，用例会按照添加的顺序去执行  2.可以用suite组织多个python文件中的测试用例

使用suite指定用例执行顺序 eg：

```python
from test_mathfunc import TestMathFunc     # 将test_mathfunc.py里的TestMathFunc类导入

suite = unittest.TestSuite()     # 新建一个suite
tests = [TestMathFunc("test_add"), TestMathFunc("test_minus")]    # test_add、test_minus均为TestMathFunc类中的成员方法，也即测试用例
suite.addTests(tests)    # 将测试用例集加入suite

suite.addTest(TestMathFunc("test_multi"))   # 方法二

suite2 = unittest.TestSuite()
suite.addTests(suite2)    # 将一个suite添加到另一个suite

runner = unittest.TextTestRunner(verbosity=2)  # 新建runner
runner.run(suite)
```

<font size="5">**test fixture**</font>

* setUp() tearDown()

  setUp、tearDown为unittest.TestCase的成员方法，测试类对这两个方法的重写可以实现在执行每个测试用例前后执行特定的一组代码

* setUpClass() tearDownClass()

  类似setUp、tearDown，区别在于执行一个测试对象的所有测试用例前执行一次，以及所有测试用例执行完成后执行一次
  
  ```python
  class xxxx....
  	@classmethod   # classmethod 修饰符对应的函数不需要实例化，不需要 self 参数，但第一个参数需要是表示自身类的 cls 参数，可以来调用类的属性，类的方法，实例化对象等。
      def setUpClass(cls):
          xxx
      @classmethod
      def tearDownClass(cls):
          xxx
  ```
  
  

<font size="5">**结果判断**</font>

> https://docs.python.org/zh-cn/3/library/unittest.html#test-cases  python官方文档

调用方法时需加`self.`

| 方法                                                         | 检查对象               | 引入版本 |
| :----------------------------------------------------------- | :--------------------- | :------- |
| [`assertEqual(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertEqual) | `a == b`               |          |
| [`assertNotEqual(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertNotEqual) | `a != b`               |          |
| [`assertTrue(x)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertTrue) | `bool(x) is True`      |          |
| [`assertFalse(x)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertFalse) | `bool(x) is False`     |          |
| [`assertIs(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertIs) | `a is b`               | 3.1      |
| [`assertIsNot(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertIsNot) | `a is not b`           | 3.1      |
| [`assertIsNone(x)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertIsNone) | `x is None`            | 3.1      |
| [`assertIsNotNone(x)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertIsNotNone) | `x is not None`        | 3.1      |
| [`assertIn(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertIn) | `a in b`               | 3.1      |
| [`assertNotIn(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertNotIn) | `a not in b`           | 3.1      |
| [`assertIsInstance(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertIsInstance) | `isinstance(a, b)`     | 3.2      |
| [`assertNotIsInstance(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertNotIsInstance) | `not isinstance(a, b)` | 3.2      |

| 方法                                                         | 检查对象                                            | 引入版本 |
| :----------------------------------------------------------- | :-------------------------------------------------- | :------- |
| [`assertAlmostEqual(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertAlmostEqual) | `round(a-b, 7) == 0`                                |          |
| [`assertNotAlmostEqual(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertNotAlmostEqual) | `round(a-b, 7) != 0`                                |          |
| [`assertGreater(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertGreater) | `a > b`                                             | 3.1      |
| [`assertGreaterEqual(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertGreaterEqual) | `a >= b`                                            | 3.1      |
| [`assertLess(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertLess) | `a < b`                                             | 3.1      |
| [`assertLessEqual(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertLessEqual) | `a <= b`                                            | 3.1      |
| [`assertRegex(s, r)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertRegex) | `r.search(s)`                                       | 3.1      |
| [`assertNotRegex(s, r)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertNotRegex) | `not r.search(s)`                                   | 3.2      |
| [`assertCountEqual(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertCountEqual) | *a* 和 *b* 具有同样数量的相同元素，无论其顺序如何。 | 3.2      |

<font size="5">**自定义报错信息**</font>

```python
self.assertNotIn(a,b,msg='报错原因：xxxx')
```

<font size="5">**执行输出**</font>

执行方式：1. `unittest.TestSuite()` 全量执行   2. 创建`runner = unittest.TextTestRunner()  `  `runner.run(suite)` 执行

运行回显结果的第一行代表每一个用例执行的结果，成功为`.`，失败为`F`，出错为`E`，跳过为`S`