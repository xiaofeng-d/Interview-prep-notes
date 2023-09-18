

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

https://www.geeksforgeeks.org/range-based-loop-c/

C++11的新特性，可以不用写出开头和结尾条件，可读性变得更高。

```c++
for ( range_declaration : range_expression ) 
    loop_statement
```

```c++
std::vector<int> vec{1,2,3,4,5,6,7,8,9,10};
for(auto i: vec)
  std::cout << i;

// Iterating over whole array
std::vector<int> v = { 0, 1, 2, 3, 4, 5 };
for (auto i : v)
    std::cout << i << ' ';

// the initializer may be a braced-init-list
for (int n : { 0, 1, 2, 3, 4, 5 })
    std::cout << n << ' ';

// Iterating over array
int a[] = { 0, 1, 2, 3, 4, 5 };
for (int n : a)
    std::cout << n << ' ';

// Just running a loop for every array
// element
for (int n : a)
    std::cout << "In loop" << ' ';

// Printing string characters
std::string str = "Geeks";
for (char c : str)
    std::cout << c << ' ';

// Printing keys and values of a map
std::map<int, int> MAP(
    { { 1, 1 }, { 2, 2 }, { 3, 3 } });
for (auto i : MAP)
    std::cout << '{' << i.first << ", " << i.second
              << "}\n";
}

```

对比常见写法：



```c++
std::vector<int> vec{1,2,3,4,5,6,7,8,9,10};
for(int i=0; i<10; i++)
  std::cout << vec[i];
  
```

C++ 17新特性：

```c++
for (auto& [key, value]: myMap) {
    cout << key << " has value " << value << std::endl;
}
```



#### C++ String

![Strings in C++](https://media.geeksforgeeks.org/wp-content/uploads/20221229144929/Strings-in-Cpp.png)

C style strings是array of characters, 最后用 '\0'结束。

C++ strings是 std::string class中的

#### C++ Vectors

初始化vectors：

获得大小：vecSize = v.size();

遍历：for( int i = 0;  i < vecSize; i++) { cout << v[i] << endl; }

在前面插入元素：v.insert( v.begin(), "apple" )

vector::begin()回复的是**iterator** 指向向量容器里的第一个元素。 pointing to the first element of vector container.

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

#### C++字典的用法

unordered_map

注意内建的hash function无法hash的一些数据结构：pair是无法用默认hash函数直接hash，除非自己定义一个hash function for pair。



```c++
#include <unordered_map>

unordered_map<string, int> umap;
umap["GeeksforGeeks"] = 10;
uamp["Contribute"] = 20;
umap["Practice"] = 30;

// 遍历
for (auto x : umap)
  cout << x.first << " " << 
  			x.second << endl;

//相关函数
//查找一个key, find，问是否能找到
if( umap.find(key) == umap.end())
  cout << key << " not found " ;
//查找一个key, count，问找不找得到
if ( umap.count(key) == 0  )
  cout << key << " not found " ;
//

//传统写法，用iterator遍历
  unordered_map<string, int>:: iterator p;
  for (p = wordFreq.begin(); 
       p != wordFreq.end(); p++)
    cout << "(" << p->first << ", " <<
                   p->second << ")\n";
}

```



#### C++ pair

```c++
#include <utility>
using namespace std;
pair<int, string> anon_pair;
anon_pair.first = 17;   //
anon_pair.second = "seventeen";
```

用法：

#### C++ priority queue

```c++
#include <iostream>
#include <queue>

priority_queue<int> pq;


priority_queue <int, vector<int>, greater<int>> gq;
```

#### Static关键字用于修饰成员函数/成员变量

#### Static Member Function in C++

Static Member Function in a class is the function that is declared as static because of which function attains certain properties as defined below:

- A static member function is independent of any object of the class. 
- A static member function can be called even if no objects of the class exist.
- A static member function can also be accessed using the class name through the scope resolution operator.
- A static member function can access static data members and static member functions inside or outside of the class.
- Static member functions have a scope inside the class and cannot access the current object pointer.
- You can also use a static member function to determine how many objects of the class have been created.

#### C++ Lambda Function

格式：

```c++
[ capture clause ] (parameters) -> return-type  
{   
   definition of method   
} 

vector<int> v {4, 1, 3, 5, 2, 3, 1, 7};

// function to sort vector, lambda expression is for sorting in
// non-increasing order Compiler can make out return type as
// bool, but shown here just for explanation
sort(v.begin(), v.end(), [](const int& a, const int& b) -> bool
{
    return a > b;
});
```

#### C++ mutable关键字

有时候希望class的成员用constant function修改，但是不想让它修改其他成员。

如果把一个function声明为const，那么传给它的this指针就变成const 指针。当一个变量成为mutable的时候，就会允许const pointer改变成员。
