---
comments: true
difficulty: Medium
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [3616. Number of Student Replacements 🔒](https://leetcode.com/problems/number-of-student-replacements)

[中文文档](/solution/3600-3699/3616.Number%20of%20Student%20Replacements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>ranks</code>, trong đó <code>ranks[i]</code> là thứ hạng của học sinh thứ <code>i<sup>th</sup></code> đến theo <strong>đúng thứ tự</strong>. Số nhỏ hơn biểu thị thứ hạng <strong>tốt hơn</strong>.</p>

<p>Ban đầu, học sinh đầu tiên mặc định được <strong>chọn</strong>.</p>

<p>Một lần <strong>thay thế</strong> xảy ra khi một học sinh có thứ hạng <strong>tốt hơn nghiêm ngặt</strong> đến và <strong>thay thế</strong> lựa chọn hiện tại.</p>

<p>Trả về tổng số lần thay thế.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">ranks = [4,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Học sinh đầu tiên có <code>ranks[0] = 4</code> được chọn ban đầu.</li>
    <li>Học sinh thứ hai có <code>ranks[1] = 1</code> có thứ hạng tốt hơn lựa chọn hiện tại, nên xảy ra một lần thay thế.</li>
    <li>Học sinh thứ ba có thứ hạng kém hơn, nên không xảy ra thay thế.</li>
    <li>Vì vậy, số lần thay thế là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">ranks = [2,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Học sinh đầu tiên có <code>ranks[0] = 2</code> được chọn ban đầu.</li>
    <li>Cả <code>ranks[1] = 2</code> và <code>ranks[2] = 3</code> đều không có thứ hạng tốt hơn lựa chọn hiện tại.</li>
    <li>Vì vậy, số lần thay thế là 0.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= ranks.length &lt;= 10<sup>5</sup>​​​​​​​</code></li>
    <li><code>1 &lt;= ranks[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một lần thay thế xảy ra đúng khi xuất hiện một thứ hạng tốt hơn nghiêm ngặt (nhỏ hơn). Ta duy trì thứ hạng hiện tại được chọn $\textit{cur}$, khởi tạo bằng học sinh đầu tiên.
>
> Duyệt từ trái sang phải; nếu $x<\textit{cur}$, cập nhật $\textit{cur}$ và tăng đáp án. Vì $n\le 10^5$, một lượt duyệt là đủ; không cần lưu lịch sử các thứ hạng.

<!-- thinking:end -->

Ta dùng biến $\text{cur}$ để lưu thứ hạng của học sinh đang được chọn. Duyệt qua mảng $\text{ranks}$, nếu gặp học sinh có thứ hạng tốt hơn (tức là $\text{ranks}[i] < \text{cur}$), ta cập nhật $\text{cur}$ và tăng đáp án lên một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số học sinh. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def totalReplacements(self, ranks: List[int]) -> int:
        ans, cur = 0, ranks[0]
        for x in ranks:
            if x < cur:
                cur = x
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int totalReplacements(int[] ranks) {
        int ans = 0;
        int cur = ranks[0];
        for (int x : ranks) {
            if (x < cur) {
                cur = x;
                ++ans;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int totalReplacements(vector<int>& ranks) {
        int ans = 0;
        int cur = ranks[0];
        for (int x : ranks) {
            if (x < cur) {
                cur = x;
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func totalReplacements(ranks []int) (ans int) {
    cur := ranks[0]
    for _, x := range ranks {
        if x < cur {
            cur = x
            ans++
        }
    }
    return
}
```

#### TypeScript

```ts
function totalReplacements(ranks: number[]): number {
    let [ans, cur] = [0, ranks[0]];
    for (const x of ranks) {
        if (x < cur) {
            cur = x;
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
