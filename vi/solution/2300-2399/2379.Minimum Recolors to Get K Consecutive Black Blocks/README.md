---
comments: true
difficulty: Easy
rating: 1360
source: Biweekly Contest 85 Q1
tags:
    - String
    - Sliding Window
---

<!-- problem:start -->

# [2379. Minimum Recolors to Get K Consecutive Black Blocks](https://leetcode.com/problems/minimum-recolors-to-get-k-consecutive-black-blocks)

[中文文档](/solution/2300-2399/2379.Minimum%20Recolors%20to%20Get%20K%20Consecutive%20Black%20Blocks/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <strong>0-indexed</strong> <code>blocks</code> có độ dài <code>n</code>, trong đó <code>blocks[i]</code> là <code>&#39;W&#39;</code> hoặc <code>&#39;B&#39;</code>, biểu thị màu của khối thứ <code>i<sup>th</sup></code>. Các ký tự <code>&#39;W&#39;</code> và <code>&#39;B&#39;</code> lần lượt biểu thị màu trắng và đen.</p>

<p>Bạn cũng được cho một số nguyên <code>k</code>, là số lượng khối đen <strong>liên tiếp</strong> mong muốn.</p>

<p>Trong một thao tác, bạn có thể <strong>đổi màu</strong> một khối trắng thành khối đen.</p>

<p>Trả về số thao tác <strong>tối thiểu</strong> cần thực hiện để có ít nhất <em><strong>một</strong> đoạn gồm </em><code>k</code><em> khối đen liên tiếp.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> blocks = &quot;WBBWWBBWBW&quot;, k = 7
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Một cách để có 7 khối đen liên tiếp là đổi màu các khối ở vị trí 0, 3 và 4
để blocks = &quot;BBBBBBBWBW&quot;.
Có thể chứng minh rằng không thể có 7 khối đen liên tiếp với ít hơn 3 thao tác.
Do đó, ta trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> blocks = &quot;WBWBBBW&quot;, k = 2
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không cần thay đổi gì, vì đã có sẵn 2 khối đen liên tiếp.
Do đó, ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == blocks.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>blocks[i]</code> là <code>&#39;W&#39;</code> hoặc <code>&#39;B&#39;</code>.</li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Tô đen toàn bộ một cửa sổ có độ dài $k$ cần số thao tác bằng số ô trắng trong cửa sổ. Ta chỉ cần xét mọi cửa sổ, không cần tìm kiếm trên các cách tô màu.
>
> Đếm số ô trắng trong $k$ ô đầu tiên, sau đó trượt cửa sổ, thêm và loại bỏ các đầu mút, đồng thời giữ lại giá trị nhỏ nhất.

<!-- thinking:end -->

Ta nhận thấy rằng yêu cầu thực sự của bài toán là tìm số khối trắng nhỏ nhất trong một cửa sổ trượt có kích thước $k$.

Do đó, chỉ cần duyệt chuỗi $blocks$, dùng một biến $cnt$ để đếm số khối trắng trong cửa sổ hiện tại, rồi dùng một biến $ans$ để lưu giá trị nhỏ nhất.

Sau khi duyệt xong, ta sẽ nhận được đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $blocks$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumRecolors(self, blocks: str, k: int) -> int:
        ans = cnt = blocks[:k].count('W')
        for i in range(k, len(blocks)):
            cnt += blocks[i] == 'W'
            cnt -= blocks[i - k] == 'W'
            ans = min(ans, cnt)
        return ans
```

#### Java

```java
class Solution {
    public int minimumRecolors(String blocks, int k) {
        int cnt = 0;
        for (int i = 0; i < k; ++i) {
            cnt += blocks.charAt(i) == 'W' ? 1 : 0;
        }
        int ans = cnt;
        for (int i = k; i < blocks.length(); ++i) {
            cnt += blocks.charAt(i) == 'W' ? 1 : 0;
            cnt -= blocks.charAt(i - k) == 'W' ? 1 : 0;
            ans = Math.min(ans, cnt);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumRecolors(string blocks, int k) {
        int cnt = count(blocks.begin(), blocks.begin() + k, 'W');
        int ans = cnt;
        for (int i = k; i < blocks.size(); ++i) {
            cnt += blocks[i] == 'W';
            cnt -= blocks[i - k] == 'W';
            ans = min(ans, cnt);
        }
        return ans;
    }
};
```

#### Go

```go
func minimumRecolors(blocks string, k int) int {
	cnt := strings.Count(blocks[:k], "W")
	ans := cnt
	for i := k; i < len(blocks); i++ {
		if blocks[i] == 'W' {
			cnt++
		}
		if blocks[i-k] == 'W' {
			cnt--
		}
		if ans > cnt {
			ans = cnt
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minimumRecolors(blocks: string, k: number): number {
    let cnt = 0;
    for (let i = 0; i < k; ++i) {
        cnt += blocks[i] === 'W' ? 1 : 0;
    }
    let ans = cnt;
    for (let i = k; i < blocks.length; ++i) {
        cnt += blocks[i] === 'W' ? 1 : 0;
        cnt -= blocks[i - k] === 'W' ? 1 : 0;
        ans = Math.min(ans, cnt);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_recolors(blocks: String, k: i32) -> i32 {
        let k = k as usize;
        let s = blocks.as_bytes();
        let n = s.len();
        let mut count = 0;
        for i in 0..k {
            if s[i] == b'B' {
                count += 1;
            }
        }
        let mut ans = k - count;
        for i in k..n {
            if s[i - k] == b'B' {
                count -= 1;
            }
            if s[i] == b'B' {
                count += 1;
            }
            ans = ans.min(k - count);
        }
        ans as i32
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $blocks
     * @param Integer $k
     * @return Integer
     */
    function minimumRecolors($blocks, $k) {
        $cnt = 0;
        for ($i = 0; $i < $k; $i++) {
            if ($blocks[$i] === 'W') {
                $cnt++;
            }
        }
        $min = $cnt;
        for ($i = $k; $i < strlen($blocks); $i++) {
            if ($blocks[$i] === 'W') {
                $cnt++;
            }
            if ($blocks[$i - $k] === 'W') {
                $cnt--;
            }
            $min = min($min, $cnt);
        }
        return $min;
    }
}
```

#### C

```c
#define min(a, b) (((a) < (b)) ? (a) : (b))

int minimumRecolors(char* blocks, int k) {
    int n = strlen(blocks);
    int count = 0;
    for (int i = 0; i < k; i++) {
        count += blocks[i] == 'B';
    }
    int ans = k - count;
    for (int i = k; i < n; i++) {
        count -= blocks[i - k] == 'B';
        count += blocks[i] == 'B';
        ans = min(ans, k - count);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
