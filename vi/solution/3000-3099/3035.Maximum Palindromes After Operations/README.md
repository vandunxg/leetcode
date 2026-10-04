---
comments: true
difficulty: Medium
rating: 1856
source: Weekly Contest 384 Q3
tags:
    - Greedy
    - Array
    - Hash Table
    - String
    - Counting
    - Sorting
---

<!-- problem:start -->

# [3035. Maximum Palindromes After Operations](https://leetcode.com/problems/maximum-palindromes-after-operations)

[中文文档](/solution/3000-3099/3035.Maximum%20Palindromes%20After%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng chuỗi <code>words</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <code>n</code> và chứa các chuỗi cũng được đánh chỉ số từ <strong>0</strong>.</p>

<p>Bạn được phép thực hiện thao tác sau <strong>bất kỳ</strong> số lần nào (<strong>kể cả</strong> <strong>0</strong>):</p>

<ul>
	<li>Chọn các số nguyên <code>i</code>, <code>j</code>, <code>x</code> và <code>y</code> sao cho <code>0 &lt;= i, j &lt; n</code>, <code>0 &lt;= x &lt; words[i].length</code>, <code>0 &lt;= y &lt; words[j].length</code>, rồi <strong>hoán đổi</strong> các ký tự <code>words[i][x]</code> và <code>words[j][y]</code>.</li>
</ul>

<p>Hãy trả về <em>một số nguyên biểu thị <strong>số lượng tối đa</strong> các <span data-keyword="palindrome-string">chuỗi đối xứng</span> mà</em> <code>words</code><em> có thể chứa sau khi thực hiện một số thao tác.</em></p>

<p><strong>Lưu ý:</strong> <code>i</code> và <code>j</code> có thể bằng nhau trong một thao tác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abbb&quot;,&quot;ba&quot;,&quot;aa&quot;]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong ví dụ này, một cách để đạt được số lượng chuỗi đối xứng tối đa là:
Chọn i = 0, j = 1, x = 0, y = 0, khi đó ta hoán đổi words[0][0] và words[1][0]. words trở thành [&quot;bbbb&quot;,&quot;aa&quot;,&quot;aa&quot;].
Tất cả các chuỗi trong words giờ đều là chuỗi đối xứng.
Vì vậy, số lượng chuỗi đối xứng tối đa có thể đạt được là 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;abc&quot;,&quot;ab&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Trong ví dụ này, một cách để đạt được số lượng chuỗi đối xứng tối đa là:
Chọn i = 0, j = 1, x = 1, y = 0, khi đó ta hoán đổi words[0][1] và words[1][0]. words trở thành [&quot;aac&quot;,&quot;bb&quot;].
Chọn i = 0, j = 0, x = 1, y = 2, khi đó ta hoán đổi words[0][1] và words[0][2]. words trở thành [&quot;aca&quot;,&quot;bb&quot;].
Cả hai chuỗi giờ đều là chuỗi đối xứng.
Vì vậy, số lượng chuỗi đối xứng tối đa có thể đạt được là 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;cd&quot;,&quot;ef&quot;,&quot;a&quot;]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Trong ví dụ này, không cần thực hiện thao tác nào.
Có một chuỗi đối xứng trong words là &quot;a&quot;.
Có thể chứng minh rằng không thể tạo ra nhiều hơn một chuỗi đối xứng sau bất kỳ số lần thao tác nào.
Vì vậy, đáp án là 1.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 100</code></li>
	<li><code>words[i]</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai chuỗi bất kỳ đều có thể hoán đổi ký tự cho nhau, và mục tiêu là tạo ra càng nhiều chuỗi đối xứng càng tốt. $n \le 1000$ với tổng độ dài bị giới hạn.
>
> Một chuỗi đối xứng chỉ cần các ký tự được ghép cặp. Tất cả ký tự tạo thành một pool; mỗi số lượng lẻ sẽ làm lãng phí một ký tự, còn các cặp còn lại nên được dùng để lấp đầy các chuỗi ngắn hơn trước.
>
> Một XOR mask đếm các ký tự có tần suất lẻ. Tổng độ dài trừ đi số lượng đó là ngân sách chẵn. Sau khi sắp xếp theo độ dài, ta trừ $2\lfloor |w|/2 \rfloor$ khỏi ngân sách.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPalindromesAfterOperations(self, words: List[str]) -> int:
        s = mask = 0
        for w in words:
            s += len(w)
            for c in w:
                mask ^= 1 << (ord(c) - ord("a"))
        s -= mask.bit_count()
        words.sort(key=len)
        ans = 0
        for w in words:
            s -= len(w) // 2 * 2
            if s < 0:
                break
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxPalindromesAfterOperations(String[] words) {
        int s = 0, mask = 0;
        for (var w : words) {
            s += w.length();
            for (var c : w.toCharArray()) {
                mask ^= 1 << (c - 'a');
            }
        }
        s -= Integer.bitCount(mask);
        Arrays.sort(words, (a, b) -> a.length() - b.length());
        int ans = 0;
        for (var w : words) {
            s -= w.length() / 2 * 2;
            if (s < 0) {
                break;
            }
            ++ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxPalindromesAfterOperations(vector<string>& words) {
        int s = 0, mask = 0;
        for (const auto& w : words) {
            s += w.length();
            for (char c : w) {
                mask ^= 1 << (c - 'a');
            }
        }
        s -= __builtin_popcount(mask);
        sort(words.begin(), words.end(), [](const string& a, const string& b) { return a.length() < b.length(); });
        int ans = 0;
        for (const auto& w : words) {
            s -= w.length() / 2 * 2;
            if (s < 0) {
                break;
            }
            ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func maxPalindromesAfterOperations(words []string) (ans int) {
	var s, mask int
	for _, w := range words {
		s += len(w)
		for _, c := range w {
			mask ^= 1 << (c - 'a')
		}
	}
	s -= bits.OnesCount(uint(mask))
	sort.Slice(words, func(i, j int) bool {
		return len(words[i]) < len(words[j])
	})
	for _, w := range words {
		s -= len(w) / 2 * 2
		if s < 0 {
			break
		}
		ans++
	}
	return
}
```

#### TypeScript

```ts
function maxPalindromesAfterOperations(words: string[]): number {
    let s: number = 0;
    let mask: number = 0;
    for (const w of words) {
        s += w.length;
        for (const c of w) {
            mask ^= 1 << (c.charCodeAt(0) - 'a'.charCodeAt(0));
        }
    }
    s -= (mask.toString(2).match(/1/g) || []).length;
    words.sort((a, b) => a.length - b.length);
    let ans: number = 0;
    for (const w of words) {
        s -= Math.floor(w.length / 2) * 2;
        if (s < 0) {
            break;
        }
        ans++;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
