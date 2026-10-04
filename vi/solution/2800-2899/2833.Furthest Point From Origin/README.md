---
comments: true
difficulty: Easy
rating: 1294
source: Weekly Contest 360 Q1
tags:
    - String
    - Counting
---

<!-- problem:start -->

# [2833. Furthest Point From Origin](https://leetcode.com/problems/furthest-point-from-origin)

[中文文档](/solution/2800-2899/2833.Furthest%20Point%20From%20Origin/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>moves</code> có độ dài <code>n</code>, chỉ gồm các ký tự <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code> và <code>&#39;_&#39;</code>. Chuỗi biểu diễn chuyển động của bạn trên trục số, bắt đầu từ gốc tọa độ <code>0</code>.</p>

<p>Ở bước di chuyển thứ <code>i<sup>th</sup></code>, bạn có thể chọn một trong các hướng sau:</p>

<ul>
	<li>di chuyển sang trái nếu <code>moves[i] = &#39;L&#39;</code> hoặc <code>moves[i] = &#39;_&#39;</code></li>
	<li>di chuyển sang phải nếu <code>moves[i] = &#39;R&#39;</code> hoặc <code>moves[i] = &#39;_&#39;</code></li>
</ul>

<p>Trả về <em><strong>khoảng cách đến gốc tọa độ</strong> của <strong>điểm xa nhất</strong> mà bạn có thể đạt được sau </em><code>n</code><em> lần di chuyển</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> moves = &quot;L_RL__R&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Điểm xa nhất có thể đạt được từ gốc tọa độ 0 là điểm -3, thông qua chuỗi di chuyển &quot;LLRLLLR&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> moves = &quot;_R__LL_&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Điểm xa nhất có thể đạt được từ gốc tọa độ 0 là điểm -5, thông qua chuỗi di chuyển &quot;LRLLLLL&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> moves = &quot;_______&quot;
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Điểm xa nhất có thể đạt được từ gốc tọa độ 0 là điểm 7, thông qua chuỗi di chuyển &quot;RRRRRRR&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= moves.length == n &lt;= 50</code></li>
	<li><code>moves</code> chỉ gồm các ký tự <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code> và <code>&#39;_&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi dấu gạch dưới có thể đi theo một trong hai hướng, và để đạt điểm xa nhất, ta cho mọi ô trống đi về phía đang lớn hơn. Vì vậy, khoảng cách bằng $|\#L-\#R|$ cộng với số dấu gạch dưới.

<!-- thinking:end -->

Khi gặp ký tự '_', ta có thể chọn di chuyển sang trái hoặc phải. Bài toán yêu cầu tìm điểm xa nhất so với gốc tọa độ. Do đó, ban đầu ta có thể duyệt chuỗi một lần, tham lam cho tất cả '_' di chuyển sang trái, rồi tìm điểm xa nhất so với gốc tọa độ tại thời điểm này. Sau đó duyệt lần nữa, tham lam cho tất cả '\_' di chuyển sang phải, rồi tìm điểm xa nhất so với gốc tọa độ tại thời điểm này. Cuối cùng, lấy giá trị lớn hơn trong hai lần duyệt.

Hơn nữa, ta chỉ cần tính hiệu giữa số lượng 'L' và 'R' trong chuỗi, sau đó cộng thêm số lượng '\_'.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def furthestDistanceFromOrigin(self, moves: str) -> int:
        return abs(moves.count("L") - moves.count("R")) + moves.count("_")
```

#### Java

```java
class Solution {
    public int furthestDistanceFromOrigin(String moves) {
        return Math.abs(count(moves, 'L') - count(moves, 'R')) + count(moves, '_');
    }

    private int count(String s, char c) {
        int cnt = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == c) {
                ++cnt;
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
    int furthestDistanceFromOrigin(string moves) {
        auto cnt = [&](char c) {
            return count(moves.begin(), moves.end(), c);
        };
        return abs(cnt('L') - cnt('R')) + cnt('_');
    }
};
```

#### Go

```go
func furthestDistanceFromOrigin(moves string) int {
	count := func(c string) int { return strings.Count(moves, c) }
	return abs(count("L")-count("R")) + count("_")
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
function furthestDistanceFromOrigin(moves: string): number {
    const count = (c: string) => moves.split('').filter(x => x === c).length;
    return Math.abs(count('L') - count('R')) + count('_');
}
```

#### Rust

```rust
impl Solution {
    pub fn furthest_distance_from_origin(moves: String) -> i32 {
        let l = moves.chars().filter(|&c| c == 'L').count() as i32;
        let r = moves.chars().filter(|&c| c == 'R').count() as i32;
        let blank = moves.chars().filter(|&c| c == '_').count() as i32;
        (l - r).abs() + blank
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
