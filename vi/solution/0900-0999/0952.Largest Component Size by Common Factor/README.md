---
comments: true
difficulty: Hard
tags:
    - Union Find
    - Array
    - Hash Table
    - Math
    - Number Theory
    - Prime Factorization
---

<!-- problem:start -->

# [952. Largest Component Size by Common Factor](https://leetcode.com/problems/largest-component-size-by-common-factor)

[中文文档](/solution/0900-0999/0952.Largest%20Component%20Size%20by%20Common%20Factor/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên dương đôi một khác nhau <code>nums</code>. Xét đồ thị sau:</p>

<ul>
	<li>Đồ thị có <code>nums.length</code> node, được gắn nhãn từ <code>nums[0]</code> đến <code>nums[nums.length - 1]</code>.</li>
	<li>Có cạnh vô hướng nối <code>nums[i]</code> và <code>nums[j]</code> nếu chúng có ước chung lớn hơn <code>1</code>.</li>
</ul>

<p>Trả về <em>kích thước của thành phần liên thông lớn nhất trong đồ thị</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0952.Largest%20Component%20Size%20by%20Common%20Factor/images/ex1.png" style="width: 500px; height: 97px;" />
<pre>
<strong>Input:</strong> nums = [4,6,15,35]
<strong>Output:</strong> 4
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0952.Largest%20Component%20Size%20by%20Common%20Factor/images/ex2.png" style="width: 500px; height: 85px;" />
<pre>
<strong>Input:</strong> nums = [20,50,9,63]
<strong>Output:</strong> 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0952.Largest%20Component%20Size%20by%20Common%20Factor/images/ex3.png" style="width: 500px; height: 260px;" />
<pre>
<strong>Input:</strong> nums = [2,3,6,7,4,12,21,39]
<strong>Output:</strong> 8
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li>Tất cả giá trị trong <code>nums</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các số có ước chung lớn hơn $1$ được nối với nhau; ta cần tìm thành phần lớn nhất. Kiểm tra $\gcd$ từng cặp có độ phức tạp bậc hai, quá chậm với $n\le 2\times 10^4$. Dùng union để nối mỗi giá trị với các ước của nó, nhờ đó các số có chung ước sẽ thuộc cùng thành phần. Đáp án là tần suất lớn nhất của các root tương ứng với các giá trị đầu vào.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self, n):
        self.p = list(range(n))

    def union(self, a, b):
        pa, pb = self.find(a), self.find(b)
        if pa != pb:
            self.p[pa] = pb

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]


class Solution:
    def largestComponentSize(self, nums: List[int]) -> int:
        uf = UnionFind(max(nums) + 1)
        for v in nums:
            i = 2
            while i <= v // i:
                if v % i == 0:
                    uf.union(v, i)
                    uf.union(v, v // i)
                i += 1
        return max(Counter(uf.find(v) for v in nums).values())
```

#### Java

```java
class UnionFind {
    int[] p;

    UnionFind(int n) {
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
    }

    void union(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa != pb) {
            p[pa] = pb;
        }
    }

    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}

class Solution {
    public int largestComponentSize(int[] nums) {
        int m = 0;
        for (int v : nums) {
            m = Math.max(m, v);
        }
        UnionFind uf = new UnionFind(m + 1);
        for (int v : nums) {
            int i = 2;
            while (i <= v / i) {
                if (v % i == 0) {
                    uf.union(v, i);
                    uf.union(v, v / i);
                }
                ++i;
            }
        }
        int[] cnt = new int[m + 1];
        int ans = 0;
        for (int v : nums) {
            int t = uf.find(v);
            ++cnt[t];
            ans = Math.max(ans, cnt[t]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    vector<int> p;
    int n;

    UnionFind(int _n)
        : n(_n)
        , p(_n) {
        iota(p.begin(), p.end(), 0);
    }

    void unite(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa != pb) p[pa] = pb;
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
};

class Solution {
public:
    int largestComponentSize(vector<int>& nums) {
        int m = *max_element(nums.begin(), nums.end());
        UnionFind* uf = new UnionFind(m + 1);
        for (int v : nums) {
            int i = 2;
            while (i <= v / i) {
                if (v % i == 0) {
                    uf->unite(v, i);
                    uf->unite(v, v / i);
                }
                ++i;
            }
        }
        vector<int> cnt(m + 1);
        int ans = 0;
        for (int v : nums) {
            int t = uf->find(v);
            ++cnt[t];
            ans = max(ans, cnt[t]);
        }
        return ans;
    }
};
```

#### Go

```go
func largestComponentSize(nums []int) int {
	m := slices.Max(nums)
	p := make([]int, m+1)
	for i := range p {
		p[i] = i
	}
	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	union := func(a, b int) {
		pa, pb := find(a), find(b)
		if pa != pb {
			p[pa] = pb
		}
	}
	for _, v := range nums {
		i := 2
		for i <= v/i {
			if v%i == 0 {
				union(v, i)
				union(v, v/i)
			}
			i++
		}
	}
	cnt := make([]int, m+1)
	for _, v := range nums {
		t := find(v)
		cnt[t]++
	}
	return slices.Max(cnt)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
