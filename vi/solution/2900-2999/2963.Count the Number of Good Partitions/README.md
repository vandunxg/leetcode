---
comments: true
difficulty: Hard
rating: 1984
source: Weekly Contest 375 Q4
tags:
    - Array
    - Hash Table
    - Math
    - Combinatorics
---

<!-- problem:start -->

# [2963. Count the Number of Good Partitions](https://leetcode.com/problems/count-the-number-of-good-partitions)

[中文文档](/solution/2900-2999/2963.Count%20the%20Number%20of%20Good%20Partitions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>0-indexed</strong> <code>nums</code> gồm các số nguyên <strong>dương</strong>.</p>

<p>Một phép chia mảng thành một hoặc nhiều mảng con <strong>liên tiếp</strong> được gọi là <strong>tốt</strong> nếu không có hai mảng con nào chứa cùng một số.</p>

<p>Trả về <em><strong>tổng số</strong> phép chia tốt của </em><code>nums</code>.</p>

<p>Vì đáp án có thể lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> 8 phép chia tốt có thể có là: ([1], [2], [3], [4]), ([1], [2], [3,4]), ([1], [2,3], [4]), ([1], [2,3,4]), ([1,2], [3], [4]), ([1,2], [3,4]), ([1,2,3], [4]) và ([1,2,3,4]).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Phép chia tốt duy nhất có thể có là: ([1,1,1,1]).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 2 phép chia tốt có thể có là: ([1,2,1], [3]) và ([1,2,1,3]).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Phân nhóm + Lũy thừa nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Một phép chia tốt phải giữ các phần tử có cùng giá trị trong cùng một phần, vì vậy đoạn từ lần xuất hiện đầu tiên đến lần xuất hiện cuối cùng của mỗi giá trị phải nằm trọn trong một phần. Bản đồ lần xuất hiện cuối cùng $last$ chia mảng thành các khối không thể tách; giữa mỗi cặp khối có thể đặt hoặc không đặt dấu chia.
>
> Khi duyệt mảng, $j$ theo dõi điểm cuối bên phải của khối hiện tại; $i=j$ thì tăng số khối $k$. Với $k$ khối, có $k-1$ vị trí tùy chọn để đặt dấu chia, tức là có $2^{k-1}$ cách chia theo modulo số nguyên tố.

<!-- thinking:end -->

Theo mô tả bài toán, cùng một số phải nằm trong cùng một mảng con. Vì vậy, ta dùng hash table $last$ để ghi lại chỉ số của lần xuất hiện cuối cùng của mỗi số.

Tiếp theo, ta dùng chỉ số $j$ để đánh dấu chỉ số lớn nhất của phần tử đã xuất hiện trong các phần tử đang xét, đồng thời dùng biến $k$ để ghi lại số mảng con hiện có thể tạo ra.

Sau đó, ta duyệt mảng $nums$ từ trái sang phải. Với số hiện tại $nums[i]$, ta lấy chỉ số lần xuất hiện cuối cùng của nó và cập nhật $j = \max(j, last[nums[i]])$. Nếu $i = j$, điều đó có nghĩa là vị trí hiện tại có thể là điểm kết thúc của một mảng con, nên ta tăng $k$ lên $1$. Tiếp tục duyệt cho đến hết mảng.

Cuối cùng, ta xét số cách chia thành $k$ mảng con. Số lượng mảng con là $k$, và có $k-1$ vị trí giữa các mảng con có thể đặt hoặc không đặt dấu chia, vì vậy số cách là $2^{k-1}$. Vì đáp án có thể rất lớn, ta cần lấy modulo $10^9 + 7$. Ở đây, ta có thể dùng lũy thừa nhanh để tăng tốc phép tính.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfGoodPartitions(self, nums: List[int]) -> int:
        last = {x: i for i, x in enumerate(nums)}
        mod = 10**9 + 7
        j, k = -1, 0
        for i, x in enumerate(nums):
            j = max(j, last[x])
            k += i == j
        return pow(2, k - 1, mod)
```

#### Java

```java
class Solution {
    public int numberOfGoodPartitions(int[] nums) {
        Map<Integer, Integer> last = new HashMap<>();
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            last.put(nums[i], i);
        }
        final int mod = (int) 1e9 + 7;
        int j = -1;
        int k = 0;
        for (int i = 0; i < n; ++i) {
            j = Math.max(j, last.get(nums[i]));
            k += i == j ? 1 : 0;
        }
        return qpow(2, k - 1, mod);
    }

    private int qpow(long a, int n, int mod) {
        long ans = 1;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % mod;
            }
            a = a * a % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfGoodPartitions(vector<int>& nums) {
        unordered_map<int, int> last;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            last[nums[i]] = i;
        }
        const int mod = 1e9 + 7;
        int j = -1, k = 0;
        for (int i = 0; i < n; ++i) {
            j = max(j, last[nums[i]]);
            k += i == j;
        }
        auto qpow = [&](long long a, int n, int mod) {
            long long ans = 1;
            for (; n; n >>= 1) {
                if (n & 1) {
                    ans = ans * a % mod;
                }
                a = a * a % mod;
            }
            return (int) ans;
        };
        return qpow(2, k - 1, mod);
    }
};
```

#### Go

```go
func numberOfGoodPartitions(nums []int) int {
	qpow := func(a, n, mod int) int {
		ans := 1
		for ; n > 0; n >>= 1 {
			if n&1 == 1 {
				ans = ans * a % mod
			}
			a = a * a % mod
		}
		return ans
	}
	last := map[int]int{}
	for i, x := range nums {
		last[x] = i
	}
	const mod int = 1e9 + 7
	j, k := -1, 0
	for i, x := range nums {
		j = max(j, last[x])
		if i == j {
			k++
		}
	}
	return qpow(2, k-1, mod)
}
```

#### TypeScript

```ts
function numberOfGoodPartitions(nums: number[]): number {
    const qpow = (a: number, n: number, mod: number) => {
        let ans = 1;
        for (; n; n >>= 1) {
            if (n & 1) {
                ans = Number((BigInt(ans) * BigInt(a)) % BigInt(mod));
            }
            a = Number((BigInt(a) * BigInt(a)) % BigInt(mod));
        }
        return ans;
    };
    const last: Map<number, number> = new Map();
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        last.set(nums[i], i);
    }
    const mod = 1e9 + 7;
    let [j, k] = [-1, 0];
    for (let i = 0; i < n; ++i) {
        j = Math.max(j, last.get(nums[i])!);
        if (i === j) {
            ++k;
        }
    }
    return qpow(2, k - 1, mod);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
