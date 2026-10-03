---
comments: true
difficulty: Hard
rating: 2621
source: Weekly Contest 294 Q4
tags:
    - Stack
    - Array
    - Prefix Sum
    - Monotonic Stack
---

<!-- problem:start -->

# [2281. Sum of Total Strength of Wizards](https://leetcode.com/problems/sum-of-total-strength-of-wizards)

[中文文档](/solution/2200-2299/2281.Sum%20of%20Total%20Strength%20of%20Wizards/README.md)

## Mô tả

<!-- description:start -->

<p>Với vai trò là người cai trị một vương quốc, bạn có một đội quân pháp sư dưới quyền chỉ huy.</p>

<p>Cho một mảng số nguyên <code>strength</code> được đánh chỉ số từ <strong>0</strong>, trong đó <code>strength[i]</code> biểu thị sức mạnh của pháp sư <code>i<sup>th</sup></code>. Với một nhóm pháp sư <strong>liên tiếp</strong> (tức là sức mạnh của các pháp sư tạo thành một <strong>mảng con</strong> của <code>strength</code>), <strong>sức mạnh tổng</strong> được định nghĩa là <strong>tích</strong> của hai giá trị sau:</p>

<ul>
	<li>Sức mạnh của pháp sư <strong>yếu nhất</strong> trong nhóm.</li>
	<li><strong>Tổng</strong> sức mạnh của tất cả pháp sư trong nhóm.</li>
</ul>

<p>Hãy trả về <em><strong>tổng</strong> sức mạnh tổng của <strong>tất cả các nhóm pháp sư liên tiếp</strong></em>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Mảng con</strong> là một dãy phần tử <strong>không rỗng</strong> liên tiếp trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> strength = [1,3,1,2]
<strong>Đầu ra:</strong> 44
<strong>Giải thích:</strong> Tất cả các nhóm pháp sư liên tiếp là:
- [1] từ [<u><strong>1</strong></u>,3,1,2] có sức mạnh tổng là min([1]) * sum([1]) = 1 * 1 = 1
- [3] từ [1,<u><strong>3</strong></u>,1,2] có sức mạnh tổng là min([3]) * sum([3]) = 3 * 3 = 9
- [1] từ [1,3,<u><strong>1</strong></u>,2] có sức mạnh tổng là min([1]) * sum([1]) = 1 * 1 = 1
- [2] từ [1,3,1,<u><strong>2</strong></u>] có sức mạnh tổng là min([2]) * sum([2]) = 2 * 2 = 4
- [1,3] từ [<u><strong>1,3</strong></u>,1,2] có sức mạnh tổng là min([1,3]) * sum([1,3]) = 1 * 4 = 4
- [3,1] từ [1,<u><strong>3,1</strong></u>,2] có sức mạnh tổng là min([3,1]) * sum([3,1]) = 1 * 4 = 4
- [1,2] từ [1,3,<u><strong>1,2</strong></u>] có sức mạnh tổng là min([1,2]) * sum([1,2]) = 1 * 3 = 3
- [1,3,1] từ [<u><strong>1,3,1</strong></u>,2] có sức mạnh tổng là min([1,3,1]) * sum([1,3,1]) = 1 * 5 = 5
- [3,1,2] từ [1,<u><strong>3,1,2</strong></u>] có sức mạnh tổng là min([3,1,2]) * sum([3,1,2]) = 1 * 6 = 6
- [1,3,1,2] từ [<u><strong>1,3,1,2</strong></u>] có sức mạnh tổng là min([1,3,1,2]) * sum([1,3,1,2]) = 1 * 7 = 7
Tổng sức mạnh tổng là 1 + 9 + 1 + 4 + 4 + 4 + 3 + 5 + 6 + 7 = 44.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> strength = [5,4,6]
<strong>Đầu ra:</strong> 213
<strong>Giải thích:</strong> Tất cả các nhóm pháp sư liên tiếp là:
- [5] từ [<u><strong>5</strong></u>,4,6] có sức mạnh tổng là min([5]) * sum([5]) = 5 * 5 = 25
- [4] từ [5,<u><strong>4</strong></u>,6] có sức mạnh tổng là min([4]) * sum([4]) = 4 * 4 = 16
- [6] từ [5,4,<u><strong>6</strong></u>] có sức mạnh tổng là min([6]) * sum([6]) = 6 * 6 = 36
- [5,4] từ [<u><strong>5,4</strong></u>,6] có sức mạnh tổng là min([5,4]) * sum([5,4]) = 4 * 9 = 36
- [4,6] từ [5,<u><strong>4,6</strong></u>] có sức mạnh tổng là min([4,6]) * sum([4,6]) = 4 * 10 = 40
- [5,4,6] từ [<u><strong>5,4,6</strong></u>] có sức mạnh tổng là min([5,4,6]) * sum([5,4,6]) = 4 * 15 = 60
Tổng sức mạnh tổng là 25 + 16 + 36 + 36 + 40 + 60 = 213.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= strength.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= strength[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sức mạnh của một mảng con là giá trị nhỏ nhất nhân với tổng các phần tử; ta cần tính tổng này. Vì $n \le 10^5$, không thể liệt kê tất cả các mảng con. Ta gán phần đóng góp của mỗi chỉ số $i$ khi nó là giá trị nhỏ nhất ngoài cùng bên trái trên đoạn mà nó chi phối: $[left[i]+1, right[i]-1]$.
>
> Monotonic stack cho ta các cận này. Tổng của tổng các mảng con trên hình chữ nhật đó là một tổ hợp của các tổng tiền tố của tiền tố $ss$, có thể tính trong thời gian hằng số, sau đó nhân với $strength[i]$ theo modulo $10^9+7$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def totalStrength(self, strength: List[int]) -> int:
        n = len(strength)
        left = [-1] * n
        right = [n] * n
        stk = []
        for i, v in enumerate(strength):
            while stk and strength[stk[-1]] >= v:
                stk.pop()
            if stk:
                left[i] = stk[-1]
            stk.append(i)
        stk = []
        for i in range(n - 1, -1, -1):
            while stk and strength[stk[-1]] > strength[i]:
                stk.pop()
            if stk:
                right[i] = stk[-1]
            stk.append(i)

        ss = list(accumulate(list(accumulate(strength, initial=0)), initial=0))
        mod = int(1e9) + 7
        ans = 0
        for i, v in enumerate(strength):
            l, r = left[i] + 1, right[i] - 1
            a = (ss[r + 2] - ss[i + 1]) * (i - l + 1)
            b = (ss[i + 1] - ss[l]) * (r - i + 1)
            ans = (ans + (a - b) * v) % mod
        return ans
```

#### Java

```java
class Solution {
    public int totalStrength(int[] strength) {
        int n = strength.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            while (!stk.isEmpty() && strength[stk.peek()] >= strength[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && strength[stk.peek()] > strength[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        int mod = (int) 1e9 + 7;
        int[] s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = (s[i] + strength[i]) % mod;
        }
        int[] ss = new int[n + 2];
        for (int i = 0; i < n + 1; ++i) {
            ss[i + 1] = (ss[i] + s[i]) % mod;
        }
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            int v = strength[i];
            int l = left[i] + 1, r = right[i] - 1;
            long a = (long) (i - l + 1) * (ss[r + 2] - ss[i + 1]);
            long b = (long) (r - i + 1) * (ss[i + 1] - ss[l]);
            ans = (ans + v * ((a - b) % mod)) % mod;
        }
        return (int) (ans + mod) % mod;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int totalStrength(vector<int>& strength) {
        int n = strength.size();
        vector<int> left(n, -1);
        vector<int> right(n, n);
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && strength[stk.top()] >= strength[i]) stk.pop();
            if (!stk.empty()) left[i] = stk.top();
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.empty() && strength[stk.top()] > strength[i]) stk.pop();
            if (!stk.empty()) right[i] = stk.top();
            stk.push(i);
        }
        int mod = 1e9 + 7;
        vector<int> s(n + 1);
        for (int i = 0; i < n; ++i) s[i + 1] = (s[i] + strength[i]) % mod;
        vector<int> ss(n + 2);
        for (int i = 0; i < n + 1; ++i) ss[i + 1] = (ss[i] + s[i]) % mod;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int v = strength[i];
            int l = left[i] + 1, r = right[i] - 1;
            long a = (long) (i - l + 1) * (ss[r + 2] - ss[i + 1]);
            long b = (long) (r - i + 1) * (ss[i + 1] - ss[l]);
            ans = (ans + v * ((a - b) % mod)) % mod;
        }
        return (int) (ans + mod) % mod;
    }
};
```

#### Go

```go
func totalStrength(strength []int) int {
	n := len(strength)
	left := make([]int, n)
	right := make([]int, n)
	for i := range left {
		left[i] = -1
		right[i] = n
	}
	stk := []int{}
	for i, v := range strength {
		for len(stk) > 0 && strength[stk[len(stk)-1]] >= v {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			left[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	stk = []int{}
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && strength[stk[len(stk)-1]] > strength[i] {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			right[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	mod := int(1e9) + 7
	s := make([]int, n+1)
	for i, v := range strength {
		s[i+1] = (s[i] + v) % mod
	}
	ss := make([]int, n+2)
	for i, v := range s {
		ss[i+1] = (ss[i] + v) % mod
	}
	ans := 0
	for i, v := range strength {
		l, r := left[i]+1, right[i]-1
		a := (ss[r+2] - ss[i+1]) * (i - l + 1)
		b := (ss[i+1] - ss[l]) * (r - i + 1)
		ans = (ans + v*((a-b)%mod)) % mod
	}
	return (ans + mod) % mod
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
