---
comments: true
difficulty: Easy
rating: 1329
source: Weekly Contest 390 Q1
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [3090. Maximum Length Substring With Two Occurrences](https://leetcode.com/problems/maximum-length-substring-with-two-occurrences)

[中文文档](/solution/3000-3099/3090.Maximum%20Length%20Substring%20With%20Two%20Occurrences/README.md)

## Mô tả

<!-- description:start -->

Cho một chuỗi <code>s</code>, hãy trả về độ dài <strong>lớn nhất</strong> của một <span data-keyword="substring">chuỗi con</span>&nbsp;sao cho mỗi ký tự xuất hiện <em>không quá hai lần</em>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;bcbbbcba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>
Chuỗi con sau đây có độ dài 4 và mỗi ký tự xuất hiện không quá hai lần: <code>&quot;bcbb<u>bcba</u>&quot;</code>.</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aaaa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>
Chuỗi con sau đây có độ dài 2 và mỗi ký tự xuất hiện không quá hai lần: <code>&quot;<u>aa</u>aa&quot;</code>.</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ký tự trong chuỗi con có thể xuất hiện nhiều nhất hai lần. $n \le 100$ cho phép liệt kê, nhưng ràng buộc này phù hợp với sliding window kinh điển.
>
> Sau khi đầu phải thêm một ký tự, nếu số lần xuất hiện nào đó vượt quá $2$ thì đầu trái phải tiến lên cho đến khi số lần đó trở lại $2$. Với cửa sổ hợp lệ, ta cập nhật độ dài lớn nhất.
>
> Một hash lưu số lần xuất hiện cùng hai con trỏ thực hiện được việc này trong một lượt duyệt.

<!-- thinking:end -->

Ta dùng hai con trỏ $l$ và $r$ để duy trì một sliding window, cùng một mảng $cnt$ để ghi nhận số lần xuất hiện của mỗi ký tự trong cửa sổ.

Trong mỗi vòng lặp, ta thêm ký tự $c$ tại con trỏ $r$ vào cửa sổ, sau đó kiểm tra xem $cnt[c]$ có lớn hơn $2$ hay không. Nếu có, ta di chuyển con trỏ $l$ sang phải cho đến khi $cnt[c]$ nhỏ hơn hoặc bằng $2$. Lúc này, ta cập nhật đáp án $ans = \max(ans, r - l + 1)$.

Cuối cùng, ta trả về đáp án $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự và trong bài này, $\Sigma = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumLengthSubstring(self, s: str) -> int:
        ans = l = 0
        cnt = defaultdict(int)
        for r, c in enumerate(s):
            cnt[c] += 1
            while cnt[c] > 2:
                cnt[s[l]] -= 1
                l += 1
            ans = max(ans, r - l + 1)
        return ans
```

#### Java

```java
class Solution {
    public int maximumLengthSubstring(String s) {
        int ans = 0;
        int[] cnt = new int[26];
        for (int l = 0, r = 0; r < s.length(); ++r) {
            int idx = s.charAt(r) - 'a';
            ++cnt[idx];
            while (cnt[idx] > 2) {
                --cnt[s.charAt(l++) - 'a'];
            }
            ans = Math.max(ans, r - l + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumLengthSubstring(string s) {
        int ans = 0;
        int cnt[26]{};
        for (int l = 0, r = 0; r < s.size(); ++r) {
            int idx = s[r] - 'a';
            ++cnt[idx];
            while (cnt[idx] > 2) {
                --cnt[s[l++] - 'a'];
            }
            ans = max(ans, r - l + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumLengthSubstring(s string) (ans int) {
	l := 0
	cnt := [26]int{}
	for r, c := range s {
		idx := int(c - 'a')
		cnt[idx]++
		for cnt[idx] > 2 {
			cnt[s[l]-'a']--
			l++
		}
		ans = max(ans, r-l+1)
	}
	return
}
```

#### TypeScript

```ts
function maximumLengthSubstring(s: string): number {
    let ans = 0;
    const cnt: number[] = Array(26).fill(0);
    for (let l = 0, r = 0; r < s.length; ++r) {
        const idx = s[r].charCodeAt(0) - 97;
        ++cnt[idx];
        while (cnt[idx] > 2) {
            --cnt[s[l++].charCodeAt(0) - 97];
        }
        ans = Math.max(ans, r - l + 1);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_length_substring(s: String) -> i32 {
        let mut cnt = [0; 26];
        let mut ans = 0;
        let mut l = 0;
        let s = s.as_bytes();

        for (r, &c) in s.iter().enumerate() {
            let i = (c - b'a') as usize;
            cnt[i] += 1;

            while cnt[i] > 2 {
                cnt[(s[l] - b'a') as usize] -= 1;
                l += 1;
            }

            ans = ans.max((r - l + 1) as i32);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
