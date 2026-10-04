---
comments: true
difficulty: Medium
rating: 1721
source: Biweekly Contest 134 Q3
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [3208. Alternating Groups II](https://leetcode.com/problems/alternating-groups-ii)

[中文文档](/solution/3200-3299/3208.Alternating%20Groups%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có một vòng tròn gồm các ô màu đỏ và xanh dương. Cho một mảng số nguyên <code>colors</code> và một số nguyên <code>k</code>. Màu của ô <code>i</code> được biểu diễn bởi <code>colors[i]</code>:</p>

<ul>
    <li><code>colors[i] == 0</code> nghĩa là ô <code>i</code> có màu <strong>đỏ</strong>.</li>
    <li><code>colors[i] == 1</code> nghĩa là ô <code>i</code> có màu <strong>xanh dương</strong>.</li>
</ul>

<p>Một nhóm <strong>xen kẽ</strong> là mỗi nhóm gồm <code>k</code> ô liên tiếp trong vòng tròn có màu <strong>xen kẽ</strong> (mỗi ô trong nhóm, ngoại trừ ô đầu tiên và ô cuối cùng, có màu khác với ô bên <strong>trái</strong> và bên <strong>phải</strong>).</p>

<p>Hãy trả về số lượng nhóm <strong>xen kẽ</strong>.</p>

<p><strong>Lưu ý</strong> rằng vì <code>colors</code> biểu diễn một <strong>vòng tròn</strong>, ô <strong>đầu tiên</strong> và ô <strong>cuối cùng</strong> được xem là nằm cạnh nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">colors = [0,1,0,1,0], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3208.Alternating%20Groups%20II/images/screenshot-2024-05-28-183519.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /></strong></p>

<p>Các nhóm xen kẽ:</p>

<p><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3208.Alternating%20Groups%20II/images/screenshot-2024-05-28-182448.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3208.Alternating%20Groups%20II/images/screenshot-2024-05-28-182844.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3208.Alternating%20Groups%20II/images/screenshot-2024-05-28-183057.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">colors = [0,1,0,0,1,0,1], k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3208.Alternating%20Groups%20II/images/screenshot-2024-05-28-183907.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /></strong></p>

<p>Các nhóm xen kẽ:</p>

<p><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3208.Alternating%20Groups%20II/images/screenshot-2024-05-28-184128.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3208.Alternating%20Groups%20II/images/screenshot-2024-05-28-184240.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">colors = [1,1,0,1], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" data-darkreader-inline-bgcolor="" data-darkreader-inline-bgimage="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3208.Alternating%20Groups%20II/images/screenshot-2024-05-28-184516.png" style="width: 150px; height: 150px; padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; --darkreader-inline-bgimage: initial; --darkreader-inline-bgcolor: #181a1b;" /></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>3 &lt;= colors.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= colors[i] &lt;= 1</code></li>
    <li><code>3 &lt;= k &lt;= colors.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Độ dài nhóm đã được cho là $k$ và $n\le 10^5$. Kiểm tra $k$ ô tại mỗi vị trí bắt đầu có độ phức tạp $O(nk)$, quá chậm.
>
> Tương tự trường hợp độ dài-$3$, tính xen kẽ là một thuộc tính liên tiếp: trải vòng tròn thành mảng có độ dài $2n$, duy trì đoạn xen kẽ hiện tại $\textit{cnt}$ và đếm khi $i\ge n$ và $\textit{cnt}\ge k$. Mỗi điểm kết thúc chỉ cần $O(1)$, nên việc duyệt có độ phức tạp tuyến tính.

<!-- thinking:end -->

Ta có thể trải vòng tròn thành một mảng có độ dài $2n$, sau đó duyệt mảng này từ trái sang phải. Ta dùng biến $\textit{cnt}$ để ghi lại độ dài hiện tại của nhóm xen kẽ. Nếu gặp hai màu giống nhau, ta đặt lại $\textit{cnt}$ về $1$; ngược lại, ta tăng $\textit{cnt}$ lên. Nếu $\textit{cnt} \ge k$ và vị trí hiện tại $i$ lớn hơn hoặc bằng $n$, ta đã tìm thấy một nhóm xen kẽ và tăng đáp án lên một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{colors}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfAlternatingGroups(self, colors: List[int], k: int) -> int:
        n = len(colors)
        ans = cnt = 0
        for i in range(n << 1):
            if i and colors[i % n] == colors[(i - 1) % n]:
                cnt = 1
            else:
                cnt += 1
            ans += i >= n and cnt >= k
        return ans
```

#### Java

```java
class Solution {
    public int numberOfAlternatingGroups(int[] colors, int k) {
        int n = colors.length;
        int ans = 0, cnt = 0;
        for (int i = 0; i < n << 1; ++i) {
            if (i > 0 && colors[i % n] == colors[(i - 1) % n]) {
                cnt = 1;
            } else {
                ++cnt;
            }
            ans += i >= n && cnt >= k ? 1 : 0;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfAlternatingGroups(vector<int>& colors, int k) {
        int n = colors.size();
        int ans = 0, cnt = 0;
        for (int i = 0; i < n << 1; ++i) {
            if (i && colors[i % n] == colors[(i - 1) % n]) {
                cnt = 1;
            } else {
                ++cnt;
            }
            ans += i >= n && cnt >= k ? 1 : 0;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfAlternatingGroups(colors []int, k int) (ans int) {
    n := len(colors)
    cnt := 0
    for i := 0; i < n<<1; i++ {
        if i > 0 && colors[i%n] == colors[(i-1)%n] {
            cnt = 1
        } else {
            cnt++
        }
        if i >= n && cnt >= k {
            ans++
        }
    }
    return
}
```

#### TypeScript

```ts
function numberOfAlternatingGroups(colors: number[], k: number): number {
    const n = colors.length;
    let [ans, cnt] = [0, 0];
    for (let i = 0; i < n << 1; ++i) {
        if (i && colors[i % n] === colors[(i - 1) % n]) {
            cnt = 1;
        } else {
            ++cnt;
        }
        ans += i >= n && cnt >= k ? 1 : 0;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
