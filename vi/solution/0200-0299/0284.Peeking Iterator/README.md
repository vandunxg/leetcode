---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Iterator
---

<!-- problem:start -->

# [284. Peeking Iterator](https://leetcode.com/problems/peeking-iterator)

[中文文档](/solution/0200-0299/0284.Peeking%20Iterator/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một iterator hỗ trợ thao tác <code>peek</code> trên iterator có sẵn, bên cạnh các thao tác <code>hasNext</code> và <code>next</code>.</p>

<p>Hãy triển khai class <code>PeekingIterator</code>:</p>

<ul>
	<li><code>PeekingIterator(Iterator&lt;int&gt; nums)</code> Khởi tạo đối tượng bằng iterator số nguyên đã cho <code>iterator</code>.</li>
	<li><code>int next()</code> Trả về phần tử tiếp theo trong mảng và chuyển pointer sang phần tử kế tiếp.</li>
	<li><code>boolean hasNext()</code> Trả về <code>true</code> nếu mảng vẫn còn phần tử.</li>
	<li><code>int peek()</code> Trả về phần tử tiếp theo trong mảng mà <strong>không</strong> chuyển pointer.</li>
</ul>

<p><strong>Lưu ý:</strong> Cách triển khai constructor và <code>Iterator</code> có thể khác nhau giữa các ngôn ngữ, nhưng tất cả đều hỗ trợ các hàm <code>int next()</code> và <code>boolean hasNext()</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;PeekingIterator&quot;, &quot;next&quot;, &quot;peek&quot;, &quot;next&quot;, &quot;next&quot;, &quot;hasNext&quot;]
[[[1, 2, 3]], [], [], [], [], []]
<strong>Đầu ra</strong>
[null, 1, 2, 2, 3, false]

<strong>Giải thích</strong>
PeekingIterator peekingIterator = new PeekingIterator([1, 2, 3]); // [<u><strong>1</strong></u>,2,3]
peekingIterator.next();    // trả về 1, pointer chuyển sang phần tử kế tiếp [1,<u><strong>2</strong></u>,3].
peekingIterator.peek();    // trả về 2, pointer không dịch chuyển [1,<u><strong>2</strong></u>,3].
peekingIterator.next();    // trả về 2, pointer chuyển sang phần tử kế tiếp [1,2,<u><strong>3</strong></u>]
peekingIterator.next();    // trả về 3, pointer chuyển sang phần tử kế tiếp [1,2,3]
peekingIterator.hasNext(); // trả về False
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li>Tất cả lời gọi <code>next</code> và <code>peek</code> đều hợp lệ.</li>
	<li>Sẽ có tối đa <code>1000</code> lời gọi đến <code>next</code>, <code>hasNext</code> và <code>peek</code>.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn sẽ mở rộng thiết kế này như thế nào để nó generic và dùng được với mọi kiểu dữ liệu, không chỉ số nguyên?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Gọi $next$ trên iterator được bọc sẽ lấy mất một giá trị, trong khi $peek$ không được làm vậy. Chỉ cần cache trước một phần tử.
>
> $peek$ lấy phần tử kế tiếp từ $next$ của iterator bên trong và lưu vào cache; $next$ trả về giá trị trong cache nếu có; $hasNext$ trả về true nếu cache có phần tử hoặc iterator bên trong vẫn còn phần tử.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Below is the interface for Iterator, which is already defined for you.
#
# class Iterator:
#     def __init__(self, nums):
#         """
#         Initializes an iterator object to the beginning of a list.
#         :type nums: List[int]
#         """
#
#     def hasNext(self):
#         """
#         Returns true if the iteration has more elements.
#         :rtype: bool
#         """
#
#     def next(self):
#         """
#         Returns the next element in the iteration.
#         :rtype: int
#         """


class PeekingIterator:
    def __init__(self, iterator):
        """
        Initialize your data structure here.
        :type iterator: Iterator
        """
        self.iterator = iterator
        self.has_peeked = False
        self.peeked_element = None

    def peek(self):
        """
        Returns the next element in the iteration without advancing the iterator.
        :rtype: int
        """
        if not self.has_peeked:
            self.peeked_element = self.iterator.next()
            self.has_peeked = True
        return self.peeked_element

    def next(self):
        """
        :rtype: int
        """
        if not self.has_peeked:
            return self.iterator.next()
        result = self.peeked_element
        self.has_peeked = False
        self.peeked_element = None
        return result

    def hasNext(self):
        """
        :rtype: bool
        """
        return self.has_peeked or self.iterator.hasNext()


# Your PeekingIterator object will be instantiated and called as such:
# iter = PeekingIterator(Iterator(nums))
# while iter.hasNext():
#     val = iter.peek()   # Get the next element but not advance the iterator.
#     iter.next()         # Should return the same value as [val].
```

#### Java

```java
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html

class PeekingIterator implements Iterator<Integer> {
    private Iterator<Integer> iterator;
    private boolean hasPeeked;
    private Integer peekedElement;

    public PeekingIterator(Iterator<Integer> iterator) {
        // initialize any member here.
        this.iterator = iterator;
    }

    // Returns the next element in the iteration without advancing the iterator.
    public Integer peek() {
        if (!hasPeeked) {
            peekedElement = iterator.next();
            hasPeeked = true;
        }
        return peekedElement;
    }

    // hasNext() and next() should behave the same as in the Iterator interface.
    // Override them if needed.
    @Override
    public Integer next() {
        if (!hasPeeked) {
            return iterator.next();
        }
        Integer result = peekedElement;
        hasPeeked = false;
        peekedElement = null;
        return result;
    }

    @Override
    public boolean hasNext() {
        return hasPeeked || iterator.hasNext();
    }
}
```

#### C++

```cpp
/*
 * Below is the interface for Iterator, which is already defined for you.
 * **DO NOT** modify the interface for Iterator.
 *
 *  class Iterator {
 *		struct Data;
 * 		Data* data;
 *  public:
 *		Iterator(const vector<int>& nums);
 * 		Iterator(const Iterator& iter);
 *
 *		// Returns the next element in the iteration.
 *		int next();
 *
 *		// Returns true if the iteration has more elements.
 *		bool hasNext() const;
 *	};
 */

class PeekingIterator : public Iterator {
public:
    PeekingIterator(const vector<int>& nums)
        : Iterator(nums) {
        // Initialize any member here.
        // **DO NOT** save a copy of nums and manipulate it directly.
        // You should only use the Iterator interface methods.
        hasPeeked = false;
    }

    // Returns the next element in the iteration without advancing the iterator.
    int peek() {
        if (!hasPeeked) {
            peekedElement = Iterator::next();
            hasPeeked = true;
        }
        return peekedElement;
    }

    // hasNext() and next() should behave the same as in the Iterator interface.
    // Override them if needed.
    int next() {
        if (!hasPeeked) return Iterator::next();
        hasPeeked = false;
        return peekedElement;
    }

    bool hasNext() const {
        return hasPeeked || Iterator::hasNext();
    }

private:
    bool hasPeeked;
    int peekedElement;
};
```

#### Go

```go
/*   Below is the interface for Iterator, which is already defined for you.
 *
 *   type Iterator struct {
 *
 *   }
 *
 *   func (this *Iterator) hasNext() bool {
 *		// Returns true if the iteration has more elements.
 *   }
 *
 *   func (this *Iterator) next() int {
 *		// Returns the next element in the iteration.
 *   }
 */

type PeekingIterator struct {
	iter          *Iterator
	hasPeeked     bool
	peekedElement int
}

func Constructor(iter *Iterator) *PeekingIterator {
	return &PeekingIterator{iter, iter.hasNext(), iter.next()}
}

func (this *PeekingIterator) hasNext() bool {
	return this.hasPeeked || this.iter.hasNext()
}

func (this *PeekingIterator) next() int {
	if !this.hasPeeked {
		return this.iter.next()
	}
	this.hasPeeked = false
	return this.peekedElement
}

func (this *PeekingIterator) peek() int {
	if !this.hasPeeked {
		this.peekedElement = this.iter.next()
		this.hasPeeked = true
	}
	return this.peekedElement
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
