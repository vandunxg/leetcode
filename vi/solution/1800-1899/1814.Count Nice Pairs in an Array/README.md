---
comments: true
difficulty: Medium
rating: 1737
source: Biweekly Contest 49 Q3
tags:
    - Array
    - Hash Table
    - Math
    - Counting
---

<!-- problem:start -->

# [1814. Count Nice Pairs in an Array](https://leetcode.com/problems/count-nice-pairs-in-an-array)

[中文文档](/solution/1800-1899/1814.Count%20Nice%20Pairs%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> gồm các số nguyên không âm. Ta định nghĩa <code>rev(x)</code> là số nguyên không âm <code>x</code> sau khi đảo ngược. Ví dụ, <code>rev(123) = 321</code> và <code>rev(120) = 21</code>. Một cặp chỉ số <code>(i, j)</code> là <strong>đẹp</strong> nếu thỏa mãn tất cả các điều kiện sau:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; nums.length</code></li>
	<li><code>nums[i] + rev(nums[j]) == nums[j] + rev(nums[i])</code></li>
</ul>

<p>Trả về <em>số cặp chỉ số đẹp</em>. Vì số lượng này có thể rất lớn, hãy trả về kết quả <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [42,11,1,97]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hai cặp đó là:
 - (0,3) : 42 + rev(97) = 42 + 79 = 121, 97 + rev(42) = 97 + 24 = 121.
 - (1,2) : 11 + rev(1) = 11 + 1 = 12, 1 + rev(11) = 1 + 11 = 12.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [13,10,35,24,76]
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Biến đổi phương trình + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp đẹp thỏa mãn $nums[i]+rev(nums[j])=nums[j]+rev(nums[i])$. Kiểm tra mọi cặp tốn $O(n^2\log M)$. Với $n\le 10^5$, cách này không thể đáp ứng.
>
> Biến đổi phương trình được $nums[i]-rev(nums[i])=nums[j]-rev(nums[j])$. Đếm từng hiệu; đáp án là tổng $\sum C(v,2)$ theo các tần suất đó, lấy modulo $10^9+7$.

<!-- thinking:end -->

Với cặp chỉ số $(i, j)$, nếu cặp thỏa mãn điều kiện, ta có $nums[i] + rev(nums[j]) = nums[j] + rev(nums[i])$, suy ra $nums[i] - nums[j] = rev(nums[j]) - rev(nums[i])$.

Vì vậy, ta có thể dùng $nums[i] - rev(nums[i])$ làm khóa của hash table và đếm số lần xuất hiện của mỗi khóa. Cuối cùng, ta tính tổ hợp của các giá trị tương ứng với từng khóa, cộng chúng lại để nhận được đáp án.

Lưu ý rằng cần thực hiện phép modulo trên đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là độ dài mảng $nums$ và giá trị lớn nhất trong mảng $nums$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countNicePairs(self, nums: List[int]) -> int:
        def rev(x):
            y = 0
            while x:
                y = y * 10 + x % 10
                x //= 10
            return y

        cnt = Counter(x - rev(x) for x in nums)
        mod = 10**9 + 7
        return sum(v * (v - 1) // 2 for v in cnt.values()) % mod
```

#### Java

```java
class Solution {
    public int countNicePairs(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            int y = x - rev(x);
            cnt.merge(y, 1, Integer::sum);
        }
        final int mod = (int) 1e9 + 7;
        long ans = 0;
        for (int v : cnt.values()) {
            ans = (ans + (long) v * (v - 1) / 2) % mod;
        }
        return (int) ans;
    }

    private int rev(int x) {
        int y = 0;
        for (; x > 0; x /= 10) {
            y = y * 10 + x % 10;
        }
        return y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countNicePairs(vector<int>& nums) {
        auto rev = [](int x) {
            int y = 0;
            for (; x > 0; x /= 10) {
                y = y * 10 + x % 10;
            }
            return y;
        };
        unordered_map<int, int> cnt;
        for (int& x : nums) {
            int y = x - rev(x);
            cnt[y]++;
        }
        long long ans = 0;
        const int mod = 1e9 + 7;
        for (auto& [_, v] : cnt) {
            ans = (ans + 1ll * v * (v - 1) / 2) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func countNicePairs(nums []int) (ans int) {
	rev := func(x int) (y int) {
		for ; x > 0; x /= 10 {
			y = y*10 + x%10
		}
		return
	}
	cnt := map[int]int{}
	for _, x := range nums {
		y := x - rev(x)
		cnt[y]++
	}
	const mod int = 1e9 + 7
	for _, v := range cnt {
		ans = (ans + v*(v-1)/2) % mod
	}
	return
}
```

#### TypeScript

```ts
function countNicePairs(nums: number[]): number {
    const rev = (x: number): number => {
        let y = 0;
        while (x) {
            y = y * 10 + (x % 10);
            x = Math.floor(x / 10);
        }
        return y;
    };
    const mod = 10 ** 9 + 7;
    const cnt = new Map<number, number>();
    let ans = 0;
    for (const x of nums) {
        const y = x - rev(x);
        ans = (ans + (cnt.get(y) ?? 0)) % mod;
        cnt.set(y, (cnt.get(y) ?? 0) + 1);
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
var countNicePairs = function (nums) {
    const rev = x => {
        let y = 0;
        for (; x > 0; x = Math.floor(x / 10)) {
            y = y * 10 + (x % 10);
        }
        return y;
    };
    const cnt = new Map();
    for (const x of nums) {
        const y = x - rev(x);
        cnt.set(y, (cnt.get(y) | 0) + 1);
    }
    let ans = 0;
    const mod = 1e9 + 7;
    for (const [_, v] of cnt) {
        ans = (ans + Math.floor((v * (v - 1)) / 2)) % mod;
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int CountNicePairs(int[] nums) {
        Dictionary<int, int> cnt = new Dictionary<int, int>();
        foreach (int x in nums) {
            int y = x - Rev(x);
            cnt[y] = cnt.GetValueOrDefault(y, 0) + 1;
        }
        int mod = (int)1e9 + 7;
        long ans = 0;
        foreach (int v in cnt.Values) {
            ans = (ans + (long)v * (v - 1) / 2) % mod;
        }
        return (int)ans;
    }

    private int Rev(int x) {
        int y = 0;
        while (x > 0) {
            y = y * 10 + x % 10;
            x /= 10;
        }
        return y;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 xây dựng toàn bộ histogram rồi mới tính tổng các tổ hợp, cần thêm một lượt duyệt. Các cặp là không có thứ tự, nên trong lúc quét ta có thể cộng số lượng hiện tại của cùng một hiệu vào đáp án rồi tăng khóa đó, không cần tính $C(v,2)$ riêng sau cùng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countNicePairs(self, nums: List[int]) -> int:
        def rev(x):
            y = 0
            while x:
                y = y * 10 + x % 10
                x //= 10
            return y

        ans = 0
        mod = 10**9 + 7
        cnt = Counter()
        for x in nums:
            y = x - rev(x)
            ans += cnt[y]
            cnt[y] += 1
        return ans % mod
```

#### Java

```java
class Solution {
    public int countNicePairs(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        final int mod = (int) 1e9 + 7;
        int ans = 0;
        for (int x : nums) {
            int y = x - rev(x);
            ans = (ans + cnt.getOrDefault(y, 0)) % mod;
            cnt.merge(y, 1, Integer::sum);
        }
        return ans;
    }

    private int rev(int x) {
        int y = 0;
        for (; x > 0; x /= 10) {
            y = y * 10 + x % 10;
        }
        return y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countNicePairs(vector<int>& nums) {
        auto rev = [](int x) {
            int y = 0;
            for (; x > 0; x /= 10) {
                y = y * 10 + x % 10;
            }
            return y;
        };
        unordered_map<int, int> cnt;
        int ans = 0;
        const int mod = 1e9 + 7;
        for (int& x : nums) {
            int y = x - rev(x);
            ans = (ans + cnt[y]++) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func countNicePairs(nums []int) (ans int) {
	rev := func(x int) (y int) {
		for ; x > 0; x /= 10 {
			y = y*10 + x%10
		}
		return
	}
	cnt := map[int]int{}
	const mod int = 1e9 + 7
	for _, x := range nums {
		y := x - rev(x)
		ans = (ans + cnt[y]) % mod
		cnt[y]++
	}
	return
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var countNicePairs = function (nums) {
    const rev = x => {
        let y = 0;
        for (; x > 0; x = Math.floor(x / 10)) {
            y = y * 10 + (x % 10);
        }
        return y;
    };
    let ans = 0;
    const mod = 1e9 + 7;
    const cnt = new Map();
    for (const x of nums) {
        const y = x - rev(x);
        const v = cnt.get(y) | 0;
        ans = (ans + v) % mod;
        cnt.set(y, v + 1);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
