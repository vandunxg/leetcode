---
comments: true
difficulty: Medium
rating: 1913
source: Weekly Contest 365 Q3
tags:
    - Array
    - Hash Table
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [2875. Minimum Size Subarray in Infinite Array](https://leetcode.com/problems/minimum-size-subarray-in-infinite-array)

[中文文档](/solution/2800-2899/2875.Minimum%20Size%20Subarray%20in%20Infinite%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> đánh chỉ số từ <strong>0</strong> và một số nguyên <code>target</code>.</p>

<p>Một mảng <code>infinite_nums</code> đánh chỉ số từ <strong>0</strong> được tạo ra bằng cách nối vô hạn các phần tử của <code>nums</code> vào chính nó.</p>

<p>Hãy trả về <em>độ dài của <strong>mảng con ngắn nhất</strong> trong mảng </em><code>infinite_nums</code><em> có tổng bằng </em><code>target</code><em>.</em> Nếu không tồn tại mảng con như vậy, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], target = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, infinite_nums = [1,2,3,1,2,3,1,2,...].
Mảng con trong phạm vi [1,2] có tổng bằng target = 5 và có độ dài = 2.
Có thể chứng minh rằng 2 là độ dài nhỏ nhất của một mảng con có tổng bằng target = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,2,3], target = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, infinite_nums = [1,1,1,2,3,1,1,1,2,3,1,1,...].
Mảng con trong phạm vi [4,5] có tổng bằng target = 4 và có độ dài = 2.
Có thể chứng minh rằng 2 là độ dài nhỏ nhất của một mảng con có tổng bằng target = 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,6,8], target = 3
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Trong ví dụ này, infinite_nums = [2,4,6,8,2,4,6,8,...].
Có thể chứng minh rằng không tồn tại mảng con nào có tổng bằng target = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= target &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Vì mảng lặp vô hạn, $target$ gồm một số vòng đầy đủ và một phần dư ngắn nhất (hoặc một đoạn quấn vòng ngắn nhất bù cho một vòng đầy đủ). Trước hết, loại bỏ nhiều tổng của các vòng đầy đủ nhất có thể; sau đó, trong một lượt duyệt prefix sum, dùng hash map để tìm đoạn ngắn nhất có tổng bằng phần dư hoặc bằng $s$ trừ phần dư.

<!-- thinking:end -->

Trước hết, ta tính tổng tất cả phần tử trong mảng $nums$, ký hiệu là $s$.

Nếu $target \gt s$, ta có thể đưa $target$ về khoảng $[0, s)$ bằng cách trừ đi $\lfloor \frac{target}{s} \rfloor \times s$ khỏi nó. Khi đó, độ dài mảng con là $a = \lfloor \frac{target}{s} \rfloor \times n$, trong đó $n$ là độ dài của mảng $nums$.

Tiếp theo, ta cần tìm mảng con ngắn nhất trong $nums$ có tổng bằng $target$, hoặc mảng con ngắn nhất có tổng prefix sum cộng suffix sum bằng $s - target$. Ta có thể dùng prefix sum và hash table để tìm các mảng con như vậy.

Nếu tìm được một mảng con như vậy, đáp án cuối cùng là $a + b$. Nếu không, đáp án là $-1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó n là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSizeSubarray(self, nums: List[int], target: int) -> int:
        s = sum(nums)
        n = len(nums)
        a = 0
        if target > s:
            a = n * (target // s)
            target -= target // s * s
        if target == s:
            return n
        pos = {0: -1}
        pre = 0
        b = inf
        for i, x in enumerate(nums):
            pre += x
            if (t := pre - target) in pos:
                b = min(b, i - pos[t])
            if (t := pre - (s - target)) in pos:
                b = min(b, n - (i - pos[t]))
            pos[pre] = i
        return -1 if b == inf else a + b
```

#### Java

```java
class Solution {
    public int minSizeSubarray(int[] nums, int target) {
        long s = Arrays.stream(nums).sum();
        int n = nums.length;
        int a = 0;
        if (target > s) {
            a = n * (target / (int) s);
            target -= target / s * s;
        }
        if (target == s) {
            return n;
        }
        Map<Long, Integer> pos = new HashMap<>();
        pos.put(0L, -1);
        long pre = 0;
        int b = 1 << 30;
        for (int i = 0; i < n; ++i) {
            pre += nums[i];
            if (pos.containsKey(pre - target)) {
                b = Math.min(b, i - pos.get(pre - target));
            }
            if (pos.containsKey(pre - (s - target))) {
                b = Math.min(b, n - (i - pos.get(pre - (s - target))));
            }
            pos.put(pre, i);
        }
        return b == 1 << 30 ? -1 : a + b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSizeSubarray(vector<int>& nums, int target) {
        long long s = accumulate(nums.begin(), nums.end(), 0LL);
        int n = nums.size();
        int a = 0;
        if (target > s) {
            a = n * (target / s);
            target -= target / s * s;
        }
        if (target == s) {
            return n;
        }
        unordered_map<int, int> pos{{0, -1}};
        long long pre = 0;
        int b = 1 << 30;
        for (int i = 0; i < n; ++i) {
            pre += nums[i];
            if (pos.count(pre - target)) {
                b = min(b, i - pos[pre - target]);
            }
            if (pos.count(pre - (s - target))) {
                b = min(b, n - (i - pos[pre - (s - target)]));
            }
            pos[pre] = i;
        }
        return b == 1 << 30 ? -1 : a + b;
    }
};
```

#### Go

```go
func minSizeSubarray(nums []int, target int) int {
	s := 0
	for _, x := range nums {
		s += x
	}
	n := len(nums)
	a := 0
	if target > s {
		a = n * (target / s)
		target -= target / s * s
	}
	if target == s {
		return n
	}
	pos := map[int]int{0: -1}
	pre := 0
	b := 1 << 30
	for i, x := range nums {
		pre += x
		if j, ok := pos[pre-target]; ok {
			b = min(b, i-j)
		}
		if j, ok := pos[pre-(s-target)]; ok {
			b = min(b, n-(i-j))
		}
		pos[pre] = i
	}
	if b == 1<<30 {
		return -1
	}
	return a + b
}
```

#### TypeScript

```ts
function minSizeSubarray(nums: number[], target: number): number {
    const s = nums.reduce((a, b) => a + b);
    const n = nums.length;
    let a = 0;
    if (target > s) {
        a = n * ((target / s) | 0);
        target -= ((target / s) | 0) * s;
    }
    if (target === s) {
        return n;
    }
    const pos: Map<number, number> = new Map();
    let pre = 0;
    pos.set(0, -1);
    let b = Infinity;
    for (let i = 0; i < n; ++i) {
        pre += nums[i];
        if (pos.has(pre - target)) {
            b = Math.min(b, i - pos.get(pre - target)!);
        }
        if (pos.has(pre - (s - target))) {
            b = Math.min(b, n - (i - pos.get(pre - (s - target))!));
        }
        pos.set(pre, i);
    }
    return b === Infinity ? -1 : a + b;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
