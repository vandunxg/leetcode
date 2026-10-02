---
comments: true
difficulty: Medium
rating: 1928
source: Weekly Contest 203 Q3
tags:
    - Array
    - Hash Table
    - Binary Search
    - Simulation
---

<!-- problem:start -->

# [1562. Find Latest Group of Size M](https://leetcode.com/problems/find-latest-group-of-size-m)

[中文文档](/solution/1500-1599/1562.Find%20Latest%20Group%20of%20Size%20M/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>arr</code> biểu diễn một hoán vị các số từ <code>1</code> đến <code>n</code>.</p>

<p>Ban đầu có một chuỗi nhị phân độ dài <code>n</code> với mọi bit bằng 0. Ở mỗi bước <code>i</code> (giả sử chuỗi nhị phân và <code>arr</code> được đánh chỉ số từ 1) từ <code>1</code> đến <code>n</code>, bit tại vị trí <code>arr[i]</code> được đặt thành <code>1</code>.</p>

<p>Cho thêm số nguyên <code>m</code>. Hãy tìm bước cuối cùng mà tồn tại một nhóm số 1 có độ dài <code>m</code>. Nhóm số 1 là một chuỗi con liên tiếp gồm các số <code>1</code>, không thể mở rộng theo bất kỳ hướng nào.</p>

<p>Trả về <em>bước cuối cùng mà tồn tại một nhóm số 1 có độ dài <strong>chính xác</strong></em> <code>m</code>. <em>Nếu không tồn tại nhóm như vậy, trả về</em> <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [3,5,1,2,4], m = 1
<strong>Output:</strong> 4
<strong>Giải thích:</strong> 
Step 1: &quot;00<u>1</u>00&quot;, groups: [&quot;1&quot;]
Step 2: &quot;0010<u>1</u>&quot;, groups: [&quot;1&quot;, &quot;1&quot;]
Step 3: &quot;<u>1</u>0101&quot;, groups: [&quot;1&quot;, &quot;1&quot;, &quot;1&quot;]
Step 4: &quot;1<u>1</u>101&quot;, groups: [&quot;111&quot;, &quot;1&quot;]
Step 5: &quot;111<u>1</u>1&quot;, groups: [&quot;11111&quot;]
Bước cuối cùng tồn tại nhóm có kích thước 1 là bước 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [3,1,5,4,2], m = 2
<strong>Output:</strong> -1
<strong>Explanation:</strong> 
Step 1: &quot;00<u>1</u>00&quot;, groups: [&quot;1&quot;]
Step 2: &quot;<u>1</u>0100&quot;, groups: [&quot;1&quot;, &quot;1&quot;]
Step 3: &quot;1010<u>1</u>&quot;, groups: [&quot;1&quot;, &quot;1&quot;, &quot;1&quot;]
Step 4: &quot;101<u>1</u>1&quot;, groups: [&quot;1&quot;, &quot;111&quot;]
Step 5: &quot;1<u>1</u>111&quot;, groups: [&quot;11111&quot;]
Không có nhóm kích thước 2 nào tồn tại ở bất kỳ bước nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == arr.length</code></li>
	<li><code>1 &lt;= m &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= n</code></li>
	<li>All integers in <code>arr</code> are <strong>distinct</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các số 0 lần lượt đổi thành 1 theo thứ tự trong $arr$; ta cần thời điểm cuối cùng một đoạn liên tiếp độ dài $m$ tồn tại. Vì $n\le 10^5$, không thể dựng lại chuỗi sau mỗi bước. Nếu $m=n$, toàn bộ mảng được lấp đầy ở bước $n$.
>
> Một rừng disjoint-set lưu kích thước của từng thành phần số 1. Trước khi vị trí mới hợp nhất với hàng xóm đã được lấp đầy, nếu thành phần của hàng xóm có kích thước đúng bằng $m$, nhóm đó vẫn tồn tại ở bước hiện tại và ta ghi nhận bước này. Sau đó union và cập nhật kích thước.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLatestStep(self, arr: List[int], m: int) -> int:
        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        def union(a, b):
            pa, pb = find(a), find(b)
            if pa == pb:
                return
            p[pa] = pb
            size[pb] += size[pa]

        n = len(arr)
        if m == n:
            return n
        vis = [False] * n
        p = list(range(n))
        size = [1] * n
        ans = -1
        for i, v in enumerate(arr):
            v -= 1
            if v and vis[v - 1]:
                if size[find(v - 1)] == m:
                    ans = i
                union(v, v - 1)
            if v < n - 1 and vis[v + 1]:
                if size[find(v + 1)] == m:
                    ans = i
                union(v, v + 1)
            vis[v] = True
        return ans
```

#### Java

```java
class Solution {
    private int[] p;
    private int[] size;

    public int findLatestStep(int[] arr, int m) {
        int n = arr.length;
        if (m == n) {
            return n;
        }
        boolean[] vis = new boolean[n];
        p = new int[n];
        size = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
            size[i] = 1;
        }
        int ans = -1;
        for (int i = 0; i < n; ++i) {
            int v = arr[i] - 1;
            if (v > 0 && vis[v - 1]) {
                if (size[find(v - 1)] == m) {
                    ans = i;
                }
                union(v, v - 1);
            }
            if (v < n - 1 && vis[v + 1]) {
                if (size[find(v + 1)] == m) {
                    ans = i;
                }
                union(v, v + 1);
            }
            vis[v] = true;
        }
        return ans;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    private void union(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return;
        }
        p[pa] = pb;
        size[pb] += size[pa];
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> p;
    vector<int> size;

    int findLatestStep(vector<int>& arr, int m) {
        int n = arr.size();
        if (m == n) return n;
        p.resize(n);
        size.assign(n, 1);
        for (int i = 0; i < n; ++i) p[i] = i;
        int ans = -1;
        vector<int> vis(n);
        for (int i = 0; i < n; ++i) {
            int v = arr[i] - 1;
            if (v && vis[v - 1]) {
                if (size[find(v - 1)] == m) ans = i;
                unite(v, v - 1);
            }
            if (v < n - 1 && vis[v + 1]) {
                if (size[find(v + 1)] == m) ans = i;
                unite(v, v + 1);
            }
            vis[v] = true;
        }
        return ans;
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    void unite(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) return;
        p[pa] = pb;
        size[pb] += size[pa];
    }
};
```

#### Go

```go
func findLatestStep(arr []int, m int) int {
	n := len(arr)
	if m == n {
		return n
	}
	p := make([]int, n)
	size := make([]int, n)
	vis := make([]bool, n)
	for i := range p {
		p[i] = i
		size[i] = 1
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
		if pa == pb {
			return
		}
		p[pa] = pb
		size[pb] += size[pa]
	}

	ans := -1
	for i, v := range arr {
		v--
		if v > 0 && vis[v-1] {
			if size[find(v-1)] == m {
				ans = i
			}
			union(v, v-1)
		}
		if v < n-1 && vis[v+1] {
			if size[find(v+1)] == m {
				ans = i
			}
			union(v, v+1)
		}
		vis[v] = true
	}
	return ans
}
```

#### JavaScript

```js
const findLatestStep = function (arr, m) {
    function find(x) {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    function union(a, b) {
        const pa = find(a);
        const pb = find(b);
        if (pa === pb) {
            return;
        }
        p[pa] = pb;
        size[pb] += size[pa];
    }

    const n = arr.length;
    if (m === n) {
        return n;
    }
    const vis = Array(n).fill(false);
    const p = Array.from({ length: n }, (_, i) => i);
    const size = Array(n).fill(1);
    let ans = -1;
    for (let i = 0; i < n; ++i) {
        const v = arr[i] - 1;
        if (v > 0 && vis[v - 1]) {
            if (size[find(v - 1)] === m) {
                ans = i;
            }
            union(v, v - 1);
        }
        if (v < n - 1 && vis[v + 1]) {
            if (size[find(v + 1)] === m) {
                ans = i;
            }
            union(v, v + 1);
        }
        vis[v] = true;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2

<!-- thinking:start -->

> **Thinking**
>
> The union-find lookups add a log factor, yet merges only touch interval endpoints. Store each ones-run's length at its two ends; a new point reads the neighboring end lengths, writes $l+r+1$ to the new ends, and checks whether $l$ or $r$ equals $m$. Updates are $O(1)$, so the total time is linear.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLatestStep(self, arr: List[int], m: int) -> int:
        n = len(arr)
        if m == n:
            return n
        cnt = [0] * (n + 2)
        ans = -1
        for i, v in enumerate(arr):
            v -= 1
            l, r = cnt[v - 1], cnt[v + 1]
            if l == m or r == m:
                ans = i
            cnt[v - l] = cnt[v + r] = l + r + 1
        return ans
```

#### Java

```java
class Solution {
    public int findLatestStep(int[] arr, int m) {
        int n = arr.length;
        if (m == n) {
            return n;
        }
        int[] cnt = new int[n + 2];
        int ans = -1;
        for (int i = 0; i < n; ++i) {
            int v = arr[i];
            int l = cnt[v - 1], r = cnt[v + 1];
            if (l == m || r == m) {
                ans = i;
            }
            cnt[v - l] = l + r + 1;
            cnt[v + r] = l + r + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findLatestStep(vector<int>& arr, int m) {
        int n = arr.size();
        if (m == n) return n;
        vector<int> cnt(n + 2);
        int ans = -1;
        for (int i = 0; i < n; ++i) {
            int v = arr[i];
            int l = cnt[v - 1], r = cnt[v + 1];
            if (l == m || r == m) ans = i;
            cnt[v - l] = cnt[v + r] = l + r + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func findLatestStep(arr []int, m int) int {
	n := len(arr)
	if m == n {
		return n
	}
	cnt := make([]int, n+2)
	ans := -1
	for i, v := range arr {
		l, r := cnt[v-1], cnt[v+1]
		if l == m || r == m {
			ans = i
		}
		cnt[v-l], cnt[v+r] = l+r+1, l+r+1
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
