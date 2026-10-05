---
comments: true
difficulty: Medium
rating: 1663
source: Biweekly Contest 190 Q2
---

<!-- problem:start -->

# [4035. Maximum Valid Split Positions I](https://leetcode.com/problems/maximum-valid-split-positions-i)

[中文文档](/solution/4000-4099/4035.Maximum%20Valid%20Split%20Positions%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Bạn có thể xóa <strong>nhiều nhất một</strong> phần tử khỏi <code>nums</code>. Gọi <code>arr</code> là mảng các phần tử còn lại theo đúng thứ tự ban đầu, và <code>m</code> là độ dài của mảng đó.</p>

<p>Một <strong>vị trí chia</strong> <code>i</code> của <code>arr</code> là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li><code>0 &lt;= i &lt; m - 1</code>, và</li>
	<li><code>gcd(arr[0..i]) == gcd(arr[i + 1..m - 1])</code>.</li>
</ul>

<p>Mảng có độ dài 1 không có vị trí chia hợp lệ.</p>

<p><strong>Điểm số</strong> của <code>arr</code> là số lượng vị trí chia hợp lệ trong mảng.</p>

<p>Trả về <strong>điểm số lớn nhất có thể</strong> của <code>arr</code>.</p>

<p>Ở đây, <code>gcd(a)</code> biểu thị <strong>ước chung lớn nhất</strong> của tất cả phần tử trong mảng <code>a</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,30,15,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách tối ưu là xóa <code>nums[2] = 15</code>. Khi đó <code>arr = [10, 30, 10]</code>.</p>

<p>Các vị trí chia là:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse; text-align:center;">
	<tbody>
		<tr>
			<th>Vị trí chia <code>i</code></th>
			<th><code>gcd(arr[0..i])</code></th>
			<th><code>gcd(arr[i + 1..m - 1])</code></th>
		</tr>
		<tr>
			<td>0</td>
			<td>10</td>
			<td>10</td>
		</tr>
		<tr>
			<td>1</td>
			<td>10</td>
			<td>10</td>
		</tr>
	</tbody>
</table>

<p>Tất cả vị trí chia đều hợp lệ. Vì vậy, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,10,14]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách tối ưu là không xóa phần tử nào. Khi đó <code>arr = [2, 10, 14]</code>.</p>

<p>Các vị trí chia là:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse; text-align:center;">
	<tbody>
		<tr>
			<th>Vị trí chia <code>i</code></th>
			<th><code>gcd(arr[0..i])</code></th>
			<th><code>gcd(arr[i + 1..m - 1])</code></th>
		</tr>
		<tr>
			<td>0</td>
			<td>2</td>
			<td>2</td>
		</tr>
		<tr>
			<td>1</td>
			<td>2</td>
			<td>14</td>
		</tr>
	</tbody>
</table>

<p>Chỉ vị trí chia tại chỉ số 0 là hợp lệ. Vì vậy, đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng còn lại duy nhất có vị trí chia là <code>arr = [2, 4]</code>.</p>

<p>Các vị trí chia là:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse; text-align:center;">
	<tbody>
		<tr>
			<th>Vị trí chia <code>i</code></th>
			<th><code>gcd(arr[0..i])</code></th>
			<th><code>gcd(arr[i + 1..m - 1])</code></th>
		</tr>
		<tr>
			<td>0</td>
			<td>2</td>
			<td>4</td>
		</tr>
	</tbody>
</table>

<p>Không có vị trí chia hợp lệ nào. Vì vậy, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt chỉ số phần tử bị xóa + GCD tiền tố và hậu tố

<!-- thinking:start -->

> **Tư duy**
>
> Vị trí chia $i$ hợp lệ khi và chỉ khi GCD của tiền tố bên trái bằng GCD của hậu tố bên phải. Với $n\le 1000$, chúng ta không cần phân tích cách một lần xóa làm thay đổi chuỗi GCD.
>
> Duyệt chỉ số phần tử bị xóa (và trường hợp không xóa), xây dựng GCD tiền tố và hậu tố của mảng còn lại, đếm các vị trí chia có hai giá trị bằng nhau, rồi giữ giá trị lớn nhất.
>
> Mỗi lượt tính điểm số có độ phức tạp $O(n\log M)$, nên tổng độ phức tạp thời gian $O(n^2\log M)$ là chấp nhận được.

<!-- thinking:end -->

Vì độ dài mảng thỏa mãn $n \leq 1000$, chúng ta có thể duyệt chỉ số phần tử bị xóa (bao gồm cả trường hợp không xóa phần tử nào) để tạo mảng $\textit{arr}$, tính điểm số của $\textit{arr}$, rồi lấy giá trị lớn nhất trong tất cả trường hợp.

Với mảng $\textit{arr}$ có độ dài $m$, trước hết chúng ta tính mảng GCD tiền tố $\textit{pre}$ và mảng GCD hậu tố $\textit{suf}$, trong đó $\textit{pre}[i] = \gcd(\textit{arr}[0..i])$ và $\textit{suf}[i] = \gcd(\textit{arr}[i..m - 1])$. Vị trí chia $i$ hợp lệ khi và chỉ khi $\textit{pre}[i] = \textit{suf}[i + 1]$, nên điểm số của $\textit{arr}$ là số lượng chỉ số thỏa mãn điều kiện này.

Độ phức tạp thời gian là $O(n^2 \times \log M)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$, còn $M$ là giá trị lớn nhất trong mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxValidSplits(self, nums: List[int]) -> int:
        def calc(arr: List[int]) -> int:
            m = len(arr)
            pre = list(accumulate(arr, gcd))
            suf = list(accumulate(arr[::-1], gcd))[::-1]
            return sum(pre[i] == suf[i + 1] for i in range(m - 1))

        ans = calc(nums)
        for i in range(len(nums)):
            ans = max(ans, calc(nums[:i] + nums[i + 1 :]))
        return ans
```

#### Java

```java
class Solution {
    public int maxValidSplits(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int del = -1; del < n; ++del) {
            int m = del == -1 ? n : n - 1;
            int[] arr = new int[m];
            for (int i = 0, j = 0; i < n; ++i) {
                if (i != del) {
                    arr[j++] = nums[i];
                }
            }
            ans = Math.max(ans, calc(arr));
        }
        return ans;
    }

    private int calc(int[] arr) {
        int m = arr.length;
        int[] pre = new int[m];
        int[] suf = new int[m];
        pre[0] = arr[0];
        for (int i = 1; i < m; ++i) {
            pre[i] = gcd(pre[i - 1], arr[i]);
        }
        suf[m - 1] = arr[m - 1];
        for (int i = m - 2; i >= 0; --i) {
            suf[i] = gcd(suf[i + 1], arr[i]);
        }
        int ans = 0;
        for (int i = 0; i < m - 1; ++i) {
            if (pre[i] == suf[i + 1]) {
                ++ans;
            }
        }
        return ans;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxValidSplits(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        for (int del = -1; del < n; ++del) {
            vector<int> arr;
            arr.reserve(n);
            for (int i = 0; i < n; ++i) {
                if (i != del) {
                    arr.push_back(nums[i]);
                }
            }
            ans = max(ans, calc(arr));
        }
        return ans;
    }

private:
    int calc(const vector<int>& arr) {
        int m = arr.size();
        vector<int> pre(m), suf(m);
        pre[0] = arr[0];
        for (int i = 1; i < m; ++i) {
            pre[i] = gcd(pre[i - 1], arr[i]);
        }
        suf[m - 1] = arr[m - 1];
        for (int i = m - 2; i >= 0; --i) {
            suf[i] = gcd(suf[i + 1], arr[i]);
        }
        int ans = 0;
        for (int i = 0; i < m - 1; ++i) {
            if (pre[i] == suf[i + 1]) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxValidSplits(nums []int) int {
	n := len(nums)
	calc := func(arr []int) int {
		m := len(arr)
		pre := make([]int, m)
		suf := make([]int, m)
		pre[0] = arr[0]
		for i := 1; i < m; i++ {
			pre[i] = gcd(pre[i-1], arr[i])
		}
		suf[m-1] = arr[m-1]
		for i := m - 2; i >= 0; i-- {
			suf[i] = gcd(suf[i+1], arr[i])
		}
		ans := 0
		for i := 0; i < m-1; i++ {
			if pre[i] == suf[i+1] {
				ans++
			}
		}
		return ans
	}
	ans := 0
	for del := -1; del < n; del++ {
		arr := make([]int, 0, n)
		for i, x := range nums {
			if i != del {
				arr = append(arr, x)
			}
		}
		ans = max(ans, calc(arr))
	}
	return ans
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function maxValidSplits(nums: number[]): number {
    const n = nums.length;
    const gcd = (a: number, b: number): number => (b === 0 ? a : gcd(b, a % b));
    const calc = (arr: number[]): number => {
        const m = arr.length;
        const pre: number[] = Array(m).fill(0);
        const suf: number[] = Array(m).fill(0);
        pre[0] = arr[0];
        for (let i = 1; i < m; ++i) {
            pre[i] = gcd(pre[i - 1], arr[i]);
        }
        suf[m - 1] = arr[m - 1];
        for (let i = m - 2; i >= 0; --i) {
            suf[i] = gcd(suf[i + 1], arr[i]);
        }
        let ans = 0;
        for (let i = 0; i < m - 1; ++i) {
            if (pre[i] === suf[i + 1]) {
                ++ans;
            }
        }
        return ans;
    };
    let ans = 0;
    for (let del = -1; del < n; ++del) {
        const arr: number[] = [];
        for (let i = 0; i < n; ++i) {
            if (i !== del) {
                arr.push(nums[i]);
            }
        }
        ans = Math.max(ans, calc(arr));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
