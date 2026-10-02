---
comments: true
difficulty: Hard
rating: 1881
source: Weekly Contest 162 Q4
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - String
    - Dynamic Programming
    - Backtracking
    - Bitmask
    - Counting
---

<!-- problem:start -->

# [1255. Maximum Score Words Formed by Letters](https://leetcode.com/problems/maximum-score-words-formed-by-letters)

[中文文档](/solution/1200-1299/1255.Maximum%20Score%20Words%20Formed%20by%20Letters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách <code>words</code>, danh sách các ký tự đơn <code>letters</code> (có thể có ký tự lặp lại) và <code>score</code> của từng ký tự.</p>

<p>Trả về tổng điểm lớn nhất của <strong>bất kỳ</strong> tập từ hợp lệ nào có thể tạo từ các ký tự đã cho (không được dùng <code>words[i]</code> từ hai lần trở lên).</p>

<p>Không bắt buộc phải dùng hết ký tự trong <code>letters</code>, và mỗi ký tự chỉ được dùng một lần. Điểm của các chữ cái <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code>, <code>&#39;c&#39;</code>, ..., <code>&#39;z&#39;</code> lần lượt được cho bởi <code>score[0]</code>, <code>score[1]</code>, ..., <code>score[25]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;dog&quot;,&quot;cat&quot;,&quot;dad&quot;,&quot;good&quot;], letters = [&quot;a&quot;,&quot;a&quot;,&quot;c&quot;,&quot;d&quot;,&quot;d&quot;,&quot;d&quot;,&quot;g&quot;,&quot;o&quot;,&quot;o&quot;], score = [1,0,9,5,0,0,3,0,0,0,0,0,0,0,2,0,0,0,0,0,0,0,0,0,0,0]
<strong>Đầu ra:</strong> 23
<strong>Giải thích:</strong>
Điểm: a=1, c=9, d=5, g=3, o=2
Với các ký tự đã cho, ta có thể tạo các từ &quot;dad&quot; (5+1+5) và &quot;good&quot; (3+2+2+5), đạt tổng điểm 23.
Hai từ &quot;dad&quot; và &quot;dog&quot; chỉ đạt tổng điểm 21.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;xxxz&quot;,&quot;ax&quot;,&quot;bx&quot;,&quot;cx&quot;], letters = [&quot;z&quot;,&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;x&quot;,&quot;x&quot;,&quot;x&quot;], score = [4,4,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,5,0,10]
<strong>Đầu ra:</strong> 27
<strong>Giải thích:</strong>
Điểm: a=4, b=4, c=4, x=5, z=10
Với các ký tự đã cho, ta có thể tạo các từ &quot;ax&quot; (4+5), &quot;bx&quot; (4+5) và &quot;cx&quot; (4+5), đạt tổng điểm 27.
Từ &quot;xxxz&quot; chỉ đạt điểm 25.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;leetcode&quot;], letters = [&quot;l&quot;,&quot;e&quot;,&quot;t&quot;,&quot;c&quot;,&quot;o&quot;,&quot;d&quot;], score = [0,0,1,1,1,0,0,0,0,0,0,1,0,0,1,0,0,0,0,1,0,0,0,0,0,0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Chỉ có thể sử dụng chữ cái &quot;e&quot; một lần.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 14</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 15</code></li>
	<li><code>1 &lt;= letters.length &lt;= 100</code></li>
	<li><code>letters[i].length == 1</code></li>
	<li><code>score.length ==&nbsp;26</code></li>
	<li><code>0 &lt;= score[i] &lt;= 10</code></li>
	<li><code>words[i]</code>, <code>letters[i]</code>&nbsp;contains only lower case English letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Có tối đa $14$ từ nên ta có thể liệt kê $2^{14}$ tập con. Một tập con hợp lệ khi số lần xuất hiện của mỗi chữ cái không vượt quá số lượng có trong $letters$.
>
> Ta đếm số lượng từng chữ cái, rồi với mỗi mask biểu diễn một lựa chọn, ghép các từ được chọn, kiểm tra tần suất ký tự và tính điểm nếu hợp lệ. Duyệt mọi mask sẽ xét hết các lựa chọn; điều kiện tần suất đảm bảo không dùng quá số chữ cái có sẵn.

<!-- thinking:end -->

Vì giới hạn dữ liệu nhỏ, ta có thể dùng liệt kê nhị phân để xét mọi tổ hợp từ trong danh sách. Sau đó, kiểm tra từng tổ hợp có thỏa mãn yêu cầu không; nếu có thì tính điểm và chọn tổ hợp có điểm cao nhất.

Trước tiên, ta dùng hash table hoặc mảng $cnt$ để ghi lại số lần xuất hiện của mỗi chữ cái trong $letters$.

Tiếp theo, ta dùng liệt kê nhị phân để xét mọi tổ hợp từ. Mỗi bit biểu thị từ tương ứng trong danh sách có được chọn hay không. Nếu bit thứ $i$ bằng $1$, ta chọn từ thứ $i$; ngược lại thì không chọn.

Sau đó, ta đếm số lần xuất hiện của từng chữ cái trong tổ hợp hiện tại và lưu vào hash table hoặc mảng $cur$. Nếu tần suất của mỗi chữ cái trong $cur$ không vượt quá tần suất tương ứng trong $cnt$, tổ hợp đó hợp lệ. Ta tính điểm của tổ hợp và cập nhật tổ hợp có điểm cao nhất.

Độ phức tạp thời gian là $(2^n \times n \times M)$, còn độ phức tạp không gian là $O(C)$. Trong đó, $n$ là số từ trong danh sách, $M$ là độ dài lớn nhất của một từ, còn $C$ là số chữ cái trong bảng chữ cái; với bài này, $C=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScoreWords(
        self, words: List[str], letters: List[str], score: List[int]
    ) -> int:
        cnt = Counter(letters)
        n = len(words)
        ans = 0
        for i in range(1 << n):
            cur = Counter(''.join([words[j] for j in range(n) if i >> j & 1]))
            if all(v <= cnt[c] for c, v in cur.items()):
                t = sum(v * score[ord(c) - ord('a')] for c, v in cur.items())
                ans = max(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public int maxScoreWords(String[] words, char[] letters, int[] score) {
        int[] cnt = new int[26];
        for (int i = 0; i < letters.length; ++i) {
            cnt[letters[i] - 'a']++;
        }
        int n = words.length;
        int ans = 0;
        for (int i = 0; i < 1 << n; ++i) {
            int[] cur = new int[26];
            for (int j = 0; j < n; ++j) {
                if (((i >> j) & 1) == 1) {
                    for (int k = 0; k < words[j].length(); ++k) {
                        cur[words[j].charAt(k) - 'a']++;
                    }
                }
            }
            boolean ok = true;
            int t = 0;
            for (int j = 0; j < 26; ++j) {
                if (cur[j] > cnt[j]) {
                    ok = false;
                    break;
                }
                t += cur[j] * score[j];
            }
            if (ok && ans < t) {
                ans = t;
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
    int maxScoreWords(vector<string>& words, vector<char>& letters, vector<int>& score) {
        int cnt[26]{};
        for (char& c : letters) {
            cnt[c - 'a']++;
        }
        int n = words.size();
        int ans = 0;
        for (int i = 0; i < 1 << n; ++i) {
            int cur[26]{};
            for (int j = 0; j < n; ++j) {
                if (i >> j & 1) {
                    for (char& c : words[j]) {
                        cur[c - 'a']++;
                    }
                }
            }
            bool ok = true;
            int t = 0;
            for (int j = 0; j < 26; ++j) {
                if (cur[j] > cnt[j]) {
                    ok = false;
                    break;
                }
                t += cur[j] * score[j];
            }
            if (ok && ans < t) {
                ans = t;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxScoreWords(words []string, letters []byte, score []int) (ans int) {
	cnt := [26]int{}
	for _, c := range letters {
		cnt[c-'a']++
	}
	n := len(words)
	for i := 0; i < 1<<n; i++ {
		cur := [26]int{}
		for j := 0; j < n; j++ {
			if i>>j&1 == 1 {
				for _, c := range words[j] {
					cur[c-'a']++
				}
			}
		}
		ok := true
		t := 0
		for i, v := range cur {
			if v > cnt[i] {
				ok = false
				break
			}
			t += v * score[i]
		}
		if ok && ans < t {
			ans = t
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
