#### Decorator

#### Numpy和矢量化操作





#### Generator

Generator Function: 可以用def关键字加上yield关键字得到。

```python
def function_name( ) : 
	yield statement 
```

Generator funciton返回的是一个generator object，并且是iterable的，可以用作一个迭代器（iterator）。

当调用special methods比如next( )的时候，函数会执行到yield statement的地方；暂停函数的执行并且把yield的值返回给caller. 但是对比之下return会直接停止函数的执行；当函数被暂停之后，函数的状态还被保存，包括generator本地的variable bindings, instruction pointer, internal stack, exception handling. 当再次调用生成器methods的时候，函数执行就可以继续。

generator执行的时候只能遍历执行一次，在结束后for loop退出，如果继续调用next方法会提示stopIteration错误。

```python
def simpleGenerator( ) : 
	yield 1
  yield 2
  yield 3
 
x = simpleGenerator()
print(next(x))
print(next(x))
print(next(x))
```

https://hgzhao.github.io/2019/04/25/interviewquestion-004-die-dai-qi-sheng-cheng-qi/

#### Iterator



pdb调试



#### Context

#### Metaclass

#### LRU_Cache的使用

https://zhuanlan.zhihu.com/p/348370957

用法：从functools里import lru_cache

例子：计算factorial

```python
def factorial(n):
    print(f"计算 {n} 的阶乘")
    return 1 if n <= 1 else n * factorial(n - 1)

a = factorial(5)
print(f'5! = {a}')
b = factorial(3)
print(f'3! = {b}')


import functools
# 注意 lru_cache 后的一对括号，证明这是带参数的装饰器
@functools.lru_cache()
def factorial(n):
    print(f"计算 {n} 的阶乘")
    return 1 if n <= 1 else n * factorial(n - 1)
```

例子：leetcode 322 CoinChange

```python
from functools import lru_cache

class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:

        @lru_cache(None)
        def dfs(rem):
            if rem < 0:
                return -1
            if rem == 0:
                return 0
            min_cost = float('inf')
            for coin in coins:
                res = dfs(rem - coin)
                if res != -1:
                    min_cost = min(min_cost, res + 1)
            return min_cost if min_cost != float('inf') else -1

        return dfs(amount)
```

**被 lru_cache 修饰的函数在被相同参数调用的时候，后续的调用都是直接从缓存读结果，而不用真正执行函数。**

3.8版本之后，可以不带括号调用lru_cache

```python
@functools.lru_cache
```

#### Python自带的string方法

https://www.programiz.com/python-programming/methods/string/upper

- **upper( ) 变大写，没有input parameter**

```python
message = 'python is fun'
message.upper()
```

- #### stoi() 把str转换成integer

#### Bisect用法

bisect.bisect_left ( a, x, lo=0, hi = len(a))

在a中找到合适的插入点x维持有序。默认查找整个数组，分成a[lo: i ],  a[ i: hi]，其中左侧val < x， 右侧val >=x. 也就是说可以把x插入在i这个点。