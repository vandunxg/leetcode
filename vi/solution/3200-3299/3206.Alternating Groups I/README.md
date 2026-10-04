---
comments: true
difficulty: Easy
rating: 1223
source: Biweekly Contest 134 Q1
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [3206. Alternating Groups I](https://leetcode.com/problems/alternating-groups-i)

[中文文档](/solution/3200-3299/3206.Alternating%20Groups%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Có một vòng tròn gồm các ô màu đỏ và xanh dương. Bạn được cho một mảng số nguyên <code>colors</code>. Màu của ô <code>i</code> được biểu diễn bởi <code>colors[i]</code>:</p>

<ul>
    <li><code>colors[i] == 0</code> nghĩa là ô <code>i</code> có màu <strong>đỏ</strong>.</li>
    <li><code>colors[i] == 1</code> nghĩa là ô <code>i</code> có màu <strong>xanh dương</strong>.</li>
</ul>

<p>Mỗi 3 ô liên tiếp trong vòng tròn có màu <strong>xen kẽ</strong> (ô ở giữa có màu khác với ô bên <strong>trái</strong> và bên <strong>phải</strong>) được gọi là một nhóm <strong>xen kẽ</strong>.</p>

<p>Trả về số lượng nhóm <strong>xen kẽ</strong>.</p>

<p><strong>Lưu ý</strong> rằng vì <code>colors</code> biểu diễn một <strong>vòng tròn</strong>, ô <strong>đầu tiên</strong> và ô <strong>cuối cùng</strong> được xem là nằm cạnh nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">colors = [1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3206.Alternating%20Groups%20I/images/image_2024-05-16_23-53-171.png" style="width: 150px; height: 150px; padding: 10px; background: #fff; border-radius: .5rem;" /></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">colors = [0,1,0,0,1]</span></p>

<p><strong>Đầu ra:</strong> 3</p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3206.Alternating%20Groups%20I/images/image_2024-05-16_23-47-491.png" style="width: 150px; height: 150px; padding: 10px; background: #fff; border-radius: .5rem;" /></p>

<p>Các nhóm xen kẽ:</p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3206.Alternating%20Groups%20I/images/image_2024-05-16_23-50-441.png" style="width: 150px; height: 150px; padding: 10px; background: #fff; border-radius: .5rem;" /></strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3206.Alternating%20Groups%20I/images/image_2024-05-16_23-48-211.png" style="width: 150px; height: 150px; padding: 10px; background: #fff; border-radius: .5rem;" /><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3200-3299/3206.Alternating%20Groups%20I/images/image_2024-05-16_23-49-351.png" style="width: 150px; height: 150px; padding: 10px; background: #fff; border-radius: .5rem;" /></strong></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>3 &lt;= colors.length &lt;= 100</code></li>
    <li><code>0 &lt;= colors[i] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 100$ và độ dài nhóm luôn cố định là $3$, ta có thể kiểm tra từng vị trí bắt đầu để tìm một bộ ba xen kẽ. Tuy nhiên, việc xử lý phần nối vòng bằng các chỉ số modulo khá rườm rà.
>
> Trải vòng tròn thành một dãy có độ dài $2n$ rồi duyệt qua dãy đó, trong đó $\textit{cnt}$ theo dõi độ dài đoạn xen kẽ hiện tại và được đặt lại khi hai ô kề nhau giống màu. Chỉ đếm khi $i\ge n$ và $\textit{cnt}\ge 3$, để mỗi chỉ số ban đầu được đếm đúng một lần khi là điểm kết thúc bên phải của một nhóm, với $O(1)$ bộ nhớ bổ sung.

<!-- thinking:end -->

Ta đặt $k = 3$, biểu thị độ dài của nhóm xen kẽ là $3$.

Để thuận tiện, ta có thể trải vòng tròn thành một mảng có độ dài $2n$, sau đó duyệt mảng này từ trái sang phải. Ta sử dụng biến $\textit{cnt}$ để ghi lại độ dài hiện tại của nhóm xen kẽ. Nếu gặp hai màu giống nhau, ta đặt lại $\textit{cnt}$ về $1$; nếu không, ta tăng $\textit{cnt}$. Nếu $\textit{cnt} \ge k$ và vị trí hiện tại $i$ lớn hơn hoặc bằng $n$, thì ta đã tìm thấy một nhóm xen kẽ và tăng đáp án lên một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{colors}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfAlternatingGroups(self, colors: List[int]) -> int:
        k = 3
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
    public int numberOfAlternatingGroups(int[] colors) {
        int k = 3;
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
    int numberOfAlternatingGroups(vector<int>& colors) {
        int k = 3;
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
func numberOfAlternatingGroups(colors []int) (ans int) {
    k := 3
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
function numberOfAlternatingGroups(colors: number[]): number {
    const k = 3;
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
