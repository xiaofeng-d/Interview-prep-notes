

## Inheritance ##

- Object-oriented programming is based on:

 		**encapsulation, abstraction, inheritance, polymorphism**



## Virtual Function ##



1. Virtual function implements **polymorphism**;  it makes sure that derived class function gets called, even when called with base class pointer reference.

Example:  W/ virtual function: "Tuna Swims";  W/o virtual function: "Fish Swims".

```
#include<iostream>
using namespace std;

class Fish
{
public: 
     virtual void Swim () //virtual function
{ cout << "Fish Swims " << endl; }

 };

class Tuna:public Fish
{
public:

 	void Swim( ) 
{ cout << "Tuna Swims " << endl;
}

};

void Fishswim( Fish& inputFish )
{
  inputFish.Swim();
}

int main()
{
	Tuna object;
	Fishswim(object); 
	return 0;
}


```

2. Virtual Destructor ensures that derived class destructor is always invoked if base class destructor is

```
#include <iostream>
using namespace std;

class Vehicle
{
public:
    Vehicle(){ cout << "Vehicle constructor! " << endl; }
    virtual ~Vehicle(){ cout << "Vehicle destructor ! " << endl; } //ADDING VIRTUAL OR NOT MAKES A DIFFERENCE!
};

class Car: public Vehicle
{
public:
    Car(){ cout << "Car constructor! " << endl; }
    ~Car(){ cout << "Car destructor ! " << endl; }
};




int main() {
    std::cout << "Hello, World!" << std::endl;
    Vehicle* pMyRacer = new Car;
    delete pMyRacer;

    return 0;
}
```



## C++主要语法复习

### Constant关键字



## 杂项笔记

#### IOTA function的用法

iota是c++ 11引入的函数，希腊语的第九个字母。相当于用value递增的数列给[first, last) 的容器赋值。需要引入<numeric>.

用法示例：

```c++
#include<vector>
#include<iostream>
#include <numeric>  
std::vector<int> nums(10);
for (int i: nums){
  std::cout<< i << "\t";
}
std::cout << std::endl;

std::iota(nums.begin(), nums.end(), 5);   // fill nums with 5,6,7, 8, ..
```

#### Explicit关键字

C++的explicit关键字只用来修饰只有一个参数的类构造函数，表明它是显式的。与它对应的关键字是implicit，隐藏的。类构造函数默认情况下是implicit的。

如果构造函数只有一个参数时，编译时会有一个缺省的转换操作，将构造函数对应数据类型的数据转换为该类的对象。

```c++
class CxString
{
  public:
  	char * _pstr;
		int _size;
  	CxString(int size)
    {
      _size = size;
      _pstr = malloc(size+1);
      memset(_pstr, 0, size + 1);
    }
    	CxString(const char *p)
    {
      int size = strlen(p);
      _pstr = malloc(size+1);
      strcpy(_pstr, p);
      _size = strlen(_pstr);
    }
}

CsString string2 = 10;
```

如果不加入explicit关键词限制constructor，在调用CsString string2= 10时，会把构造函数对应数据类型的**数据**转换为该类对象。加入explicit，会禁止类构造函数的隐式自动转换。当constructor有一个以上的参数的时候，本身就不会产生隐式转换，所以explicit关键词没有效果。

例外情况是：如果constructor有一个以上参数，但是除了第一个之外全部有默认值的话，则explicit也会产生效果。



#### C++迭代器

**1 正向迭代器，定义方法如下：**

容器类名::iterator 迭代器名;

**2 常量正向迭代器，定义方法如下：**

容器类名::const_iterator 迭代器名;

**3 反向迭代器，定义方法如下：**

容器类名::reverse_iterator 迭代器名;

**4 常量反向迭代器，定义方法如下：**

容器类名::const_reverse_iterator 迭代器名;

```c++
#include <iostream>
#include <vector>
using namespace std;
int main()
{
    vector<int> v;  //v是存放int类型变量的可变长数组，开始时没有元素
    for (int n = 0; n<5; ++n)
        v.push_back(n);  //push_back成员函数在vector容器尾部添加一个元素
    vector<int>::iterator i;  //定义正向迭代器
    for (i = v.begin(); i != v.end(); ++i) {  //用迭代器遍历容器
        cout << *i << " ";  //*i 就是迭代器i指向的元素
        *i *= 2;  //每个元素变为原来的2倍
    }
    cout << endl;
    //用反向迭代器遍历容器
    for (vector<int>::reverse_iterator j = v.rbegin(); j != v.rend(); ++j)
        cout << *j << " ";
    return 0;
}
```

#### C++ Range Based for loop

C++11的新特性，可以不用写出开头和结尾条件，可读性变得更高。

```c++
std::vector<int> vec{1,2,3,4,5,6,7,8,9,10};
for(auto n: vec)
  std::cout << n;
  
```



#### C++ Vectors

初始化vectors：

获得大小：vecSize = v.size();

遍历：for( int i = 0;  i < vecSize; i++) { cout << v[i] << endl; }

在前面插入元素：v.insert( v.begin(), "apple" )

vector::begin()回复的是**iterator** pointing to the first element of vector container.

```c++
#include<vector>

int main()
{
  vector<string> v {"banana", "cherry", "mango"};
  v.insert(v.begin(), "apple")
  for(auto& element: fruits){
		cout << element << endl;
}
}
```

