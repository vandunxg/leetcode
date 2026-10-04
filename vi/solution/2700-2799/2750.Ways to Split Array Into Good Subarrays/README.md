---
comments: true
difficulty: Medium
rating: 1597
source: Weekly Contest 351 Q3
tags:
    - Array
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [2750. Ways to Split Array Into Good Subarrays](https://leetcode.com/problems/ways-to-split-array-into-good-subarrays)

[中文文档](/solution/2700-2799/2750.Ways%20to%20Split%20Array%20Into%20Good%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng nhị phân <code>nums</code>.</p>

<p>Một mảng con của một mảng là <strong>tốt</strong> nếu nó chứa <strong>chính xác</strong> <strong>một</strong> phần tử có giá trị <code>1</code>.</p>

<p>Trả về <em>một số nguyên biểu thị số cách chia mảng </em><code>nums</code><em> thành các mảng con <strong>tốt</strong></em>. Vì số này có thể rất lớn, hãy trả về kết quả <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Mảng con là một dãy phần tử liên tiếp <strong>không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,0,0,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 cách chia nums thành các mảng con tốt:
- [0,1] [0,0,1]
- [0,1,0] [0,1]
- [0,1,0,0] [1]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,0]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có 1 cách chia nums thành các mảng con tốt:
- [0,1,0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nguyên lý nhân

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con tốt chứa chính xác một $1$; ta cần đếm số cách chia mảng thành các phần như vậy. Các vị trí cắt được quyết định hoàn toàn bởi vị trí của các số 1.
>
> Giữa hai số 1 liên tiếp tại $j$ và $i$ có $i-j$ vị trí để cắt, và các đoạn độc lập với nhau, vì vậy đáp án là tích của các khoảng cách đó. Nếu không có số 1 nào thì đáp án là $0$.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể đặt một đường phân cách giữa hai $1$s. Giả sử chỉ số của hai $1$s lần lượt là $j$ và $i$, thì có $i - j$ đường phân cách khác nhau có thể đặt. Ta tìm tất cả các cặp $j$ và $i$ thỏa mãn điều kiện, rồi nhân tất cả các giá trị $i - j$ với nhau. Nếu không tìm thấy đường phân cách nào giữa hai $1$s, điều đó có nghĩa là mảng không có $1$s nào và đáp án là $0$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfGoodSubarraySplits(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        ans, j = 1, -1
        for i, x in enumerate(nums):
            if x == 0:
                continue
            if j > -1:
                ans = ans * (i - j) % mod
            j = i
        return 0 if j == -1 else ans
```

#### Java

```java
class Solution {
    public int numberOfGoodSubarraySplits(int[] nums) {
        final int mod = (int) 1e9 + 7;
        int ans = 1, j = -1;
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] == 0) {
                continue;
            }
            if (j > -1) {
                ans = (int) ((long) ans * (i - j) % mod);
            }
            j = i;
        }
        return j == -1 ? 0 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfGoodSubarraySplits(vector<int>& nums) {
        const int mod = 1e9 + 7;
        int ans = 1, j = -1;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] == 0) {
                continue;
            }
            if (j > -1) {
                ans = 1LL * ans * (i - j) % mod;
            }
            j = i;
        }
        return j == -1 ? 0 : ans;
    }
};
```

#### Go

```go
func numberOfGoodSubarraySplits(nums []int) int {
	const mod int = 1e9 + 7
	ans, j := 1, -1
	for i, x := range nums {
		if x == 0 {
			continue
		}
		if j > -1 {
			ans = ans * (i - j) % mod
		}
		j = i
	}
	if j == -1 {
		return 0
	}
	return ans
}
```

#### TypeScript

```ts
function numberOfGoodSubarraySplits(nums: number[]): number {
    let ans = 1;
    let j = -1;
    const mod = 10 ** 9 + 7;
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        if (nums[i] === 0) {
            continue;
        }
        if (j > -1) {
            ans = (ans * (i - j)) % mod;
        }
        j = i;
    }
    return j === -1 ? 0 : ans;
}
```

#### C#

```cs
public class Solution {
    public int NumberOfGoodSubarraySplits(int[] nums) {
        long ans = 1, j = -1;
        int mod = 1000000007;
        int n = nums.Length;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == 0) {
                continue;
            }
            if (j > -1) {
                ans = ans * (i - j) % mod;
            }
            j = i;
        }
        return j == -1 ? 0 : (int) ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
