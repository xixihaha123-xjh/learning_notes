# list 双向链表

std::list支持从容器的任何位置快速插入和删除元素的序列容器，不支持随机访问。双向链表,头文件：<list> 

## 1.1 定义

```
template<class T, class Allocator = std::allocator<T>>
class list
{};
```

参数解释：

* T 元素类型，T必须满足可拷贝复制（CopyAssignable）和可拷贝构造（CopyConstructible）的要求 
* Allocator 用于获取/释放内存及构造/析构内存中元素的分配器

## 1.2 常用函数

### 1.2.1 front()

* 返回容器的第一个元素的引用。空容器上行为未定义

```c++
reference front()              // return first element of mutable sequence(可变序列)
{
	return (*begin());
}

const_reference front() const  // return first element of nonmutable sequence
{
	return (*begin());
}
```

### 1.2.2 back()

* 返回容器中最后一个元素的引用。空容器上行为未定义

```c++
reference back()                // return last element of mutable sequence(可变序列)
{
	return (*(--end()));
}

const_reference back() const    // return last element of nonmutable sequence(不可变序列)
{
	return (*(--end()));
}
```

### 1.2.3 empty()

* 返回容器是否为空

```c++
bool empty() const noexcept      // test if sequence is empty
{
	return (this->_Mysize() == 0);
}
```

### 1.2.4 size()

* 返回容器中元素的个数

```c++
size_type size() const noexcept  // return length of sequence
{
	return (this->_Mysize());
}
```

### 1.2.5 clear()

* 删除容器内所有元素。调用size() 返回0

```c++
void clear();
```

### 1.2.6 resize()

* 重设容器的大小为count个元素。如果当前size() 大于count小于count，则在尾部增加元素，并以_Val的副本初始化 

```c++
void resize(size_type _Newsize, const _Ty& _Val)
{	// determine new length, padding with _Val elements as needed
	if (this->_Mysize() < _Newsize)
		_Insert_n(_Unchecked_end(), _Newsize - this->_Mysize(), _Val);
	else
		while (_Newsize < this->_Mysize())
			pop_back();
}
```

### 1.2.7 insert()

* 插入元素到容器指定位置

```c++
// 在 _Where前插入 _Val
iterator insert(const_iterator _Where, const _Ty& _Val)                       // (1)
// 在 _Where前插入 _Val的_Count个副本
iterator insert(const_iterator _Where, size_type _Count, const _Ty& _Val)     // (2)
// 在 _Where前插入来自范围[first, last)的元素
template<class _Iter, class = enable_if_t<_Is_iterator_v<_Iter>>>             // (3) 
iterator insert(const_iterator _Where, _Iter _First, _Iter _Last)
```

### 1.2.8 emplace()



### 1.2.9 erase()

* 从容器内删除指定元素，返回被移除元素之后的迭代器

```c++
// 删除位于 _Where 的元素
iterator erase(const_iterator _Where);
// 删除范围[first, last)中的元素
iterator erase(const_iterator _First, const_iterator _Last);
```

### 1.2.10 push_back()

* 将元素_Val 添加到容器尾部

```c++
void push_back(_Ty&& _Val)                              // 右值移动
{	// insert element at end
	_Insert(_Unchecked_end(), _STD move(_Val));         // 移动构造
}
		
void push_back(const _Ty& _Val)
{	// insert element at end
	_Insert(_Unchecked_end(), _Val);
}
```

### 1.2.11 push_front()

* insert element at beginning    将元素value添加到容器的头部 

```c++
void push_front(_Ty&& _Val);

void push_front(const _Ty& _Val)
```

### 1.2.12 pop_back()

*  移除容器最后一个元素。在空容器上调用，行为未定义 

```c++
void pop_back()
{	// erase element at end
	erase(--end());
}
```

### 1.2.13 pop_front()

*  将元素value添加到容器的头部 

```c++
void pop_front()
{	// erase element at beginning
	erase(begin());
}
```

### 1.2.14 emplace_back()

*  添加新元素到容器尾部。通过std::allocator_traits::construct构造，参数args…以std::forward(args)…转发到构造函数。 

```c++
template<class... _Valty>
decltype(auto) emplace_back(_Valty&&... _Val)
{	// insert element at end
	_Insert(_Unchecked_end(), _STD forward<_Valty>(_Val)...);
#if _HAS_CXX17
		return (back());
#endif /* _HAS_CXX17 */
}
```

### 1.2.15 emplace_front()

*  插入元素到容器开始，通过std::allocator_traits::construct构造元素,将参数args…作为std::forward(args)…转发给构造函数 

```c++
template<class... _Valty>
decltype(auto) emplace_front(_Valty&&... _Val)
{	// insert element at beginning
	_Insert(_Unchecked_begin(), _STD forward<_Valty>(_Val)...);

#if _HAS_CXX17
		return (front());
#endif /* _HAS_CXX17 */
}
```

### 1.2.16 swap()

*  将内容和_Right的交换 

```c++
void swap(list& _Right) noexcept // strengthened
// exchange contents with _Right 与_Right交换内容
```

### 1.2.17 merge()

* 合并两个已排序链表为一个。操作后 _Right变为空。(1) (2) 使用 operator < 合并，(3)  (4) 使用 _Pred 合并

```c++
// merge in elements from _Right, both ordered by operator <
void merge(list& _Right);                                                     // (1)
void merge(list&& _Right);                                                    // (2)

// merge in elements from _Right, both ordered by _Pred
template<class _Pr2>
void merge(list& _Right, _Pr2 _Pred);                                         // (3)

template<class _Pr2>
void merge(list&& _Right, _Pr2 _Pred);                                        // (4)
```

### 1.2.18 splice()

*  从一个list转移元素给另一个。不复制或者移动元素，仅重值向链表节点的内部指针。 

> *  从other转移所有元素到 *this中，元素被插入到pos指向的元素之前。操作后容器other变成空。如果other和 *this指代同一对象则行为未定义 
>
> * 从other转移it所指代的元素到*this。元素被插入到pos所指向的元素之前。 
>
> * 从other转移范围[first, last)中的元素到*this。元素被插入到pos所指向的元素之前。如果pos是范围[first, last)中的迭代器则行为未定义 

```c++
// splice all of _Right at _Where
void splice(const_iterator _Where, list&& _Right);
// splice all of _Right at _Where
void splice(const_iterator _Where, list& _Right);
// splice _Right [_First, _First + 1) at _Where
void splice(const_iterator _Where, list&& _Right, const_iterator _First);
// splice _Right [_First, _First + 1) at _Where
void splice(const_iterator _Where, list& _Right, const_iterator _First);
// splice _Right [_First, _Last) at _Where
void splice(const_iterator _Where, list&& _Right, const_iterator _First, const_iterator _Last);
// splice _Right [_First, _Last) at _Where
void splice(const_iterator _Where, list& _Right, const_iterator _First, const_iterator _Last);
```

