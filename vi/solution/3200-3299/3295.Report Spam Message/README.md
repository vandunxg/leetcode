---
comments: true
difficulty: Medium
rating: 1198
source: Weekly Contest 416 Q1
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [3295. Report Spam Message](https://leetcode.com/problems/report-spam-message)

[中文文档](/solution/3200-3299/3295.Report%20Spam%20Message/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>message</code> và một mảng chuỗi <code>bannedWords</code>.</p>

<p>Một mảng các từ được xem là <strong>spam</strong> nếu có <strong>ít nhất</strong> hai từ trong đó <b>khớp chính xác</b> với bất kỳ từ nào trong <code>bannedWords</code>.</p>

<p>Trả về <code>true</code> nếu mảng <code>message</code> là spam, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">message = [&quot;hello&quot;,&quot;world&quot;,&quot;leetcode&quot;], bannedWords = [&quot;world&quot;,&quot;hello&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hai từ <code>&quot;hello&quot;</code> và <code>&quot;world&quot;</code> trong mảng <code>message</code> đều xuất hiện trong mảng <code>bannedWords</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">message = [&quot;hello&quot;,&quot;programming&quot;,&quot;fun&quot;], bannedWords = [&quot;world&quot;,&quot;programming&quot;,&quot;leetcode&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có một từ trong mảng <code>message</code> (<code>&quot;programming&quot;</code>) xuất hiện trong mảng <code>bannedWords</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= message.length, bannedWords.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= message[i].length, bannedWords[i].length &lt;= 15</code></li>
	<li><code>message[i]</code> và <code>bannedWords[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một message là spam nếu có ít nhất hai từ nằm trong danh sách bị cấm. $n,m\le 10^5$, vì vậy duyệt danh sách từ bị cấm cho từng từ sẽ có độ phức tạp bậc hai.
>
> Đưa các từ bị cấm vào một hash set rồi đếm số từ xuất hiện trong message; trả về việc số lượng đó có ít nhất $2$ hay không. Độ phức tạp thời gian kỳ vọng là tuyến tính.

<!-- thinking:end -->

Chúng ta sử dụng một hash table $s$ để lưu tất cả các từ trong $\textit{bannedWords}$. Sau đó, chúng ta duyệt qua từng từ trong $\textit{message}$. Nếu từ đó xuất hiện trong hash table $s$, chúng ta tăng bộ đếm $cnt$ lên một. Nếu $cnt$ lớn hơn hoặc bằng $2$, chúng ta trả về $\text{true}$; nếu không, chúng ta trả về $\text{false}$.

Độ phức tạp thời gian là $O((n + m) \times |w|)$, và độ phức tạp không gian là $O(m \times |w|)$. Trong đó, $n$ là độ dài của mảng $\textit{message}$, còn $m$ và $|w|$ lần lượt là độ dài của mảng $\textit{bannedWords}$ và độ dài lớn nhất của các từ trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reportSpam(self, message: List[str], bannedWords: List[str]) -> bool:
        s = set(bannedWords)
        return sum(w in s for w in message) >= 2
```

#### Java

```java
class Solution {
    public boolean reportSpam(String[] message, String[] bannedWords) {
        Set<String> s = new HashSet<>();
        for (var w : bannedWords) {
            s.add(w);
        }
        int cnt = 0;
        for (var w : message) {
            if (s.contains(w) && ++cnt >= 2) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool reportSpam(vector<string>& message, vector<string>& bannedWords) {
        unordered_set<string> s(bannedWords.begin(), bannedWords.end());
        int cnt = 0;
        for (const auto& w : message) {
            if (s.contains(w) && ++cnt >= 2) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func reportSpam(message []string, bannedWords []string) bool {
	s := map[string]bool{}
	for _, w := range bannedWords {
		s[w] = true
	}
	cnt := 0
	for _, w := range message {
		if s[w] {
			cnt++
			if cnt >= 2 {
				return true
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function reportSpam(message: string[], bannedWords: string[]): boolean {
    const s = new Set<string>(bannedWords);
    let cnt = 0;
    for (const w of message) {
        if (s.has(w) && ++cnt >= 2) {
            return true;
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
