---
comments: true
difficulty: Medium
rating: 1479
source: Weekly Contest 323 Q2
tags:
    - Array
    - Hash Table
    - Binary Search
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [2501. Longest Square Streak in an Array](https://leetcode.com/problems/longest-square-streak-in-an-array)

[中文文档](/solution/2500-2599/2501.Longest%20Square%20Streak%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>. Một dãy con của <code>nums</code> được gọi là <strong>square streak</strong> nếu:</p>

<ul>
	<li>Độ dài của dãy con ít nhất là <code>2</code>, và</li>
	<li><strong>sau khi</strong> sắp xếp dãy con, mỗi phần tử (ngoại trừ phần tử đầu tiên) là <strong>bình phương</strong> của số đứng trước.</li>
</ul>

<p><em>Trả về độ dài của <strong>square streak dài nhất</strong> trong </em><code>nums</code><em>, hoặc trả về </em><code>-1</code><em> nếu không có <strong>square streak</strong>.</em></p>

<p><strong>Dãy con</strong> là một mảng có thể được tạo ra từ một mảng khác bằng cách xóa một số phần tử hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,6,16,8,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Chọn dãy con [4,16,2]. Sau khi sắp xếp, dãy này trở thành [2,4,16].
- 4 = 2 * 2.
- 16 = 4 * 4.
Do đó, [4,16,2] là một square streak.
Có thể chứng minh rằng mọi dãy con có độ dài 4 đều không phải là square streak.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,5,6,7]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có square streak nào trong nums, vì vậy trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một square streak yêu cầu mỗi phần tử tiếp theo là bình phương của phần tử trước đó. Việc liệt kê mọi dãy con là bất khả thi khi $n\le 10^5$; ngay cả việc duyệt các chuỗi theo thứ tự rồi tìm phần tử kế tiếp bằng tìm kiếm tuyến tính cũng lãng phí vì cấu trúc phần tử kế tiếp là duy nhất.
>
> Giá trị tiếp theo được xác định bởi $x\mapsto x^2$, nên chỉ cần kiểm tra xem bình phương có tồn tại hay không. Sau khi đưa các phần tử của $\textit{nums}$ vào một set, ta liên tục bình phương từng phần tử bắt đầu để tính độ dài chuỗi tương ứng. Các bình phương tăng rất nhanh nên một chuỗi chỉ có độ dài $O(\log\log M)$. Những chuỗi có độ dài nhiều nhất là $1$ được trả về là $-1$.

<!-- thinking:end -->

Trước tiên, chúng ta dùng một hash table để ghi lại tất cả phần tử trong mảng. Sau đó, ta coi mỗi phần tử trong mảng là phần tử đầu tiên của dãy con, liên tục bình phương phần tử này và kiểm tra xem kết quả sau khi bình phương có nằm trong hash table hay không. Nếu có, ta dùng kết quả sau khi bình phương làm phần tử tiếp theo và tiếp tục kiểm tra cho đến khi kết quả sau khi bình phương không còn nằm trong hash table. Lúc này, ta kiểm tra xem độ dài dãy con có lớn hơn $1$ hay không. Nếu có, ta cập nhật đáp án.

Độ phức tạp thời gian là $O(n \times \log \log M)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$, còn $M$ là giá trị lớn nhất của các phần tử trong mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSquareStreak(self, nums: List[int]) -> int:
        s = set(nums)
        ans = -1
        for x in nums:
            t = 0
            while x in s:
                x *= x
                t += 1
            if t > 1:
                ans = max(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public int longestSquareStreak(int[] nums) {
        Set<Long> s = new HashSet<>();
        for (long x : nums) {
            s.add(x);
        }
        int ans = -1;
        for (long x : s) {
            int t = 0;
            for (; s.contains(x); x *= x) {
                ++t;
            }
            if (t > 1) {
                ans = Math.max(ans, t);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSquareStreak(vector<int>& nums) {
        unordered_set<long long> s(nums.begin(), nums.end());
        int ans = -1;
        for (long long x : nums) {
            int t = 0;
            for (; s.contains(x); x *= x) {
                ++t;
            }
            if (t > 1) {
                ans = max(ans, t);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestSquareStreak(nums []int) int {
	s := map[int]bool{}
	for _, x := range nums {
		s[x] = true
	}
	ans := -1
	for x := range s {
		t := 0
		for s[x] {
			x *= x
			t++
		}
		if t > 1 {
			ans = max(ans, t)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function longestSquareStreak(nums: number[]): number {
    const s = new Set(nums);
    let ans = -1;

    for (const num of nums) {
        let x = num;
        let t = 0;

        while (s.has(x)) {
            x *= x;
            t += 1;
        }

        if (t > 1) {
            ans = Math.max(ans, t);
        }
    }

    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var longestSquareStreak = function (nums) {
    const s = new Set(nums);
    let ans = -1;

    for (const num of nums) {
        let x = num;
        let t = 0;

        while (s.has(x)) {
            x *= x;
            t += 1;
        }

        if (t > 1) {
            ans = Math.max(ans, t);
        }
    }

    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tìm kiếm với Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 mở rộng độc lập từ mỗi phần tử bắt đầu, nên các hậu tố trùng nhau như $2,4,16$ và $4,16$ bị tính lại.
>
> Gọi $\textit{dfs}(x)$ là độ dài streak bắt đầu từ $x$, với công thức truy hồi $1+\textit{dfs}(x^2)$ và bằng $0$ khi $x$ không tồn tại. Memoization giúp tính mỗi giá trị đúng một lần và biến lượt duyệt thành tuyến tính. Đáp án là giá trị lớn nhất trên mọi phần tử bắt đầu, hoặc $-1$ nếu giá trị đó nhỏ hơn $2$.

<!-- thinking:end -->

Tương tự Lời giải 1, trước tiên chúng ta dùng một hash table để ghi lại tất cả phần tử trong mảng. Sau đó, ta thiết kế một hàm $\textit{dfs}(x)$ biểu diễn độ dài square streak bắt đầu từ $x$. Đáp án là $\max(\textit{dfs}(x))$, trong đó $x$ là một phần tử trong mảng $\textit{nums}$.

Quá trình tính hàm $\textit{dfs}(x)$ như sau:

- Nếu $x$ không có trong hash table, trả về $0$.
- Ngược lại, trả về $1 + \textit{dfs}(x^2)$.

Trong quá trình này, chúng ta có thể sử dụng memoization, tức là dùng một hash table để ghi lại giá trị của hàm $\textit{dfs}(x)$ nhằm tránh các phép tính dư thừa.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSquareStreak(self, nums: List[int]) -> int:
        @cache
        def dfs(x: int) -> int:
            if x not in s:
                return 0
            return 1 + dfs(x * x)

        s = set(nums)
        ans = max(dfs(x) for x in s)
        return -1 if ans < 2 else ans
```

#### Java

```java
class Solution {
    private Map<Long, Integer> f = new HashMap<>();
    private Set<Long> s = new HashSet<>();

    public int longestSquareStreak(int[] nums) {
        for (long x : nums) {
            s.add(x);
        }
        int ans = 0;
        for (long x : s) {
            ans = Math.max(ans, dfs(x));
        }
        return ans < 2 ? -1 : ans;
    }

    private int dfs(long x) {
        if (!s.contains(x)) {
            return 0;
        }
        if (f.containsKey(x)) {
            return f.get(x);
        }
        int ans = 1 + dfs(x * x);
        f.put(x, ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSquareStreak(vector<int>& nums) {
        unordered_set<long long> s(nums.begin(), nums.end());
        int ans = 0;
        unordered_map<long long, int> f;
        auto dfs = [&](this auto&& dfs, long long x) -> int {
            if (!s.contains(x)) {
                return 0;
            }
            if (f.contains(x)) {
                return f[x];
            }
            f[x] = 1 + dfs(x * x);
            return f[x];
        };
        for (long long x : s) {
            ans = max(ans, dfs(x));
        }
        return ans < 2 ? -1 : ans;
    }
};
```

#### Go

```go
func longestSquareStreak(nums []int) (ans int) {
	s := map[int]bool{}
	for _, x := range nums {
		s[x] = true
	}
	f := map[int]int{}
	var dfs func(int) int
	dfs = func(x int) int {
		if !s[x] {
			return 0
		}
		if v, ok := f[x]; ok {
			return v
		}
		f[x] = 1 + dfs(x*x)
		return f[x]
	}
	for x := range s {
		if t := dfs(x); ans < t {
			ans = t
		}
	}
	if ans < 2 {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function longestSquareStreak(nums: number[]): number {
    const s = new Set(nums);
    const f = new Map<number, number>();
    const dfs = (x: number): number => {
        if (f.has(x)) {
            return f.get(x)!;
        }
        if (!s.has(x)) {
            return 0;
        }
        f.set(x, 1 + dfs(x ** 2));
        return f.get(x)!;
    };

    for (const x of s) {
        dfs(x);
    }
    const ans = Math.max(...f.values());
    return ans > 1 ? ans : -1;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var longestSquareStreak = function (nums) {
    const s = new Set(nums);
    const f = new Map();
    const dfs = x => {
        if (f.has(x)) {
            return f.get(x);
        }
        if (!s.has(x)) {
            return 0;
        }
        f.set(x, 1 + dfs(x ** 2));
        return f.get(x);
    };

    for (const x of s) {
        dfs(x);
    }
    const ans = Math.max(...f.values());
    return ans > 1 ? ans : -1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
