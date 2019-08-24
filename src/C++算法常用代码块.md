### C++算法常用代码块

#### 输入输出

- 循环读入

```C++
int a, b;
while (cin >> a >> b) {
    cout << a << " " << b << endl;
}
```

```C++
Input:
1 2
2 3
3 4
4
Ctrl+D

Output:
1 2
2 3
3 4
```

- 读取一行

```C++
string line;
getline(cin, line);
```

- 读取一行整数

```C++
string line;
getline(cin, line);
istringstream iss(s);
int tmp;
while (iss >> tmp)
	cout << tmp << endl;
```

```C++
Input:
1 2 3 4 5
Ctrl+D

Output:
1
2
3
4
5
```

- 读取一行自定义分隔符，可用于字符串分割

```C++
string s;
while (getline(cin, s, ','))
    cout << s << endl;
```

```
Input:
a, b, c, d, e
Ctrl+D

Output:
a
b
c
d
e
```

字符串分割

```C++
char split = ',';
string s = "a,b,c,d,e";
istringstream iss(s); //转换成输入流
string tmp;
while (getline(iss, tmp, split)) {
	cout << tmp << endl;
}
```

```C++
Output:
a
b
c
d
e
```

#### vector常用操作

| 方法名                                                       | 作用                     |
| ------------------------------------------------------------ | ------------------------ |
| [**push_back**](http://www.cplusplus.com/reference/vector/vector/push_back/) | 在末尾添加元素           |
| [**push_back**](http://www.cplusplus.com/reference/vector/vector/push_back/) | 删除末尾的预算，无返回值 |
| [**front**](http://www.cplusplus.com/reference/vector/vector/front/) | 获取第一个元素           |
| [**back**](http://www.cplusplus.com/reference/vector/vector/back/) | 获取最后一个元素         |
| [**size**](http://www.cplusplus.com/reference/vector/vector/size/) | 获取大小                 |
| [**empty**](http://www.cplusplus.com/reference/vector/vector/empty/) | 判断是否为空             |

- 初始化

```C++
vector<int>(n); // 初始化为指定大小
vector<int>(n, 1); // 初始化为大小为n 初始元素全部为1
```

- 迭代器使用

C++中的迭代器相当于指针，获取元素时需要解引用，其中`end()`指向最后一个元素的下一个位置，一般不解引用

```C++
int myints[] = {16,2,77,29};
vector<int> v(myints, myints + sizeof(myints) / sizeof(int));
for (vector<int>::iterator it = v.begin(); it < v.end(); ++it)
    cout << *it << " ";
```

```C++
Output:
16 2 77 29
```

> C++中的范围一般使用的是半闭区间，包括开头，不包括结尾
>
> ```C++
> int myints[] = {16,2,77,29};
> vector<int> v(myints, myints + sizeof(myints) / sizeof(int));
> sort(v.begin(), v.begin()+3);
> for (vector<int>::iterator it = v.begin(); it < v.end(); ++it)
>     cout << *it << " ";
> ```
>
> ```C++
> Output:
> 2 16 77 29
> ```

`vector<int>::iterator` 简化

```C++
typedef vector<int>::iterator iter;
for (iter it = v.begin(); it < v.end(); ++it)
    cout << *it << " ";
    
// C++11
for (auto it = v.begin(); it < v.end(); ++it)
    cout << *it << " ";
```

#### string常用操作

- 初始化

  - 默认空字符串

    ```C++
    string();
    ```

  - 从另个字符串初始化

    ```C++
    string (const string& str);
    ```

  - 指定大小和填充字符

    ```C++
    string (size_t n, char c);
    ```

| 方法名                                                       | 作用                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [**substr**](http://www.cplusplus.com/reference/string/string/substr/) | 获取子串<br />`string substr (size_t pos = 0, size_t len = npos) const;`<br />`pos`表示起始位置，`len`表示截取长度 |
| [**find_first_of**](http://www.cplusplus.com/reference/string/string/find_first_of/) | 找匹配子串的第一个位置                                       |

| 函数名                                                       | 作用              |
| ------------------------------------------------------------ | ----------------- |
| [**stoi** ](http://www.cplusplus.com/reference/string/stoi/) | 字符串转int       |
| [**stol** ](http://www.cplusplus.com/reference/string/stol/) | 字符串转long      |
| [**stoll** ](http://www.cplusplus.com/reference/string/stoll/) | 字符串转long long |
| [**stof** ](http://www.cplusplus.com/reference/string/stof/) | 字符串转float     |
| [**stod** ](http://www.cplusplus.com/reference/string/stod/) | 字符串转double    |
| [**to_string** ](http://www.cplusplus.com/reference/string/to_string/) | 数值型转字符串    |

#### stack常用操作

| 方法名                                                       | 作用           |
| ------------------------------------------------------------ | -------------- |
| [**push**](http://www.cplusplus.com/reference/stack/stack/push/) | 作用入栈       |
| [**pop**](http://www.cplusplus.com/reference/stack/stack/pop/) | 出栈           |
| [**top**](http://www.cplusplus.com/reference/stack/stack/top/) | 获取栈顶元素   |
| [**size**](http://www.cplusplus.com/reference/stack/stack/size/) | 栈中元素数量   |
| [**empty**](http://www.cplusplus.com/reference/stack/stack/empty/) | 判断栈是否为空 |

#### unordered_map常用操作

[map和unordered_map区别](https://blog.csdn.net/batuwuhanpei/article/details/50727227)

| 方法名                                                       | 作用                   |
| ------------------------------------------------------------ | ---------------------- |
| [**find**](http://www.cplusplus.com/reference/unordered_map/unordered_map/find/) | 根据键查找，返回迭代器 |
| [**erase**](http://www.cplusplus.com/reference/unordered_map/unordered_map/erase/) | 删除指定键的元素       |

- 插入和更新

1. 对于初始化后的`map`，可以直接通过`[]`来插入和更新元素；对于`value`是整型时，使用`map[k]++`，若`k`不存在，则从0开始递增
2. 使用`insert`插入`pair`，`pair`可以通过`make_pair`构造

```C++
int main() {
    unordered_map<int, int> map;
    int k, v;
    while (cin >> k >> v) {
        map[k] = v;
    }
    map.insert(make_pair(1, 2));
    for (auto it = map.begin(); it != map.end(); ++it)
        cout << it->first << " " << it->second << endl;
    return 0;
}
```

```C++
Input:
2 3
3 4
4 5
5 6
Ctrl+D

Output:
1 2
2 3
3 4
4 5
5 6
```

- 迭代器

通过`it->first` 获取`key`，`it->second`获取`value`

```C++
for (auto it = map.begin(); it != map.end(); ++it)
    cout << it->first << " " << it->second << endl;
```

#### unordered_set常用操作

| 方法名                                                       | 作用                             |
| ------------------------------------------------------------ | -------------------------------- |
| [**insert**](http://www.cplusplus.com/reference/unordered_set/unordered_set/insert/) | 插入元素，只会插入不存在的元素   |
| [**erase**](http://www.cplusplus.com/reference/unordered_set/unordered_set/erase/) | 删除元素                         |
| [**clear**](http://www.cplusplus.com/reference/unordered_set/unordered_set/clear/) | 清空集合，删除所有元素           |
| [**find**](http://www.cplusplus.com/reference/unordered_set/unordered_set/find/) | 查找某个元素是否存在，返回迭代器 |
| [**size**](http://www.cplusplus.com/reference/unordered_set/unordered_set/size/) | 集合元素个数                     |
| [**empty**](http://www.cplusplus.com/reference/unordered_set/unordered_set/empty/) | 集合是否为空                     |

#### queue常用操作

| 方法名                                                       | 作用         |
| ------------------------------------------------------------ | ------------ |
| [**push**](http://www.cplusplus.com/reference/queue/queue/push/) | 队尾插入元素 |
| [**pop**](http://www.cplusplus.com/reference/queue/queue/pop/) | 删除队头元素 |
| [**front**](http://www.cplusplus.com/reference/queue/queue/front/) | 获取队头元素 |
| [**back**](http://www.cplusplus.com/reference/queue/queue/back/) | 获取队尾元素 |
| [**size**](http://www.cplusplus.com/reference/queue/queue/size/) | 队列元素个数 |
| [**empty**](http://www.cplusplus.com/reference/queue/queue/empty/) | 队列是否为空 |

#### algorithm常用操作

| 函数名                                                       | 作用                                               |
| ------------------------------------------------------------ | -------------------------------------------------- |
| [**min**](http://www.cplusplus.com/reference/algorithm/min/) | 获取两个值中的最小值                               |
| [**max**](http://www.cplusplus.com/reference/algorithm/max/) | 获取两个值中的最大值                               |
| [**swap**](http://www.cplusplus.com/reference/algorithm/swap/) | 交换元素                                           |
| [**min_element**](http://www.cplusplus.com/reference/algorithm/min_element/) | 获取给定范围的最小值，半开区间，不包括最后一个元素 |
| [**max_element**](http://www.cplusplus.com/reference/algorithm/max_element/) | 获取给定范围的最大值，半开区间，不包括最后一个元素 |
| [**sort**](http://www.cplusplus.com/reference/algorithm/sort/) | 排序                                               |
| [**reverse**](http://www.cplusplus.com/reference/algorithm/reverse/) | 反转数组                                           |
| [**find**](http://www.cplusplus.com/reference/algorithm/find/) | 查找元素                                           |

```C++
int a, b;
cin >> a >> b;
cout << min(a, b) << " " << max(a, b) << endl;
swap(a, b);
cout << a << " " << b << endl;
cout << min(a, b) << " " << max(a, b) << endl;
```

```C++
Input:
5 6

Output:
5 6
6 5
5 6
```


```C++
int tmp;
vector<int> v;
while (cin >> tmp) {
    v.push_back(tmp);
    cout << tmp << " ";
}
cout << endl;

cout << *min_element(v.begin(), v.end()) << " " << *max_element(v.begin(), v.end()) << endl;

sort(v.begin(), v.end());
for (auto it = v.begin(); it < v.end(); ++it)
    cout << *it << " ";
cout << endl;

reverse(v.begin(), v.end());
for (auto it = v.begin(); it < v.end(); ++it)
    cout << *it << " ";
cout << endl;

if (find(v.begin(), v.end(), 1) != v.end())
    cout << "yes" << endl;
else
    cout << "no" << endl;
```

```C++
Input:
1 2 3 4 9 8 7 6 5
Ctrl+D

Output:
1 2 3 4 9 8 7 6 5
1 9
1 2 3 4 5 6 7 8 9 
9 8 7 6 5 4 3 2 1 
yes
```

> 排序自定义比较函数 (`void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);`)
>
> 排序默认使用的是递增排序，`comp`默认是`less`, 可以使用`greater`进行递减排序
>
> ```C++
> int tmp;
> vector<int> v;
> while (cin >> tmp) {
>     v.push_back(tmp);
>     cout << tmp << " ";
> }
> cout << endl;
> 
> sort(v.begin(), v.end(), greater<>());
> for (auto it = v.begin(); it < v.end(); ++it)
>     cout << *it << " ";
> cout << endl;
> ```
>
> ```C++
> Input:
> 1 2 3 4 9 8 7 6 5
> Ctrl+D
> 
> Output:
> 1 2 3 4 9 8 7 6 5
> 9 8 7 6 5 4 3 2 1
> ```
>
> 自定义比较函数
>
> ```C++
> // 相当于greater
> bool comp(int a, int b) {
>     return a > b; // 返回true时，a放置在b前面
> }
> 
> int main() {
>     int tmp;
>     vector<int> v;
>     while (cin >> tmp) {
>         v.push_back(tmp);
>         cout << tmp << " ";
>     }
>     cout << endl;
> 
>     sort(v.begin(), v.end(), comp);
>     for (auto it = v.begin(); it < v.end(); ++it)
>         cout << *it << " ";
>     cout << endl;
>     return 0;
> }
> ```
>
> ```C++
> Input:
> 1 2 3 4 9 8 7 6 5
> Ctrl+D
> 
> Output:
> 1 2 3 4 9 8 7 6 5
> 9 8 7 6 5 4 3 2 1
> ```



