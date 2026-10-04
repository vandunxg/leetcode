---
comments: true
difficulty: Medium
rating: 1502
source: Biweekly Contest 148 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3424. Minimum Cost to Make Arrays Identical](https://leetcode.com/problems/minimum-cost-to-make-arrays-identical)

[中文文档](/solution/3400-3499/3424.Minimum%20Cost%20to%20Make%20Arrays%20Identical/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>arr</code> và <code>brr</code> có cùng độ dài <code>n</code>, cùng một số nguyên <code>k</code>. Bạn có thể thực hiện các thao tác sau trên <code>arr</code> với <em>bất kỳ</em> số lần nào:</p>

<ul>
	<li>Chia <code>arr</code> thành số lượng <em>bất kỳ</em> các <strong>liên tiếp</strong> <span data-keyword="subarray-nonempty">mảng con</span> và sắp xếp lại các mảng con này theo <em>bất kỳ thứ tự nào</em>. Thao tác này có chi phí cố định là <code>k</code>.</li>
	<li>
	<p>Chọn một phần tử bất kỳ trong <code>arr</code> rồi cộng hoặc trừ một số nguyên dương <code>x</code> vào phần tử đó. Chi phí của thao tác này là <code>x</code>.</p>
	</li>
</ul>

<p>Trả về <strong>tổng chi phí nhỏ nhất</strong> để biến <code>arr</code> thành <strong>bằng</strong> <code>brr</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">arr = [-7,9,5], brr = [7,-2,-5], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chia <code>arr</code> thành hai mảng con liên tiếp: <code>[-7]</code> và <code>[9, 5]</code>, sau đó sắp xếp lại thành <code>[9, 5, -7]</code>, với chi phí là 2.</li>
	<li>Trừ 2 khỏi phần tử <code>arr[0]</code>. Mảng trở thành <code>[7, 5, -7]</code>. Chi phí của thao tác này là 2.</li>
	<li>Trừ 7 khỏi phần tử <code>arr[1]</code>. Mảng trở thành <code>[7, -2, -7]</code>. Chi phí của thao tác này là 7.</li>
	<li>Cộng 2 vào phần tử <code>arr[2]</code>. Mảng trở thành <code>[7, -2, -5]</code>. Chi phí của thao tác này là 2.</li>
</ul>

<p>Tổng chi phí để hai mảng bằng nhau là <code>2 + 2 + 7 + 2 = 13</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">arr = [2,1], brr = [2,1], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì hai mảng đã bằng nhau nên không cần thực hiện thao tác nào, và tổng chi phí là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length == brr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= 2 * 10<sup>10</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= arr[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= brr[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Nếu không sắp xếp lại, chi phí là tổng các hiệu tuyệt đối tại từng chỉ số. Nếu sắp xếp lại, trước tiên ta phải trả chi phí $k$, sau đó có thể ghép $\textit{arr}$ với $\textit{brr}$ theo bất kỳ cách nào.
>
> Việc tìm kiếm các hoán vị là bất khả thi với $n\le 10^5$. Sắp xếp cả hai mảng rồi ghép các phần tử theo thứ tự sẽ tối thiểu hóa $\sum|a_i-b_i|$.
>
> Ta tính $c_1$ khi không chia mảng, và $c_2=k$ cộng với chi phí ghép sau khi sắp xếp, rồi lấy giá trị nhỏ hơn. Một phần sắp xếp lại không thể tốt hơn hai phương án “không sắp xếp lại” hoặc “trả $k$ và sắp xếp lại toàn bộ”.

<!-- thinking:end -->

Nếu không được phép chia mảng, ta có thể tính trực tiếp tổng các hiệu tuyệt đối giữa hai mảng tại từng vị trí, gọi đó là tổng chi phí $c_1$. Nếu được phép chia, ta có thể chia mảng $\textit{arr}$ thành $n$ mảng con có độ dài 1, sau đó sắp xếp lại chúng theo bất kỳ thứ tự nào và so sánh với mảng $\textit{brr}$ để tính tổng các hiệu tuyệt đối, gọi đó là tổng chi phí $c_2$. Để tối thiểu hóa $c_2$, ta có thể sắp xếp cả hai mảng rồi tính tổng các hiệu tuyệt đối. Kết quả cuối cùng là $\min(c_1, c_2 + k)$.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, arr: List[int], brr: List[int], k: int) -> int:
        c1 = sum(abs(a - b) for a, b in zip(arr, brr))
        arr.sort()
        brr.sort()
        c2 = k + sum(abs(a - b) for a, b in zip(arr, brr))
        return min(c1, c2)
```

#### Java

```java
class Solution {
    public long minCost(int[] arr, int[] brr, long k) {
        long c1 = calc(arr, brr);
        Arrays.sort(arr);
        Arrays.sort(brr);
        long c2 = calc(arr, brr) + k;
        return Math.min(c1, c2);
    }

    private long calc(int[] arr, int[] brr) {
        long ans = 0;
        for (int i = 0; i < arr.length; ++i) {
            ans += Math.abs(arr[i] - brr[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minCost(vector<int>& arr, vector<int>& brr, long long k) {
        auto calc = [&](vector<int>& arr, vector<int>& brr) {
            long long ans = 0;
            for (int i = 0; i < arr.size(); ++i) {
                ans += abs(arr[i] - brr[i]);
            }
            return ans;
        };
        long long c1 = calc(arr, brr);
        ranges::sort(arr);
        ranges::sort(brr);
        long long c2 = calc(arr, brr) + k;
        return min(c1, c2);
    }
};
```

#### Go

```go
func minCost(arr []int, brr []int, k int64) int64 {
	calc := func(a, b []int) (ans int64) {
		for i := range a {
			ans += int64(abs(a[i] - b[i]))
		}
		return
	}
	c1 := calc(arr, brr)
	sort.Ints(arr)
	sort.Ints(brr)
	c2 := calc(arr, brr) + k
	return min(c1, c2)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minCost(arr: number[], brr: number[], k: number): number {
    const calc = (a: number[], b: number[]) => {
        let ans = 0;
        for (let i = 0; i < a.length; ++i) {
            ans += Math.abs(a[i] - b[i]);
        }
        return ans;
    };
    const c1 = calc(arr, brr);
    arr.sort((a, b) => a - b);
    brr.sort((a, b) => a - b);
    const c2 = calc(arr, brr) + k;
    return Math.min(c1, c2);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_cost(mut arr: Vec<i32>, mut brr: Vec<i32>, k: i64) -> i64 {
        let c1: i64 = arr.iter()
            .zip(&brr)
            .map(|(a, b)| (*a - *b).abs() as i64)
            .sum();

        arr.sort_unstable();
        brr.sort_unstable();

        let c2: i64 = k + arr.iter()
            .zip(&brr)
            .map(|(a, b)| (*a - *b).abs() as i64)
            .sum::<i64>();

        c1.min(c2)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
