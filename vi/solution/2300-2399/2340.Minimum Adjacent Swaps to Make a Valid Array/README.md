---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [2340. Minimum Adjacent Swaps to Make a Valid Array 🔒](https://leetcode.com/problems/minimum-adjacent-swaps-to-make-a-valid-array)

[中文文档](/solution/2300-2399/2340.Minimum%20Adjacent%20Swaps%20to%20Make%20a%20Valid%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Có thể thực hiện các phép <strong>hoán đổi</strong> các phần tử <strong>liền kề</strong> trên <code>nums</code>.</p>

<p>Một mảng <strong>hợp lệ</strong> thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Phần tử lớn nhất (một trong các phần tử lớn nhất nếu có nhiều phần tử) nằm ở vị trí ngoài cùng bên phải của mảng.</li>
	<li>Phần tử nhỏ nhất (một trong các phần tử nhỏ nhất nếu có nhiều phần tử) nằm ở vị trí ngoài cùng bên trái của mảng.</li>
</ul>

<p>Trả về số phép hoán đổi <em><strong>ít nhất</strong> cần thực hiện để biến </em><code>nums</code><em> thành một mảng hợp lệ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,5,5,3,1]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Thực hiện các phép hoán đổi sau:
- Phép hoán đổi 1: Hoán đổi phần tử thứ <sup>3</sup> và thứ <sup>4</sup>, khi đó nums là [3,4,5,<u><strong>3</strong></u>,<u><strong>5</strong></u>,1].
- Phép hoán đổi 2: Hoán đổi phần tử thứ <sup>4</sup> và thứ <sup>5</sup>, khi đó nums là [3,4,5,3,<u><strong>1</strong></u>,<u><strong>5</strong></u>].
- Phép hoán đổi 3: Hoán đổi phần tử thứ <sup>3</sup> và thứ <sup>4</sup>, khi đó nums là [3,4,5,<u><strong>1</strong></u>,<u><strong>3</strong></u>,5].
- Phép hoán đổi 4: Hoán đổi phần tử thứ <sup>2</sup> và thứ <sup>3</sup>, khi đó nums là [3,4,<u><strong>1</strong></u>,<u><strong>5</strong></u>,3,5].
- Phép hoán đổi 5: Hoán đổi phần tử thứ <sup>1</sup> và thứ <sup>2</sup>, khi đó nums là [3,<u><strong>1</strong></u>,<u><strong>4</strong></u>,5,3,5].
- Phép hoán đổi 6: Hoán đổi phần tử thứ <sup>0</sup> và thứ <sup>1</sup>, khi đó nums là [<u><strong>1</strong></u>,<u><strong>3</strong></u>,4,5,3,5].
Có thể chứng minh rằng cần ít nhất 6 phép hoán đổi để biến mảng thành mảng hợp lệ.
</pre>

<strong class="example">Ví dụ 2:</strong>

<pre>
<strong>Đầu vào:</strong> nums = [9]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mảng đã hợp lệ, nên trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duy trì chỉ số của các phần tử cực trị và phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ cần đưa một phần tử nhỏ nhất ngoài cùng bên trái và một phần tử lớn nhất ngoài cùng bên phải. Một lần duyệt là đủ để ghi lại hai chỉ số này.
>
> Nếu hai phần tử không vượt qua nhau, cộng hai khoảng cách; nếu phần tử nhỏ nhất nằm bên phải phần tử lớn nhất, hai đường di chuyển dùng chung một phép hoán đổi nên trừ đi một.

<!-- thinking:end -->

Ta có thể dùng các chỉ số $i$ và $j$ lần lượt để ghi lại chỉ số của giá trị nhỏ nhất đầu tiên và giá trị lớn nhất cuối cùng trong mảng $\textit{nums}$. Duyệt qua mảng $\textit{nums}$ để cập nhật giá trị của $i$ và $j$.

Tiếp theo, ta cần xét số phép hoán đổi.

- Nếu $i = j$, nghĩa là mảng $\textit{nums}$ đã là một mảng hợp lệ, nên không cần hoán đổi. Trả về $0$.
- Nếu $i < j$, nghĩa là giá trị nhỏ nhất trong mảng $\textit{nums}$ nằm bên trái giá trị lớn nhất. Số phép hoán đổi cần thực hiện là $i + n - 1 - j$, trong đó $n$ là độ dài của mảng $\textit{nums}$.
- Nếu $i > j$, nghĩa là giá trị nhỏ nhất trong mảng $\textit{nums}$ nằm bên phải giá trị lớn nhất. Số phép hoán đổi cần thực hiện là $i + n - 1 - j - 1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSwaps(self, nums: List[int]) -> int:
        i = j = 0
        for k, v in enumerate(nums):
            if v < nums[i] or (v == nums[i] and k < i):
                i = k
            if v >= nums[j] or (v == nums[j] and k > j):
                j = k
        return 0 if i == j else i + len(nums) - 1 - j - (i > j)
```

#### Java

```java
class Solution {
    public int minimumSwaps(int[] nums) {
        int n = nums.length;
        int i = 0, j = 0;
        for (int k = 0; k < n; ++k) {
            if (nums[k] < nums[i] || (nums[k] == nums[i] && k < i)) {
                i = k;
            }
            if (nums[k] > nums[j] || (nums[k] == nums[j] && k > j)) {
                j = k;
            }
        }
        if (i == j) {
            return 0;
        }
        return i + n - 1 - j - (i > j ? 1 : 0);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSwaps(vector<int>& nums) {
        int n = nums.size();
        int i = 0, j = 0;
        for (int k = 0; k < n; ++k) {
            if (nums[k] < nums[i] || (nums[k] == nums[i] && k < i)) {
                i = k;
            }
            if (nums[k] > nums[j] || (nums[k] == nums[j] && k > j)) {
                j = k;
            }
        }
        if (i == j) {
            return 0;
        }
        return i + n - 1 - j - (i > j);
    }
};
```

#### Go

```go
func minimumSwaps(nums []int) int {
	var i, j int
	for k, v := range nums {
		if v < nums[i] || (v == nums[i] && k < i) {
			i = k
		}
		if v > nums[j] || (v == nums[j] && k > j) {
			j = k
		}
	}
	if i == j {
		return 0
	}
	if i < j {
		return i + len(nums) - 1 - j
	}
	return i + len(nums) - 2 - j
}
```

#### TypeScript

```ts
function minimumSwaps(nums: number[]): number {
    let i = 0;
    let j = 0;
    const n = nums.length;
    for (let k = 0; k < n; ++k) {
        if (nums[k] < nums[i] || (nums[k] == nums[i] && k < i)) {
            i = k;
        }
        if (nums[k] > nums[j] || (nums[k] == nums[j] && k > j)) {
            j = k;
        }
    }
    return i == j ? 0 : i + n - 1 - j - (i > j ? 1 : 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
