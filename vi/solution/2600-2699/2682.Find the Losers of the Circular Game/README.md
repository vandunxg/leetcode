---
comments: true
difficulty: Easy
rating: 1382
source: Weekly Contest 345 Q1
tags:
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [2682. Find the Losers of the Circular Game](https://leetcode.com/problems/find-the-losers-of-the-circular-game)

[中文文档](/solution/2600-2699/2682.Find%20the%20Losers%20of%20the%20Circular%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> người bạn đang chơi một trò chơi. Họ ngồi thành một vòng tròn và được đánh số từ <code>1</code> đến <code>n</code> theo <strong>thứ tự chiều kim đồng hồ</strong>. Cụ thể hơn, di chuyển theo chiều kim đồng hồ từ người bạn thứ <code>i<sup>th</sup></code> sẽ đưa bạn đến người bạn thứ <code>(i+1)<sup>th</sup></code> với <code>1 &lt;= i &lt; n</code>, còn di chuyển theo chiều kim đồng hồ từ người bạn thứ <code>n<sup>th</sup></code> sẽ đưa bạn đến người bạn thứ <code>1<sup>st</sup></code>.</p>

<p>Luật chơi như sau:</p>

<p>Người bạn thứ <code>1<sup>st</sup></code> nhận quả bóng.</p>

<ul>
	<li>Sau đó, người bạn thứ <code>1<sup>st</sup></code> chuyền bóng cho người bạn cách họ <code>k</code> bước theo hướng <strong>chiều kim đồng hồ</strong>.</li>
	<li>Sau đó, người bạn nhận bóng chuyền bóng cho người bạn cách họ <code>2 * k</code> bước theo hướng <strong>chiều kim đồng hồ</strong>.</li>
	<li>Sau đó, người bạn nhận bóng chuyền bóng cho người bạn cách họ <code>3 * k</code> bước theo hướng <strong>chiều kim đồng hồ</strong>, và cứ tiếp tục như vậy.</li>
</ul>

<p>Nói cách khác, ở lượt thứ <code>i<sup>th</sup></code>, người bạn đang giữ bóng phải chuyền bóng cho người bạn cách họ <code>i * k</code> bước theo hướng <strong>chiều kim đồng hồ</strong>.</p>

<p>Trò chơi kết thúc khi có người bạn nhận bóng lần thứ hai.</p>

<p><strong>Những người thua cuộc</strong> là những người bạn không nhận bóng trong suốt trò chơi.</p>

<p>Cho số lượng người bạn là <code>n</code> và một số nguyên <code>k</code>, hãy trả về <em>mảng answer chứa những người thua cuộc theo thứ tự <strong>tăng dần</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, k = 2
<strong>Đầu ra:</strong> [4,5]
<strong>Giải thích:</strong> Trò chơi diễn ra như sau:
1) Bắt đầu từ người bạn thứ 1<sup>st</sup> và chuyền bóng cho người bạn cách 2 bước - người bạn thứ 3<sup>rd</sup>.
2) Người bạn thứ 3<sup>rd</sup> chuyền bóng cho người bạn cách 4 bước - người bạn thứ 2<sup>nd</sup>.
3) Người bạn thứ 2<sup>nd</sup> chuyền bóng cho người bạn cách 6 bước - người bạn thứ 3<sup>rd</sup>.
4) Trò chơi kết thúc vì người bạn thứ 3<sup>rd</sup> nhận bóng lần thứ hai.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, k = 4
<strong>Đầu ra:</strong> [2,3,4]
<strong>Giải thích:</strong> Trò chơi diễn ra như sau:
1) Bắt đầu từ người bạn thứ 1<sup>st</sup> và chuyền bóng cho người bạn cách 4 bước - người bạn thứ 1<sup>st</sup>.
2) Trò chơi kết thúc vì người bạn thứ 1<sup>st</sup> nhận bóng lần thứ hai.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= n &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Quả bóng di chuyển $p k$ bước trên một vòng tròn cho đến khi có người nhận bóng hai lần. Vì $n \le 50$, ta có thể mô phỏng trực tiếp. Mảng đánh dấu người đã nhận bóng giúp xác định những chỉ số chưa được đánh dấu sau vòng lặp, đó chính là những người thua cuộc, sau khi chuyển về số thứ tự bắt đầu từ $1$.

<!-- thinking:end -->

Ta dùng một mảng `vis` để ghi nhận mỗi người bạn đã nhận bóng hay chưa; ban đầu, tất cả người bạn đều chưa nhận bóng. Sau đó, ta mô phỏng quá trình chơi theo các quy tắc đã nêu trong đề bài cho đến khi có người nhận bóng lần thứ hai.

Trong quá trình mô phỏng, ta dùng hai biến $i$ và $p$ lần lượt biểu diễn người bạn đang giữ bóng và độ dài bước chuyền hiện tại. Ban đầu, $i=0, p=1$, nghĩa là người bạn đầu tiên nhận bóng. Mỗi lần chuyền bóng, ta cập nhật $i$ thành $(i+p \times k) \bmod n$, biểu diễn số thứ tự của người bạn tiếp theo nhận bóng, rồi cập nhật $p$ thành $p+1$, biểu diễn độ dài bước chuyền cho lần chuyền tiếp theo. Trò chơi kết thúc khi một người bạn nhận bóng lần thứ hai.

Cuối cùng, ta duyệt qua mảng `vis` và thêm số thứ tự của những người bạn chưa nhận bóng vào mảng đáp án.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng người bạn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def circularGameLosers(self, n: int, k: int) -> List[int]:
        vis = [False] * n
        i, p = 0, 1
        while not vis[i]:
            vis[i] = True
            i = (i + p * k) % n
            p += 1
        return [i + 1 for i in range(n) if not vis[i]]
```

#### Java

```java
class Solution {
    public int[] circularGameLosers(int n, int k) {
        boolean[] vis = new boolean[n];
        int cnt = 0;
        for (int i = 0, p = 1; !vis[i]; ++p) {
            vis[i] = true;
            ++cnt;
            i = (i + p * k) % n;
        }
        int[] ans = new int[n - cnt];
        for (int i = 0, j = 0; i < n; ++i) {
            if (!vis[i]) {
                ans[j++] = i + 1;
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
    vector<int> circularGameLosers(int n, int k) {
        bool vis[n];
        memset(vis, false, sizeof(vis));
        for (int i = 0, p = 1; !vis[i]; ++p) {
            vis[i] = true;
            i = (i + p * k) % n;
        }
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            if (!vis[i]) {
                ans.push_back(i + 1);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func circularGameLosers(n int, k int) (ans []int) {
	vis := make([]bool, n)
	for i, p := 0, 1; !vis[i]; p++ {
		vis[i] = true
		i = (i + p*k) % n
	}
	for i, x := range vis {
		if !x {
			ans = append(ans, i+1)
		}
	}
	return
}
```

#### TypeScript

```ts
function circularGameLosers(n: number, k: number): number[] {
    const vis = new Array(n).fill(false);
    const ans: number[] = [];
    for (let i = 0, p = 1; !vis[i]; p++) {
        vis[i] = true;
        i = (i + p * k) % n;
    }
    for (let i = 0; i < vis.length; i++) {
        if (!vis[i]) {
            ans.push(i + 1);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn circular_game_losers(n: i32, k: i32) -> Vec<i32> {
        let mut vis: Vec<bool> = vec![false; n as usize];

        let mut i = 0;
        let mut p = 1;
        while !vis[i] {
            vis[i] = true;
            i = (i + p * (k as usize)) % (n as usize);
            p += 1;
        }

        let mut ans = Vec::new();
        for i in 0..vis.len() {
            if !vis[i] {
                ans.push((i + 1) as i32);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
