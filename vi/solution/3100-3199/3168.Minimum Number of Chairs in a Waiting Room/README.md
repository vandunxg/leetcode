---
comments: true
difficulty: Easy
rating: 1211
source: Weekly Contest 400 Q1
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [3168. Minimum Number of Chairs in a Waiting Room](https://leetcode.com/problems/minimum-number-of-chairs-in-a-waiting-room)

[Tài liệu tiếng Trung](/solution/3100-3199/3168.Minimum%20Number%20of%20Chairs%20in%20a%20Waiting%20Room/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>. Hãy mô phỏng các sự kiện diễn ra ở mỗi giây <code>i</code>:</p>

<ul>
    <li>Nếu <code>s[i] == &#39;E&#39;</code>, một người bước vào phòng chờ và ngồi vào một trong các ghế ở đó.</li>
    <li>Nếu <code>s[i] == &#39;L&#39;</code>, một người rời khỏi phòng chờ và giải phóng một chiếc ghế.</li>
</ul>

<p>Trả về <strong>số </strong>ghế tối thiểu cần có để luôn có ghế cho mỗi người bước vào phòng chờ, biết rằng ban đầu phòng chờ <strong>trống</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;EEEEEEE&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau mỗi giây, một người bước vào phòng chờ và không có ai rời đi. Vì vậy, cần ít nhất 7 chiếc ghế.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;ELELEEL&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Giả sử phòng chờ có 2 chiếc ghế. Bảng dưới đây thể hiện trạng thái của phòng chờ ở mỗi giây.</p>
</div>

<table>
    <tbody>
        <tr>
            <th>Giây</th>
            <th>Sự kiện</th>
            <th>Số người trong phòng chờ</th>
            <th>Số ghế còn trống</th>
        </tr>
        <tr>
            <td>0</td>
            <td>Vào</td>
            <td>1</td>
            <td>1</td>
        </tr>
        <tr>
            <td>1</td>
            <td>Rời đi</td>
            <td>0</td>
            <td>2</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Vào</td>
            <td>1</td>
            <td>1</td>
        </tr>
        <tr>
            <td>3</td>
            <td>Rời đi</td>
            <td>0</td>
            <td>2</td>
        </tr>
        <tr>
            <td>4</td>
            <td>Vào</td>
            <td>1</td>
            <td>1</td>
        </tr>
        <tr>
            <td>5</td>
            <td>Vào</td>
            <td>2</td>
            <td>0</td>
        </tr>
        <tr>
            <td>6</td>
            <td>Rời đi</td>
            <td>1</td>
            <td>1</td>
        </tr>
    </tbody>
</table>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;ELEELEELLL&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Giả sử phòng chờ có 3 chiếc ghế. Bảng dưới đây thể hiện trạng thái của phòng chờ ở mỗi giây.</p>
</div>

<table>
    <tbody>
        <tr>
            <th>Giây</th>
            <th>Sự kiện</th>
            <th>Số người trong phòng chờ</th>
            <th>Số ghế còn trống</th>
        </tr>
        <tr>
            <td>0</td>
            <td>Vào</td>
            <td>1</td>
            <td>2</td>
        </tr>
        <tr>
            <td>1</td>
            <td>Rời đi</td>
            <td>0</td>
            <td>3</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Vào</td>
            <td>1</td>
            <td>2</td>
        </tr>
        <tr>
            <td>3</td>
            <td>Vào</td>
            <td>2</td>
            <td>1</td>
        </tr>
        <tr>
            <td>4</td>
            <td>Rời đi</td>
            <td>1</td>
            <td>2</td>
        </tr>
        <tr>
            <td>5</td>
            <td>Vào</td>
            <td>2</td>
            <td>1</td>
        </tr>
        <tr>
            <td>6</td>
            <td>Vào</td>
            <td>3</td>
            <td>0</td>
        </tr>
        <tr>
            <td>7</td>
            <td>Rời đi</td>
            <td>2</td>
            <td>1</td>
        </tr>
        <tr>
            <td>8</td>
            <td>Rời đi</td>
            <td>1</td>
            <td>2</td>
        </tr>
        <tr>
            <td>9</td>
            <td>Rời đi</td>
            <td>0</td>
            <td>3</td>
        </tr>
    </tbody>
</table>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 50</code></li>
    <li><code>s</code> chỉ chứa các chữ cái <code>&#39;E&#39;</code> và <code>&#39;L&#39;</code>.</li>
    <li><code>s</code> biểu diễn một chuỗi các lượt vào và rời đi hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> `E` biểu thị một người đến, còn `L` biểu thị một người rời đi; chỉ mua thêm ghế khi không còn ghế trống và không bao giờ bỏ bớt ghế. Đáp án là số người tối đa có mặt đồng thời.
>
> Ghế trống được dùng lại ngay: người đến sẽ ngồi vào ghế trống nếu có; nếu không, $cnt$ tăng; người rời đi làm số ghế trống tăng.
>
> Theo dõi $cnt$ và $left$ khi duyệt $s$. Sau cùng, $cnt$ là số ghế tối thiểu.

<!-- thinking:end -->

Ta dùng biến `cnt` để ghi nhận tổng số ghế cần có hiện tại, và biến `left` để ghi nhận số ghế trống hiện tại. Ta duyệt chuỗi `s`. Nếu ký tự hiện tại là 'E', khi còn ghế trống thì dùng trực tiếp một ghế, nếu không thì thêm một ghế; nếu ký tự hiện tại là 'L', số ghế trống tăng thêm một.

Sau khi duyệt xong, ta trả về `cnt`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi `s`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumChairs(self, s: str) -> int:
        cnt = left = 0
        for c in s:
            if c == "E":
                if left:
                    left -= 1
                else:
                    cnt += 1
            else:
                left += 1
        return cnt
```

#### Java

```java
class Solution {
    public int minimumChairs(String s) {
        int cnt = 0, left = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == 'E') {
                if (left > 0) {
                    --left;
                } else {
                    ++cnt;
                }
            } else {
                ++left;
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumChairs(string s) {
        int cnt = 0, left = 0;
        for (char& c : s) {
            if (c == 'E') {
                if (left > 0) {
                    --left;
                } else {
                    ++cnt;
                }
            } else {
                ++left;
            }
        }
        return cnt;
    }
};
```

#### Go

```go
func minimumChairs(s string) int {
    cnt, left := 0, 0
    for _, c := range s {
        if c == 'E' {
            if left > 0 {
                left--
            } else {
                cnt++
            }
        } else {
            left++
        }
    }
    return cnt
}
```

#### TypeScript

```ts
function minimumChairs(s: string): number {
    let [cnt, left] = [0, 0];
    for (const c of s) {
        if (c === 'E') {
            if (left > 0) {
                --left;
            } else {
                ++cnt;
            }
        } else {
            ++left;
        }
    }
    return cnt;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
