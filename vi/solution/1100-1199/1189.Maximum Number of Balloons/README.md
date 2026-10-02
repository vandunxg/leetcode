---
comments: true
difficulty: Easy
rating: 1181
source: Weekly Contest 154 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [1189. Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons)

[中文文档](/solution/1100-1199/1189.Maximum%20Number%20of%20Balloons/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>text</code>, hãy dùng các ký tự trong <code>text</code> để tạo được nhiều lần xuất hiện của từ <strong>&quot;balloon&quot;</strong> nhất có thể.</p>

<p>Mỗi ký tự trong <code>text</code> chỉ được dùng <strong>tối đa một lần</strong>. Hãy trả về số lần xuất hiện tối đa có thể tạo được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1189.Maximum%20Number%20of%20Balloons/images/1536_ex1_upd.jpg" style="width: 132px; height: 35px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;nlaebolko&quot;
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1189.Maximum%20Number%20of%20Balloons/images/1536_ex2_upd.jpg" style="width: 267px; height: 35px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;loonbalxballpoon&quot;
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;leetcode&quot;
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 10<sup>4</sup></code></li>
	<li><code>text</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống với <a href="https://leetcode.com/problems/rearrange-characters-to-make-target-string/description/" target="_blank"> 2287: Rearrange Characters to Make Target String.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm tần suất

<!-- thinking:start -->

> **Tư duy**
>
> Từ `balloon` cần các ký tự $b,a,n$ mỗi ký tự một lần, và $l,o$ mỗi ký tự hai lần. Sau khi đếm ký tự trong `text`, chia đôi số lượng $l$ và $o$, rồi lấy giá trị nhỏ nhất trong số lượng $b,a,l,o,n$. Không cần xóa ký tự khỏi chuỗi nhiều lần.

<!-- thinking:end -->

Ta đếm tần suất của từng chữ cái trong chuỗi `text`, sau đó chia đôi tần suất của 'o' và 'l' vì từ `balloon` có hai chữ cái 'o' và hai chữ cái 'l'.

Tiếp theo, ta xét từng chữ cái trong từ `balon` và tìm tần suất nhỏ nhất của chúng trong chuỗi `text`. Tần suất nhỏ nhất này chính là số lần tối đa từ `balloon` có thể xuất hiện trong `text`.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài chuỗi `text`, còn $C$ là kích thước của tập ký tự. Ở bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxNumberOfBalloons(self, text: str) -> int:
        cnt = Counter(text)
        cnt['o'] >>= 1
        cnt['l'] >>= 1
        return min(cnt[c] for c in 'balon')
```

#### Java

```java
class Solution {
    public int maxNumberOfBalloons(String text) {
        int[] cnt = new int[26];
        for (int i = 0; i < text.length(); ++i) {
            ++cnt[text.charAt(i) - 'a'];
        }
        cnt['l' - 'a'] >>= 1;
        cnt['o' - 'a'] >>= 1;
        int ans = 1 << 30;
        for (char c : "balon".toCharArray()) {
            ans = Math.min(ans, cnt[c - 'a']);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxNumberOfBalloons(string text) {
        int cnt[26]{};
        for (char c : text) {
            ++cnt[c - 'a'];
        }
        cnt['o' - 'a'] >>= 1;
        cnt['l' - 'a'] >>= 1;
        int ans = 1 << 30;
        string t = "balon";
        for (char c : t) {
            ans = min(ans, cnt[c - 'a']);
        }
        return ans;
    }
};
```

#### Go

```go
func maxNumberOfBalloons(text string) int {
	cnt := [26]int{}
	for _, c := range text {
		cnt[c-'a']++
	}
	cnt['l'-'a'] >>= 1
	cnt['o'-'a'] >>= 1
	ans := 1 << 30
	for _, c := range "balon" {
		if x := cnt[c-'a']; ans > x {
			ans = x
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxNumberOfBalloons(text: string): number {
    const cnt = new Array(26).fill(0);
    for (const c of text) {
        cnt[c.charCodeAt(0) - 97]++;
    }
    return Math.min(cnt[0], cnt[1], cnt[11] >> 1, cnt[14] >> 1, cnt[13]);
}
```

#### Rust

```rust
impl Solution {
    pub fn max_number_of_balloons(text: String) -> i32 {
        let mut arr = [0; 5];
        for c in text.chars() {
            match c {
                'b' => {
                    arr[0] += 1;
                }
                'a' => {
                    arr[1] += 1;
                }
                'l' => {
                    arr[2] += 1;
                }
                'o' => {
                    arr[3] += 1;
                }
                'n' => {
                    arr[4] += 1;
                }
                _ => {}
            }
        }
        arr[2] /= 2;
        arr[3] /= 2;
        let mut res = i32::MAX;
        for num in arr {
            res = res.min(num);
        }
        res
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $text
     * @return Integer
     */
    function maxNumberOfBalloons($text) {
        $cnt1 = $cnt2 = $cnt3 = $cnt4 = $cnt5 = 0;
        for ($i = 0; $i < strlen($text); $i++) {
            if ($text[$i] == 'b') {
                $cnt1 += 1;
            } elseif ($text[$i] == 'a') {
                $cnt2 += 1;
            } elseif ($text[$i] == 'l') {
                $cnt3 += 1;
            } elseif ($text[$i] == 'o') {
                $cnt4 += 1;
            } elseif ($text[$i] == 'n') {
                $cnt5 += 1;
            }
        }
        $cnt3 = floor($cnt3 / 2);
        $cnt4 = floor($cnt4 / 2);
        return min($cnt1, $cnt2, $cnt3, $cnt4, $cnt5);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
