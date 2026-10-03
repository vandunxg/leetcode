---
comments: true
difficulty: Medium
rating: 1454
source: Biweekly Contest 51 Q3
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1846. Maximum Element After Decreasing and Rearranging](https://leetcode.com/problems/maximum-element-after-decreasing-and-rearranging)

[中文文档](/solution/1800-1899/1846.Maximum%20Element%20After%20Decreasing%20and%20Rearranging/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên dương <code>arr</code>. Thực hiện một số thao tác (có thể không thực hiện thao tác nào) trên <code>arr</code> để nó thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Giá trị của phần tử <strong>đầu tiên</strong> trong <code>arr</code> phải bằng <code>1</code>.</li>
	<li>Độ lệch tuyệt đối giữa hai phần tử kề nhau bất kỳ phải <strong>nhỏ hơn hoặc bằng </strong><code>1</code>. Nói cách khác, <code>abs(arr[i] - arr[i - 1]) &lt;= 1</code> với mọi <code>i</code> thỏa mãn <code>1 &lt;= i &lt; arr.length</code> (<strong>đánh chỉ số từ 0</strong>). <code>abs(x)</code> là giá trị tuyệt đối của <code>x</code>.</li>
</ul>

<p>Có 2 loại thao tác có thể thực hiện tùy ý số lần:</p>

<ul>
	<li><strong>Giảm</strong> giá trị của bất kỳ phần tử nào trong <code>arr</code> xuống một <strong>số nguyên dương nhỏ hơn</strong>.</li>
	<li><strong>Sắp xếp lại</strong> các phần tử của <code>arr</code> theo bất kỳ thứ tự nào.</li>
</ul>

<p>Trả về <em>giá trị <strong>lớn nhất</strong> có thể có của một phần tử trong </em><code>arr</code><em> sau khi thực hiện các thao tác để thỏa mãn các điều kiện</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,2,1,2,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Ta có thể thỏa mãn các điều kiện bằng cách sắp xếp lại <code>arr</code> thành <code>[1,2,2,2,1]</code>.
Phần tử lớn nhất trong <code>arr</code> là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [100,1,1000]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Một cách để thỏa mãn các điều kiện là thực hiện như sau:
1. Sắp xếp lại <code>arr</code> thành <code>[1,100,1000]</code>.
2. Giảm giá trị của phần tử thứ hai xuống 2.
3. Giảm giá trị của phần tử thứ ba xuống 3.
Bây giờ <code>arr = [1,2,3]</code>, thỏa mãn các điều kiện.
Phần tử lớn nhất trong <code>arr is 3.</code></pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3,4,5]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Mảng đã thỏa mãn các điều kiện, và phần tử lớn nhất là 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Thuật toán tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể sắp xếp lại và giảm (nhưng không tăng) các giá trị sao cho phần tử đầu tiên là $1$ và chênh lệch giữa các phần tử kề nhau không quá $1$, đồng thời tối đa hóa phần tử cuối. Ta nên giảm các giá trị ít nhất có thể.
>
> Sắp xếp mảng, đặt phần tử đầu tiên thành $1$, rồi giới hạn mỗi phần tử tiếp theo ở mức $arr[i-1]+1$. Chuỗi không giảm thu được là dãy cao nhất thỏa mãn các điều kiện, nên phần tử cuối là đáp án.

<!-- thinking:end -->

Trước tiên, ta sắp xếp mảng rồi đặt phần tử đầu tiên của mảng bằng $1$.

Tiếp theo, ta bắt đầu duyệt mảng từ phần tử thứ hai. Nếu phần tử hiện tại lớn hơn phần tử trước đó cộng $1$, ta tham lam giảm phần tử hiện tại xuống bằng phần tử trước đó cộng $1$.

Cuối cùng, ta trả về phần tử cuối cùng của mảng.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumElementAfterDecrementingAndRearranging(self, arr: List[int]) -> int:
        arr.sort()
        arr[0] = 1
        for i in range(1, len(arr)):
            arr[i] = min(arr[i], arr[i - 1] + 1)
        return arr[-1]
```

#### Java

```java
class Solution {
    public int maximumElementAfterDecrementingAndRearranging(int[] arr) {
        int n = arr.length;
        Arrays.sort(arr);
        arr[0] = 1;
        int ans = 1;
        for (int i = 1; i < n; ++i) {
            arr[i] = Math.min(arr[i], arr[i - 1] + 1);
        }
        return arr[n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumElementAfterDecrementingAndRearranging(vector<int>& arr) {
        ranges::sort(arr);
        int n = arr.size();
        arr[0] = 1;
        for (int i = 1; i < n; ++i) {
            arr[i] = min(arr[i], arr[i - 1] + 1);
        }

        return arr[n - 1];
    }
};
```

#### Go

```go
func maximumElementAfterDecrementingAndRearranging(arr []int) int {
    slices.Sort(arr)

    arr[0] = 1
    for i := 1; i < len(arr); i++ {
        arr[i] = min(arr[i], arr[i-1]+1)
    }

    return arr[len(arr)-1]
}
```

#### TypeScript

```ts
function maximumElementAfterDecrementingAndRearranging(arr: number[]): number {
    arr.sort((a, b) => a - b);

    arr[0] = 1;
    for (let i = 1; i < arr.length; i++) {
        arr[i] = Math.min(arr[i], arr[i - 1] + 1);
    }

    return arr.at(-1)!;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_element_after_decrementing_and_rearranging(mut arr: Vec<i32>) -> i32 {
        arr.sort_unstable();

        arr[0] = 1;

        for i in 1..arr.len() {
            arr[i] = arr[i].min(arr[i - 1] + 1);
        }

        *arr.last().unwrap()
    }
}
```

#### C#

```cs
public class Solution {
    public int MaximumElementAfterDecrementingAndRearranging(int[] arr) {
        Array.Sort(arr);
        int n = arr.Length;
        arr[0] = 1;
        for (int i = 1; i < n; ++i) {
            arr[i] = Math.Min(arr[i], arr[i - 1] + 1);
        }
        return arr[n - 1];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
