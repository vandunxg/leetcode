---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [775. Global and Local Inversions](https://leetcode.com/problems/global-and-local-inversions)

[中文文档](/solution/0700-0799/0775.Global%20and%20Local%20Inversions/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> có độ dài <code>n</code>, biểu diễn một hoán vị của tất cả số nguyên trong khoảng <code>[0, n - 1]</code>.</p>

<p>Số lượng <strong>nghịch thế toàn cục</strong> là số cặp <code>(i, j)</code> khác nhau thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; n</code></li>
	<li><code>nums[i] &gt; nums[j]</code></li>
</ul>

<p>Số lượng <strong>nghịch thế cục bộ</strong> là số chỉ số <code>i</code> thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= i &lt; n - 1</code></li>
	<li><code>nums[i] &gt; nums[i + 1]</code></li>
</ul>

<p>Trả về <code>true</code> <em>nếu số lượng <strong>nghịch thế toàn cục</strong> bằng số lượng <strong>nghịch thế cục bộ</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,0,2]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có 1 nghịch thế toàn cục và 1 nghịch thế cục bộ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,0]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Có 2 nghịch thế toàn cục và 1 nghịch thế cục bộ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt; n</code></li>
	<li>Tất cả số nguyên trong <code>nums</code> đều <strong>khác nhau</strong>.</li>
	<li><code>nums</code> là hoán vị của tất cả các số trong khoảng <code>[0, n - 1]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xác định số nghịch thế toàn cục có bằng số nghịch thế giữa các phần tử kề nhau hay không. $n\le 10^5$. Tương đương, không có nghịch thế nào giữa hai vị trí cách nhau hơn $1$.
>
> Không tồn tại $i\le j-2$ sao cho $nums[i]>nums[j]$. Theo dõi giá trị lớn nhất của tiền tố $nums[0..i-2]$; nếu nó lớn hơn $nums[i]$ thì trả về false.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isIdealPermutation(self, nums: List[int]) -> bool:
        mx = 0
        for i in range(2, len(nums)):
            if (mx := max(mx, nums[i - 2])) > nums[i]:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean isIdealPermutation(int[] nums) {
        int mx = 0;
        for (int i = 2; i < nums.length; ++i) {
            mx = Math.max(mx, nums[i - 2]);
            if (mx > nums[i]) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isIdealPermutation(vector<int>& nums) {
        int mx = 0;
        for (int i = 2; i < nums.size(); ++i) {
            mx = max(mx, nums[i - 2]);
            if (mx > nums[i]) return false;
        }
        return true;
    }
};
```

#### Go

```go
func isIdealPermutation(nums []int) bool {
	mx := 0
	for i := 2; i < len(nums); i++ {
		mx = max(mx, nums[i-2])
		if mx > nums[i] {
			return false
		}
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng điều kiện tương đương rút gọn. Nếu đếm trực tiếp: xét các cặp kề nhau để đếm nghịch thế cục bộ, và dùng Fenwick tree để đếm nghịch thế toàn cục.
>
> Theo dõi hiệu giữa hai số đếm và trả về false nếu số nghịch thế toàn cục vượt quá số nghịch thế cục bộ. Độ phức tạp tăng thêm hệ số $\log n$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x, delta):
        while x <= self.n:
            self.c[x] += delta
            x += x & -x

    def query(self, x):
        s = 0
        while x:
            s += self.c[x]
            x -= x & -x
        return s


class Solution:
    def isIdealPermutation(self, nums: List[int]) -> bool:
        n = len(nums)
        tree = BinaryIndexedTree(n)
        cnt = 0
        for i, v in enumerate(nums):
            cnt += i < n - 1 and v > nums[i + 1]
            cnt -= i - tree.query(v)
            if cnt < 0:
                return False
            tree.update(v + 1, 1)
        return True
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public boolean isIdealPermutation(int[] nums) {
        int n = nums.length;
        BinaryIndexedTree tree = new BinaryIndexedTree(n);
        int cnt = 0;
        for (int i = 0; i < n && cnt >= 0; ++i) {
            cnt += (i < n - 1 && nums[i] > nums[i + 1] ? 1 : 0);
            cnt -= (i - tree.query(nums[i]));
            tree.update(nums[i] + 1, 1);
        }
        return cnt == 0;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
public:
    BinaryIndexedTree(int _n)
        : n(_n)
        , c(_n + 1) {}

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    int query(int x) {
        int s = 0;
        while (x) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }

private:
    int n;
    vector<int> c;
};

class Solution {
public:
    bool isIdealPermutation(vector<int>& nums) {
        int n = nums.size();
        BinaryIndexedTree tree(n);
        long cnt = 0;
        for (int i = 0; i < n && ~cnt; ++i) {
            cnt += (i < n - 1 && nums[i] > nums[i + 1]);
            cnt -= (i - tree.query(nums[i]));
            tree.update(nums[i] + 1, 1);
        }
        return cnt == 0;
    }
};
```

#### Go

```go
func isIdealPermutation(nums []int) bool {
	n := len(nums)
	tree := newBinaryIndexedTree(n)
	cnt := 0
	for i, v := range nums {
		if i < n-1 && v > nums[i+1] {
			cnt++
		}
		cnt -= (i - tree.query(v))
		if cnt < 0 {
			break
		}
		tree.update(v+1, 1)
	}
	return cnt == 0
}

type BinaryIndexedTree struct {
	n int
	c []int
}

func newBinaryIndexedTree(n int) BinaryIndexedTree {
	c := make([]int, n+1)
	return BinaryIndexedTree{n, c}
}

func (this BinaryIndexedTree) update(x, delta int) {
	for x <= this.n {
		this.c[x] += delta
		x += x & -x
	}
}

func (this BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s += this.c[x]
		x -= x & -x
	}
	return s
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
