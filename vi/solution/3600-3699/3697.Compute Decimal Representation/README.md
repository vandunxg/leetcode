---
comments: true
difficulty: Easy
rating: 1250
source: Weekly Contest 469 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3697. Compute Decimal Representation](https://leetcode.com/problems/compute-decimal-representation)

[Tài liệu tiếng Trung](/solution/3600-3699/3697.Compute%20Decimal%20Representation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <strong>dương</strong> <code>n</code>.</p>

<p>Một số nguyên dương là một <strong>thành phần cơ số 10</strong> nếu nó là tích của một chữ số từ 1 đến 9 và một lũy thừa không âm của 10. Ví dụ, 500, 30 và 7 là các <strong>thành phần cơ số 10</strong>, còn 537, 102 và 11 thì không.</p>

<p>Hãy biểu diễn <code>n</code> dưới dạng tổng <strong>chỉ gồm</strong> các thành phần cơ số 10, sử dụng <strong>ít thành phần cơ số 10 nhất</strong> có thể.</p>

<p>Trả về một mảng chứa các <strong>thành phần cơ số 10</strong> này theo thứ tự <strong>giảm dần</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 537</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[500,30,7]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể biểu diễn 537 thành <code>500 + 30 + 7</code>. Không thể biểu diễn 537 dưới dạng tổng sử dụng ít hơn 3 thành phần cơ số 10.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 102</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[100,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể biểu diễn 102 thành <code>100 + 2</code>. 102 không phải là một thành phần cơ số 10, nên cần 2 thành phần cơ số 10.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[6]</span></p>

<p><strong>Giải thích:</strong></p>

<p>6 là một thành phần cơ số 10.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Tách $n$ thành các số hạng $d\times 10^p$ và đưa chúng ra theo thứ tự giảm dần. Phép modulo ở các chữ số thấp hơn cho ta từng chữ số; bỏ qua các chữ số 0.
>
> Duy trì giá trị vị trí $p$. Mỗi lần $\textit{divmod}$ tạo ra một chữ số $v$; nếu chữ số này khác 0, thêm $p\cdot v$, sau đó nhân $p$ với $10$.
>
> Đảo ngược danh sách để vị trí cao hơn đứng trước.

<!-- thinking:end -->

Ta có thể liên tục thực hiện các phép modulo và chia trên $n$. Mỗi kết quả modulo nhân với giá trị vị trí hiện tại $p$ sẽ tạo thành một thành phần cơ số 10. Nếu kết quả modulo khác $0$, ta thêm thành phần này vào đáp án. Sau đó, ta nhân $p$ với $10$ và tiếp tục xử lý vị trí tiếp theo.

Cuối cùng, ta đảo ngược đáp án để sắp xếp các phần tử theo thứ tự giảm dần.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là số nguyên dương đầu vào. Độ phức tạp không gian là $O(\log n)$ để lưu đáp án.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def decimalRepresentation(self, n: int) -> List[int]:
        ans = []
        p = 1
        while n:
            n, v = divmod(n, 10)
            if v:
                ans.append(p * v)
            p *= 10
        ans.reverse()
        return ans
```

#### Java

```java
class Solution {
    public int[] decimalRepresentation(int n) {
        List<Integer> parts = new ArrayList<>();
        int p = 1;
        while (n > 0) {
            int v = n % 10;
            n /= 10;
            if (v != 0) {
                parts.add(p * v);
            }
            p *= 10;
        }
        Collections.reverse(parts);
        int[] ans = new int[parts.size()];
        for (int i = 0; i < parts.size(); ++i) {
            ans[i] = parts.get(i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> decimalRepresentation(int n) {
        vector<int> ans;
        long long p = 1;
        while (n > 0) {
            int v = n % 10;
            n /= 10;
            if (v != 0) {
                ans.push_back(p * v);
            }
            p *= 10;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func decimalRepresentation(n int) []int {
    ans := []int{}
    p := 1
    for n > 0 {
        v := n % 10
        n /= 10
        if v != 0 {
            ans = append(ans, p*v)
        }
        p *= 10
    }
    slices.Reverse(ans)
    return ans
}
```

#### TypeScript

```ts
function decimalRepresentation(n: number): number[] {
    const ans: number[] = [];
    let p: number = 1;
    while (n) {
        const v = n % 10;
        n = (n / 10) | 0;
        if (v) {
            ans.push(p * v);
        }
        p *= 10;
    }
    ans.reverse();
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
