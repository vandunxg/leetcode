---
comments: true
difficulty: Medium
rating: 1417
source: Weekly Contest 518 Q2
---

<!-- problem:start -->

# [4044. Count Good Cyclic Rotations](https://leetcode.com/problems/count-good-cyclic-rotations)

[中文文档](/solution/4000-4099/4044.Count%20Good%20Cyclic%20Rotations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài chẵn <code>n</code>.</p>

<p>Một <strong>phép xoay vòng</strong> của <code>nums</code> được tạo ra bằng cách chọn một <span data-keyword="array-prefix">tiền tố</span> của <code>nums</code> có độ dài từ 0 đến <code>n - 1</code> (bao gồm cả hai đầu), rồi chuyển tiền tố đó xuống cuối mảng mà vẫn giữ nguyên thứ tự của tất cả phần tử.</p>

<p>Một phép xoay vòng là <strong>tốt</strong> nếu tổng của <code>n / 2</code> phần tử đầu tiên <strong>lớn hơn nghiêm ngặt</strong> tổng của <code>n / 2</code> phần tử cuối cùng.</p>

<p>Hãy trả về số phép xoay vòng <code>nums</code> là tốt.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các phép xoay vòng của <code>nums</code> là:</p>

<table>
	<thead>
		<tr>
			<th style="text-align: center; padding: 6px 12px;">Phép xoay vòng</th>
			<th style="text-align: center; padding: 6px 12px;">Tổng của <code>n / 2</code> phần tử đầu tiên</th>
			<th style="text-align: center; padding: 6px 12px;">Tổng của <code>n / 2</code> phần tử cuối cùng</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="text-align: center; padding: 6px 12px;"><code>[1, 2, 3, 4, 5, 6]</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>1 + 2 + 3 = 6</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>4 + 5 + 6 = 15</code></td>
		</tr>
		<tr>
			<td style="text-align: center; padding: 6px 12px;"><code>[2, 3, 4, 5, 6, 1]</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>2 + 3 + 4 = 9</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>5 + 6 + 1 = 12</code></td>
		</tr>
		<tr>
			<td style="text-align: center; padding: 6px 12px;"><code>[3, 4, 5, 6, 1, 2]</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>3 + 4 + 5 = 12</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>6 + 1 + 2 = 9</code></td>
		</tr>
		<tr>
			<td style="text-align: center; padding: 6px 12px;"><code>[4, 5, 6, 1, 2, 3]</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>4 + 5 + 6 = 15</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>1 + 2 + 3 = 6</code></td>
		</tr>
		<tr>
			<td style="text-align: center; padding: 6px 12px;"><code>[5, 6, 1, 2, 3, 4]</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>5 + 6 + 1 = 12</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>2 + 3 + 4 = 9</code></td>
		</tr>
		<tr>
			<td style="text-align: center; padding: 6px 12px;"><code>[6, 1, 2, 3, 4, 5]</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>6 + 1 + 2 = 9</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>3 + 4 + 5 = 12</code></td>
		</tr>
	</tbody>
</table>

<p>Tổng của nửa đầu lớn hơn tổng của nửa sau trong 3 phép xoay vòng. Vì vậy, đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các phép xoay vòng của <code>nums</code> là:</p>

<table>
	<thead>
		<tr>
			<th style="text-align: center; padding: 6px 12px;">Phép xoay vòng</th>
			<th style="text-align: center; padding: 6px 12px;">Tổng của <code>n / 2</code> phần tử đầu tiên</th>
			<th style="text-align: center; padding: 6px 12px;">Tổng của <code>n / 2</code> phần tử cuối cùng</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="text-align: center; padding: 6px 12px;"><code>[1, 2, 1, 2]</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>1 + 2 = 3</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>1 + 2 = 3</code></td>
		</tr>
		<tr>
			<td style="text-align: center; padding: 6px 12px;"><code>[2, 1, 2, 1]</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>2 + 1 = 3</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>2 + 1 = 3</code></td>
		</tr>
		<tr>
			<td style="text-align: center; padding: 6px 12px;"><code>[1, 2, 1, 2]</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>1 + 2 = 3</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>1 + 2 = 3</code></td>
		</tr>
		<tr>
			<td style="text-align: center; padding: 6px 12px;"><code>[2, 1, 2, 1]</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>2 + 1 = 3</code></td>
			<td style="text-align: center; padding: 6px 12px;"><code>2 + 1 = 3</code></td>
		</tr>
	</tbody>
</table>

<p>Không có phép xoay vòng nào là tốt vì hai tổng bằng nhau trong mọi phép xoay vòng. Vì vậy, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>n</code> là số chẵn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Một phép xoay vòng là tốt chỉ khi tổng của nửa đầu lớn hơn nghiêm ngặt tổng của nửa sau. Nếu tính lại tổng của hai nửa trong mỗi lần thì độ phức tạp sẽ là bậc hai.
>
> Khi dịch trái một vị trí, nửa đầu bỏ $\textit{nums}[i]$ và nhận $\textit{nums}[(i+m)\bmod n]$; nửa sau thực hiện ngược lại. Vì vậy, cả hai tổng đều có thể được cập nhật trong $O(1)$.
>
> Bắt đầu từ mảng ban đầu, ta xoay $n$ lần và đếm số lần $l>r$.

<!-- thinking:end -->

Gọi $n$ là độ dài mảng và $m = n / 2$. Trước tiên, tính tổng $l$ của $m$ phần tử đầu tiên và tổng $r$ của $m$ phần tử cuối cùng trong mảng ban đầu. Nếu $l > r$, tăng đáp án lên $1$.

Sau đó, bắt đầu từ mảng ban đầu và dịch vòng sang trái một vị trí, tổng cộng $n - 1$ lần. Ở lần dịch thứ $i$ ($i$ bắt đầu từ $0$), nửa đầu bỏ $\textit{nums}[i]$ và nhận $\textit{nums}[(i + m) \bmod n]$, còn nửa sau thực hiện ngược lại. Cập nhật $l$ và $r$ trong $O(1)$, đồng thời tăng đáp án mỗi khi $l > r$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countGoodRotations(self, nums: list[int]) -> int:
        n = len(nums)
        m = n // 2
        l = sum(nums[:m])
        r = sum(nums[m:])
        ans = int(l > r)
        for i in range(n - 1):
            l -= nums[i]
            r += nums[i]
            l += nums[(i + m) % n]
            r -= nums[(i + m) % n]
            ans += int(l > r)
        return ans
```

#### Java

```java
class Solution {
    public int countGoodRotations(int[] nums) {
        int n = nums.length;
        int m = n / 2;

        long l = 0, r = 0;
        for (int i = 0; i < m; i++) {
            l += nums[i];
        }
        for (int i = m; i < n; i++) {
            r += nums[i];
        }

        int ans = l > r ? 1 : 0;

        for (int i = 0; i < n - 1; i++) {
            l -= nums[i];
            r += nums[i];
            l += nums[(i + m) % n];
            r -= nums[(i + m) % n];
            ans += l > r ? 1 : 0;
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countGoodRotations(vector<int>& nums) {
        int n = nums.size();
        int m = n / 2;

        long long l = 0, r = 0;
        for (int i = 0; i < m; i++) {
            l += nums[i];
        }
        for (int i = m; i < n; i++) {
            r += nums[i];
        }

        int ans = l > r;

        for (int i = 0; i < n - 1; i++) {
            l -= nums[i];
            r += nums[i];
            l += nums[(i + m) % n];
            r -= nums[(i + m) % n];
            ans += l > r;
        }

        return ans;
    }
};
```

#### Go

```go
func countGoodRotations(nums []int) int {
	n := len(nums)
	m := n / 2

	var l, r int64
	for i := 0; i < m; i++ {
		l += int64(nums[i])
	}
	for i := m; i < n; i++ {
		r += int64(nums[i])
	}

	ans := 0
	if l > r {
		ans++
	}

	for i := 0; i < n-1; i++ {
		l -= int64(nums[i])
		r += int64(nums[i])
		l += int64(nums[(i+m)%n])
		r -= int64(nums[(i+m)%n])
		if l > r {
			ans++
		}
	}

	return ans
}
```

#### TypeScript

```ts
function countGoodRotations(nums: number[]): number {
    const n = nums.length;
    const m = Math.floor(n / 2);

    let l = 0,
        r = 0;
    for (let i = 0; i < m; i++) {
        l += nums[i];
    }
    for (let i = m; i < n; i++) {
        r += nums[i];
    }

    let ans = l > r ? 1 : 0;

    for (let i = 0; i < n - 1; i++) {
        l -= nums[i];
        r += nums[i];
        l += nums[(i + m) % n];
        r -= nums[(i + m) % n];
        ans += l > r ? 1 : 0;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
