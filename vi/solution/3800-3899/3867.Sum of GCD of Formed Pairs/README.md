---
comments: true
difficulty: Medium
rating: 1406
source: Biweekly Contest 178 Q2
tags:
    - Array
    - Math
    - Two Pointers
    - Number Theory
    - Sorting
    - Simulation
---

<!-- problem:start -->

# [3867. Sum of GCD of Formed Pairs](https://leetcode.com/problems/sum-of-gcd-of-formed-pairs)

[中文文档](/solution/3800-3899/3867.Sum%20of%20GCD%20of%20Formed%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p>Hãy xây dựng một mảng <code>prefixGcd</code>, trong đó với mỗi chỉ số <code>i</code>:</p>

<ul>
	<li>Gọi <code>mx<sub>i</sub> = max(nums[0], nums[1], ..., nums[i])</code>.</li>
	<li><code>prefixGcd[i] = gcd(nums[i], mx<sub>i</sub>)</code>.</li>
</ul>

<p>Sau khi xây dựng <code>prefixGcd</code>:</p>

<ul>
	<li>Sắp xếp <code>prefixGcd</code> theo thứ tự <strong>không giảm</strong>.</li>
	<li>Tạo các cặp bằng cách lấy phần tử <strong>nhỏ nhất chưa ghép</strong> và phần tử <strong>lớn nhất chưa ghép</strong>.</li>
	<li>Lặp lại quá trình này cho đến khi không thể tạo thêm cặp nào.</li>
	<li>Với mỗi cặp được tạo, <strong>tính</strong> <code>gcd</code> của hai phần tử.</li>
	<li>Nếu <code>n</code> là số lẻ, phần tử <strong>ở giữa</strong> trong mảng <code>prefixGcd</code> sẽ <strong>không được ghép</strong> và cần được bỏ qua.</li>
</ul>

<p>Trả về một số nguyên biểu thị <strong>tổng các giá trị GCD</strong> của tất cả các cặp được tạo.</p>
Thuật ngữ <code>gcd(a, b)</code> biểu thị <strong>ước chung lớn nhất</strong> của <code>a</code> và <code>b</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,6,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xây dựng <code>prefixGcd</code>:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>nums[i]</code></th>
			<th style="border: 1px solid black;"><code>mx<sub>i</sub></code></th>
			<th style="border: 1px solid black;"><code>prefixGcd[i]</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">6</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
	</tbody>
</table>

<p><code>prefixGcd = [2, 6, 2]</code>. Sau khi sắp xếp, ta được <code>[2, 2, 6]</code>.</p>

<p>Ghép phần tử nhỏ nhất với phần tử lớn nhất: <code>gcd(2, 6) = 2</code>. Phần tử 2 ở giữa còn lại bị bỏ qua. Do đó, tổng là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,6,2,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xây dựng <code>prefixGcd</code>:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>nums[i]</code></th>
			<th style="border: 1px solid black;"><code>mx<sub>i</sub></code></th>
			<th style="border: 1px solid black;"><code>prefixGcd[i]</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">6</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">8</td>
			<td style="border: 1px solid black;">8</td>
			<td style="border: 1px solid black;">8</td>
		</tr>
	</tbody>
</table>

<p><code>prefixGcd = [3, 6, 2, 8]</code>. Sau khi sắp xếp, ta được <code>[2, 3, 6, 8]</code>.</p>

<p>Tạo các cặp: <code>gcd(2, 8) = 2</code> và <code>gcd(3, 6) = 3</code>. Do đó, tổng là <code>2 + 3 = 5</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>​​​​​​​9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Xây dựng $\textit{prefixGcd}$ từ các giá trị lớn nhất trên prefix, sắp xếp rồi tính tổng $\gcd$ của các cặp min-max. Vì $n \le 10^5$, ta làm đúng theo định nghĩa.
>
> Duy trì giá trị lớn nhất trên prefix trong một lần duyệt, đồng thời tính $\gcd(nums[i],mx)$.
>
> Sau khi sắp xếp, ghép phần tử nhỏ thứ $i$ với phần tử lớn thứ $i$; bỏ phần tử ở giữa khi $n$ lẻ.
>
> Có $\lfloor n/2 \rfloor$ cặp.

<!-- thinking:end -->

Ta mô phỏng theo mô tả của đề bài.

Ta tạo một mảng $\textit{prefixGcd}$ để lưu giá trị tại mỗi chỉ số $i$. Đồng thời, ta duy trì một biến $mx$ để theo dõi giá trị lớn nhất hiện tại. Với mỗi phần tử $nums[i]$, ta cập nhật $mx$ và tính giá trị của $\textit{prefixGcd}[i]$. Sau đó, ta sắp xếp $\textit{prefixGcd}$ và tính tổng các GCD của những cặp được tạo.

Độ phức tạp thời gian là $O(n \log M + n \log n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng và $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def gcdSum(self, nums: list[int]) -> int:
        n = len(nums)
        prefix_gcd = [0] * n
        mx = 0
        for i, x in enumerate(nums):
            mx = max(mx, x)
            prefix_gcd[i] = gcd(x, mx)
        prefix_gcd.sort()
        return sum(gcd(prefix_gcd[i], prefix_gcd[-i - 1]) for i in range(n // 2))
```

#### Java

```java
class Solution {
    public long gcdSum(int[] nums) {
        int n = nums.length;
        int[] prefixGcd = new int[n];
        int mx = 0;

        for (int i = 0; i < n; i++) {
            int x = nums[i];
            mx = Math.max(mx, x);
            prefixGcd[i] = gcd(x, mx);
        }

        Arrays.sort(prefixGcd);

        long ans = 0;
        for (int i = 0; i < n / 2; i++) {
            ans += gcd(prefixGcd[i], prefixGcd[n - i - 1]);
        }

        return ans;
    }

    private int gcd(int a, int b) {
        while (b != 0) {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long gcdSum(vector<int>& nums) {
        int n = nums.size();
        vector<int> prefix_gcd(n);
        int mx = 0;

        for (int i = 0; i < n; i++) {
            int x = nums[i];
            mx = max(mx, x);
            prefix_gcd[i] = gcd(x, mx);
        }

        sort(prefix_gcd.begin(), prefix_gcd.end());

        long long ans = 0;
        for (int i = 0; i < n / 2; i++) {
            ans += gcd(prefix_gcd[i], prefix_gcd[n - i - 1]);
        }

        return ans;
    }
};
```

#### Go

```go
func gcdSum(nums []int) int64 {
	n := len(nums)
	prefixGcd := make([]int, n)
	mx := 0

	for i, x := range nums {
		if x > mx {
			mx = x
		}
		prefixGcd[i] = gcd(x, mx)
	}

	sort.Ints(prefixGcd)

	var ans int64
	for i := 0; i < n/2; i++ {
		ans += int64(gcd(prefixGcd[i], prefixGcd[n-i-1]))
	}

	return ans
}

func gcd(a, b int) int {
	for b != 0 {
		a, b = b, a%b
	}
	return a
}
```

#### TypeScript

```ts
function gcdSum(nums: number[]): number {
    const n = nums.length;
    const prefixGcd: number[] = new Array(n);
    let mx = 0;

    for (let i = 0; i < n; i++) {
        const x = nums[i];
        mx = Math.max(mx, x);
        prefixGcd[i] = gcd(x, mx);
    }

    prefixGcd.sort((a, b) => a - b);

    let ans = 0;
    for (let i = 0; i < n >> 1; i++) {
        ans += gcd(prefixGcd[i], prefixGcd[n - i - 1]);
    }

    return ans;
}

function gcd(a: number, b: number): number {
    while (b !== 0) {
        const t = a % b;
        a = b;
        b = t;
    }
    return a;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
