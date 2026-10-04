---
comments: true
difficulty: Medium
rating: 1460
source: Weekly Contest 397 Q2
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [3147. Taking Maximum Energy From the Mystic Dungeon](https://leetcode.com/problems/taking-maximum-energy-from-the-mystic-dungeon)

[中文文档](/solution/3100-3199/3147.Taking%20Maximum%20Energy%20From%20the%20Mystic%20Dungeon/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một hầm ngục huyền bí, có <code>n</code> pháp sư đứng thành một hàng. Mỗi pháp sư có một thuộc tính cung cấp năng lượng cho bạn. Một số pháp sư có thể cung cấp năng lượng âm, nghĩa là họ lấy năng lượng của bạn.</p>

<p>Bạn bị nguyền rằng sau khi hấp thụ năng lượng từ pháp sư <code>i</code>, bạn sẽ ngay lập tức được dịch chuyển đến pháp sư <code>(i + k)</code>. Quá trình này sẽ lặp lại cho đến khi bạn đến pháp sư mà <code>(i + k)</code> không tồn tại.</p>

<p>Nói cách khác, bạn sẽ chọn một điểm bắt đầu rồi dịch chuyển với các bước nhảy <code>k</code> cho đến khi đến cuối dãy pháp sư, <strong>hấp thụ toàn bộ năng lượng</strong> trong hành trình.</p>

<p>Cho một mảng <code>energy</code> và một số nguyên <code>k</code>. Hãy trả về lượng năng lượng <strong>lớn nhất</strong> có thể nhận được.</p>

<p><strong>Lưu ý</strong> rằng khi đến một pháp sư, bạn <em>bắt buộc</em> phải lấy năng lượng từ họ, bất kể đó là năng lượng âm hay dương.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
"> energy = [5,2,-10,-5,1], k = 3</span></p>

<p><strong>Đầu ra:</strong><span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
"> 3</span></p>

<p><strong>Giải thích:</strong> Ta có thể nhận tổng cộng 3 đơn vị năng lượng bằng cách bắt đầu từ pháp sư 1 và hấp thụ 2 + 1 = 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong><span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
"> energy = [-2,-3,-1], k = 2</span></p>

<p><strong>Đầu ra:</strong><span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
"> -1</span></p>

<p><strong>Giải thích:</strong> Ta có thể nhận tổng cộng -1 đơn vị năng lượng bằng cách bắt đầu từ pháp sư 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= energy.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-1000 &lt;= energy[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= energy.length - 1</code></li>
</ul>

<p>&nbsp;</p>
​​​​​​

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Một hành trình có thể bắt đầu ở bất kỳ đâu và nhảy từng bước $k$, cộng dồn các giá trị năng lượng có thể âm. Cộng dồn xuôi từ mọi điểm bắt đầu vẫn tuyến tính, nhưng việc tìm phần tiếp tục tốt nhất sẽ dễ hơn nếu bắt đầu từ cuối.
>
> Các lớp dư không trộn lẫn với nhau. Khi đi ngược từ cuối một lớp, tổng đang tính chính là điểm bắt đầu tốt nhất trong lớp đó.
>
> Liệt kê các điểm kết thúc trong $[n-k,n)$ và thực hiện $j-=k$, đồng thời cập nhật giá trị lớn nhất toàn cục. Mỗi chỉ số chỉ được duyệt một lần.

<!-- thinking:end -->

Ta có thể liệt kê các điểm kết thúc trong khoảng $[n - k, n)$, sau đó duyệt ngược từ mỗi điểm kết thúc, cộng dồn các giá trị năng lượng của các pháp sư cách nhau $k$ vị trí và cập nhật đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{energy}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumEnergy(self, energy: List[int], k: int) -> int:
        ans = -inf
        n = len(energy)
        for i in range(n - k, n):
            j, s = i, 0
            while j >= 0:
                s += energy[j]
                ans = max(ans, s)
                j -= k
        return ans
```

#### Java

```java
class Solution {
    public int maximumEnergy(int[] energy, int k) {
        int ans = -(1 << 30);
        int n = energy.length;
        for (int i = n - k; i < n; ++i) {
            for (int j = i, s = 0; j >= 0; j -= k) {
                s += energy[j];
                ans = Math.max(ans, s);
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
    int maximumEnergy(vector<int>& energy, int k) {
        int ans = -(1 << 30);
        int n = energy.size();
        for (int i = n - k; i < n; ++i) {
            for (int j = i, s = 0; j >= 0; j -= k) {
                s += energy[j];
                ans = max(ans, s);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumEnergy(energy []int, k int) int {
	ans := -(1 << 30)
	n := len(energy)
	for i := n - k; i < n; i++ {
		for j, s := i, 0; j >= 0; j -= k {
			s += energy[j]
			ans = max(ans, s)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maximumEnergy(energy: number[], k: number): number {
    const n = energy.length;
    let ans = -Infinity;
    for (let i = n - k; i < n; ++i) {
        for (let j = i, s = 0; j >= 0; j -= k) {
            s += energy[j];
            ans = Math.max(ans, s);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_energy(energy: Vec<i32>, k: i32) -> i32 {
        let n = energy.len();
        let mut ans = i32::MIN;
        for i in n - k as usize..n {
            let mut s = 0;
            let mut j = i as i32;
            while j >= 0 {
                s += energy[j as usize];
                ans = ans.max(s);
                j -= k;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
