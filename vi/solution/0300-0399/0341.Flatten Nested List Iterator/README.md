---
comments: true
difficulty: Medium
tags:
    - Stack
    - Tree
    - Depth-First Search
    - Design
    - Queue
    - Iterator
---

<!-- problem:start -->

# [341. Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator)

[中文文档](/solution/0300-0399/0341.Flatten%20Nested%20List%20Iterator/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách lồng nhau <code>nestedList</code> gồm các số nguyên. Mỗi phần tử là số nguyên hoặc một danh sách mà các phần tử bên trong cũng có thể là số nguyên hoặc danh sách khác. Hãy triển khai iterator để làm phẳng danh sách.</p>

<p>Hãy triển khai class <code>NestedIterator</code>:</p>

<ul>
	<li><code>NestedIterator(List&lt;NestedInteger&gt; nestedList)</code> khởi tạo iterator với danh sách lồng nhau <code>nestedList</code>.</li>
	<li><code>int next()</code> trả về số nguyên tiếp theo trong danh sách lồng nhau.</li>
	<li><code>boolean hasNext()</code> trả về <code>true</code> nếu danh sách lồng nhau vẫn còn số nguyên, nếu không thì trả về <code>false</code>.</li>
</ul>

<p>Mã của bạn sẽ được kiểm tra bằng pseudocode sau:</p>

<pre>
khởi tạo iterator với nestedList
res = []
while iterator.hasNext()
    thêm iterator.next() vào cuối res
trả về res
</pre>

<p>Nếu <code>res</code> khớp với danh sách phẳng mong đợi, lời giải của bạn được xem là đúng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nestedList = [[1,1],2,[1,1]]
<strong>Đầu ra:</strong> [1,1,2,1,1]
<strong>Giải thích:</strong> Gọi next liên tục cho đến khi hasNext trả về false, thứ tự các phần tử next trả về phải là: [1,1,2,1,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nestedList = [1,[4,[6]]]
<strong>Đầu ra:</strong> [1,4,6]
<strong>Giải thích:</strong> Gọi next liên tục cho đến khi hasNext trả về false, thứ tự các phần tử next trả về phải là: [1,4,6].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nestedList.length &lt;= 500</code></li>
	<li>Giá trị các số nguyên trong danh sách lồng nhau nằm trong khoảng <code>[-10<sup>6</sup>, 10<sup>6</sup>]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Làm phẳng các số nguyên lồng nhau để tạo iterator. Tìm leaf tiếp theo mỗi lần gọi `next` khiến việc quản lý cursor phức tạp hơn. Độ sâu có giới hạn nên ta có thể làm phẳng danh sách ngay trong constructor.
>
> DFS thêm các số nguyên vào kết quả và đệ quy với từng danh sách con. `next` và `hasNext` chỉ cần di chuyển index. Đổi lại, mỗi lần gọi chỉ tốn $O(1)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
# class NestedInteger:
#    def isInteger(self) -> bool:
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        """
#
#    def getInteger(self) -> int:
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        """
#
#    def getList(self) -> [NestedInteger]:
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        """


class NestedIterator:
    def __init__(self, nestedList: [NestedInteger]):
        def dfs(ls):
            for x in ls:
                if x.isInteger():
                    self.nums.append(x.getInteger())
                else:
                    dfs(x.getList())

        self.nums = []
        self.i = -1
        dfs(nestedList)

    def next(self) -> int:
        self.i += 1
        return self.nums[self.i]

    def hasNext(self) -> bool:
        return self.i + 1 < len(self.nums)


# Your NestedIterator object will be instantiated and called as such:
# i, v = NestedIterator(nestedList), []
# while i.hasNext(): v.append(i.next())
```

#### Java

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return empty list if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class NestedIterator implements Iterator<Integer> {
    private List<Integer> nums = new ArrayList<>();
    private int i = -1;

    public NestedIterator(List<NestedInteger> nestedList) {
        dfs(nestedList);
    }

    @Override
    public Integer next() {
        return nums.get(++i);
    }

    @Override
    public boolean hasNext() {
        return i + 1 < nums.size();
    }

    private void dfs(List<NestedInteger> ls) {
        for (var x : ls) {
            if (x.isInteger()) {
                nums.add(x.getInteger());
            } else {
                dfs(x.getList());
            }
        }
    }
}

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i = new NestedIterator(nestedList);
 * while (i.hasNext()) v[f()] = i.next();
 */
```

#### C++

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */

class NestedIterator {
public:
    NestedIterator(vector<NestedInteger>& nestedList) {
        auto dfs = [&](this auto&& dfs, vector<NestedInteger>& ls) -> void {
            for (auto& x : ls) {
                if (x.isInteger()) {
                    nums.push_back(x.getInteger());
                } else {
                    dfs(x.getList());
                }
            }
        };
        dfs(nestedList);
    }

    int next() {
        return nums[++i];
    }

    bool hasNext() {
        return i + 1 < nums.size();
    }

private:
    vector<int> nums;
    int i = -1;
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) cout << i.next();
 */
```

#### Go

```go
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * type NestedInteger struct {
 * }
 *
 * // Return true if this NestedInteger holds a single integer, rather than a nested list.
 * func (this NestedInteger) IsInteger() bool {}
 *
 * // Return the single integer that this NestedInteger holds, if it holds a single integer
 * // The result is undefined if this NestedInteger holds a nested list
 * // So before calling this method, you should have a check
 * func (this NestedInteger) GetInteger() int {}
 *
 * // Set this NestedInteger to hold a single integer.
 * func (n *NestedInteger) SetInteger(value int) {}
 *
 * // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 * func (this *NestedInteger) Add(elem NestedInteger) {}
 *
 * // Return the nested list that this NestedInteger holds, if it holds a nested list
 * // The list length is zero if this NestedInteger holds a single integer
 * // You can access NestedInteger's List element directly if you want to modify it
 * func (this NestedInteger) GetList() []*NestedInteger {}
 */

type NestedIterator struct {
	nums []int
	i    int
}

func Constructor(nestedList []*NestedInteger) *NestedIterator {
	var dfs func([]*NestedInteger)
	nums := []int{}
	i := -1
	dfs = func(ls []*NestedInteger) {
		for _, x := range ls {
			if x.IsInteger() {
				nums = append(nums, x.GetInteger())
			} else {
				dfs(x.GetList())
			}
		}
	}
	dfs(nestedList)
	return &NestedIterator{nums, i}
}

func (this *NestedIterator) Next() int {
	this.i++
	return this.nums[this.i]
}

func (this *NestedIterator) HasNext() bool {
	return this.i+1 < len(this.nums)
}
```

#### TypeScript

```ts
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *     If value is provided, then it holds a single integer
 *     Otherwise it holds an empty nested list
 *     constructor(value?: number) {
 *         ...
 *     };
 *
 *     Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     isInteger(): boolean {
 *         ...
 *     };
 *
 *     Return the single integer that this NestedInteger holds, if it holds a single integer
 *     Return null if this NestedInteger holds a nested list
 *     getInteger(): number | null {
 *         ...
 *     };
 *
 *     Set this NestedInteger to hold a single integer equal to value.
 *     setInteger(value: number) {
 *         ...
 *     };
 *
 *     Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
 *     add(elem: NestedInteger) {
 *         ...
 *     };
 *
 *     Return the nested list that this NestedInteger holds,
 *     or an empty list if this NestedInteger holds a single integer
 *     getList(): NestedInteger[] {
 *         ...
 *     };
 * };
 */

class NestedIterator {
    private nums: number[] = [];
    private i = -1;
    constructor(nestedList: NestedInteger[]) {
        const dfs = (ls: NestedInteger[]) => {
            for (const x of ls) {
                if (x.isInteger()) {
                    this.nums.push(x.getInteger());
                } else {
                    dfs(x.getList());
                }
            }
        };
        dfs(nestedList);
    }

    hasNext(): boolean {
        return this.i + 1 < this.nums.length;
    }

    next(): number {
        return this.nums[++this.i];
    }
}

/**
 * Your ParkingSystem object will be instantiated and called as such:
 * var obj = new NestedIterator(nestedList)
 * var a: number[] = []
 * while (obj.hasNext()) a.push(obj.next());
 */
```

#### Rust

```rust
// #[derive(Debug, PartialEq, Eq)]
// pub enum NestedInteger {
//   Int(i32),
//   List(Vec<NestedInteger>)
// }
struct NestedIterator {
    nums: Vec<i32>,
    i: usize,
}

/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl NestedIterator {
    fn new(nested_list: Vec<NestedInteger>) -> Self {
        let mut nums = Vec::new();
        Self::dfs(&nested_list, &mut nums);
        NestedIterator { nums, i: 0 }
    }

    fn next(&mut self) -> i32 {
        let result = self.nums[self.i];
        self.i += 1;
        result
    }

    fn has_next(&self) -> bool {
        self.i < self.nums.len()
    }

    fn dfs(nested_list: &Vec<NestedInteger>, nums: &mut Vec<i32>) {
        for ni in nested_list {
            match ni {
                NestedInteger::Int(x) => nums.push(*x),
                NestedInteger::List(list) => Self::dfs(list, nums),
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
