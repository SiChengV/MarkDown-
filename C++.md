[TOC]

# C++

## C++知识

参考资料：

* https://learn.microsoft.com/zh-cn/cpp/cpp/cpp-language-reference?view=msvc-170
  微软提供的c++语言参考

#### 命名空间

```c++
using std::bind;  // 引入std命名空间的bind函数  后续使用时不用再加std::
```

#### 强制类型转换

* dynamic_cast

  用于基类到派生类的强制转换（例如实现基类指针调用同名子类普通函数可使用此方法）

  语法：

  ```c++
  Base *pBase = new Derived();   // 如果这里是new Base，则后面的转换会失败，指针转换则返回空指针
  Derived *pDerived = dynamic_cast<Derived*>(pBase);
  ```

  

#### 构造函数

挂在某一条构造函数的初始化列表只有在此构造函数被调用时才会去初始化

```c++
class A{
public:
   A():a(10){cout << "A()" << endl;}
   A(int b){this->b = b;cout << "A(int b)" << endl;}
   int a;
   int b;
};

int main(){
        A s = 1;            // 调用了有参的构造函数，s.a没有初始化
        cout << s.a<< " " << s.b << endl; 
    		// A(int b)
    		// 131403488 1
        return 0;
}
```

Widget w1 = w2   调用拷贝构造函数

w1 = w2 调用赋值运算符函数

#### const

顶层const：int *const p1
底层const：const int *p2

#### 回调函数与std::function与std::bind

使用function和bind注册回调函数

```c++
#include <functional>

class A{
public:
    int fun(int){xxx}
}

A a;
// 已绑定成员函数为例
// function模板类型指定了调用被调函数时的返回值和参数
// bind的参数为 1. 被调类成员函数地址，对象指针， 预留参数
function<int(int)> = bind(&A::func, &a, std::placeholders::_1);    
```

注意：

* 函数如果有默认参数，在注册时会丢失，需要在bind时或调用时重新制定实际参数
* bind成员函数时定死参数后，还需要在调用的时候传入参数，否则编译报错

#### 函数指针与成员函数指针

```c++
// 函数指针 做为入参
void print(int i){
    // code
}

void func(void (*pfunc)(int), int i){
    pfunc(i);
}

// 成员函数指针做为入参
class A{
public:
	void fun(int a){
        cout << a;
    }
};

void test(A x, void (A::*pfun)(int), int y){   // 入参可填 &A::fun
    (x.*pfun)(y);   // 调用
}


```



#### 输入输出格式控制

* setw 控制一次输入输出占多少个字符，只生效一次

  ```c++
  cout << "setw(6), several elements: [" << 89 << std::setw(6) << 12 << 34 << "]\n";
  // setw(6), several elements: [89    1234]
  ```

* setfill 搭配setw一起使用，指定补充的字符

  ```c++
  cout << << "setfill('*'): [" << std::setfill('*') << std::setw(10) << 42 << "]\n";
  // setfill('*'): [********42]
  ```

  

#### new

new(std::nothrow) 在内存分配失败时会返回一个空指针，而不是触发std::bad_alloc，可以方便的进行检查

##### inline函数

如果将函数的实现放在头文件中，那么每一个包含该头文件的cpp文件都将得到一份关于该函数的定义，那么链接器会报函数重定义错误。
如果将函数的实现放在头文件，并且标记为 inline 那么每一个包含该头文件的cpp文件都将得到一份关于该函数的定义，并且链接器不会报错。
如果将函数的实现放在cpp文件中，并且没有标记为inline,那么该函数可以被连接到其他编译单元中。
如果将函数的实现放在cpp文件中，并且标记为inline, 那么该函数对其他编译单元不可见（类似static的效果），也就是其他cpp文件不能链接该函数库，这就是标题中出现的 … undefined reference to …
问题原因就是，编译器在编译一个inline函数时，需要知道其完整定义，如果编译器在本编译单元找不到inline函数定义就会报错（inline函数的调用不涉及到函数的call，也就不需要链接器参与进来工作，所以也就不会去其他编译单元查找函数定义）。


#### 获取命令行参数

```c++
#include <getopt.h>    // 包含头文件

    while (1) {
        int32_t opt = getopt(argc, argv, "a:c:l:t:p:m:s:h:f:");  // a冒号，相当于 -a xxxx
        if (opt == -1)
            break;
        switch (opt) {
            case 'a':
                g_appid = optarg;
                cout << "-a g_appid=" << g_appid << endl;
                break;
            case 'f':
                g_fileName = optarg;
                cout << "-a g_fileName=" << g_fileName << endl;
                break;
        }
    }
    return 0;

```

Google的gflags开源库有类似的功能

#### 常用函数

* strncpy_s

  ```c++
  //  将str1的size个字节赋值到str2中，size2一般为sizeof(str2)
  strncpy_s(char * str2, int size2, char * str1, int size1);
  ```

* copy_n

  提供拷贝，且允许拷贝源数据地址和目的数据地址有重叠

  ```c++
  // first 其实源数据地址   count 拷贝字节数  result 拷贝目的地址   返回值 指向拷贝后的最后一个数据的后一个元素
  OutputIt copy_n( InputIt first, Size count, OutputIt result );
  ```

  

## 现代C++特性

#### std::chrono库

头文件\<chrono>

**三个主要类型：**

* clock

  参照时钟 通常使用system_clock

* time_point

  时间点记录了从格林尼治时间开始经过了多久

* duration

  表示一段时间

system_clock的计时单位是$10^{-7}$秒

```c++
	chrono::system_clock::time_point begin = chrono::system_clock::now();    
    	your code here....
    chrono::system_clock::time_point end = chrono::system_clock::now();
    if(chrono::duration_cast<std::chrono::milliseconds>(end - begin).count() < 1000){
        // 一秒准时退出
        break;
    }
	std::this_thread::sleep_for(std::chrono::duration<long long, ratio<1,1000>>(1)); 休眠一毫秒
```

nanoseconds：纳秒。microseconds：微秒。

#### Atomic

原子变量，使用原子变量来改造存在资源竞争的变量，可以不使用互斥量和锁来对变量实现原子操作

eg：`std::atomic<int> globalVariable = 0;`  即可将globalVariable变成一个原子的int变量

#### Bitset

位运算操作使用

参考资料：https://www.cnblogs.com/rabbithu/p/bitset.html

#### 新特性string_view

相当于一个只读的string，注意不可用string来初始化，不可对观察的对象进行修改操作，它不管理内存，只保存指针和长度，所以效率高

注意string_view.data()返回的是原始字符串数组的指针，会打印完整的原始字符串

string&不能代替string_view的原因：

1. 

   ```c++
   void foo( std::string_view bob ) {
     std::cout << bob << "\n";
   }
   int main(int argc, char const*const* argv) {
     foo( "This is a string long enough to avoid the std::string SBO" );
     if (argc > 1)
       foo( argv[1] );
   }
   ```

   以上面的为例，如果foo的参数是string &类型的话，则会构造一个临时的string，此时的效率就没有stirng_view高

2. string_view允许不拷贝的情况下使用string的子串

#### 新特性span

在内存上连续对象的观察者，类似于string_view，可以解决数组指针退化，和越界访问的问题

#### decltype

```c++
// 此例中用auto和decltype推导一个引用类型，可以看到使用auto推导会丢失引用，而decltype不会
int a = 10;
int& p = a;
decltype(p) a2 = p;
a2 = 20;
cout << p << endl;    // 20
auto a3 = p;
a3 = 30;
cout << "auto : " << p << endl;   // auto：20 未修改成30
return 0;
```

auto根据等号右边的初始值自动推导类型，而decltype根据括号中的表达式推导类型，和等号右边无关，因此可以用decltype声明一个变量而auto不行

#### random库

生成随机数种子，避免了使用rand和srand的时候在一秒内的随机数种子相同的情况（srand的传输time(NULL)精确到秒）

```c++
// 生成长度随机但小于len的随机字符
std::string getRandomVal(int len){
	char tmp;
	std::string buff;
	std::random_device rd;            // 关键两行
    std::default_random_engine random(rd());
    int num = rd() % len;
	for (int i=0; i<num; i++){
		tmp = random() % 36;
		if(tmp < 10){
			tmp += '0';
		}
		else{
			tmp -= 10;
			tmp += 'A';
		}
		buff +=tmp;
	}
	return buff;
}
```

## 设计模式

#### 单例模式

**Meyers's Singleton:**

```c++
/*  c++11后为线程安全
	这种单例解决了static变量初始化顺序问题：调用getInstance才会初始化，保证了顺序
	子类继承父类后，inst各子类是独立的（待确认）
*/
class Singleton {
public:
    static Singleton& getInstance() {
        static Singleton inst;
        return inst;
    }
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

    // 其他数据函数
    // ...

private:
    Singleton() { ... }
    // 其他数据成员
    // ...
};
```
