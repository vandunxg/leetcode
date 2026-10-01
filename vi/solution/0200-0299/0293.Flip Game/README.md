---
comments: true
difficulty: Easy
tags:
    - String
---

<!-- problem:start -->

# [293. Flip Game 🔒](https://leetcode.com/problems/flip-game)

[中文文档](/solution/0200-0299/0293.Flip%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi Flip Game với bạn mình.</p>

<p>Bạn được cho chuỗi <code>currentState</code> chỉ chứa <code>&#39;+&#39;</code> và <code>&#39;-&#39;</code>. Bạn và bạn mình lần lượt đổi <strong>hai ký tự liên tiếp</strong> <code>&quot;++&quot;</code> thành <code>&quot;--&quot;</code>. Trò chơi kết thúc khi một người không thể đi tiếp; người còn lại sẽ thắng.</p>

<p>Hãy trả về tất cả trạng thái có thể có của chuỗi <code>currentState</code> sau <strong>một nước đi hợp lệ</strong>. Bạn có thể trả về kết quả theo <strong>bất kỳ thứ tự nào</strong>. Nếu không có nước đi hợp lệ, hãy trả về danh sách rỗng <code>[]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> currentState = &quot;++++&quot;
<strong>Đầu ra:</strong> [&quot;--++&quot;,&quot;+--+&quot;,&quot;++--&quot;]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> currentState = &quot;+&quot;
<strong>Đầu ra:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= currentState.length &lt;= 500</code></li>
	<li><code>currentState[i]</code> là <code>&#39;+&#39;</code> hoặc <code>&#39;-&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nước đi đổi một cặp $++$ thành $--$. Duyệt các cặp ký tự liền kề, đổi mỗi cặp $++$, lưu chuỗi thu được rồi khôi phục lại chuỗi.

<!-- thinking:end -->

Ta duyệt chuỗi. Nếu ký tự hiện tại và ký tự tiếp theo đều là `+`, ta đổi cả hai thành `-`, thêm chuỗi kết quả vào mảng đáp án, rồi đổi hai ký tự đó trở lại `+`.

Sau khi duyệt xong, ta trả về mảng đáp án.

Độ phức tạp thời gian là $O(n^2)$, với $n$ là độ dài chuỗi. Nếu không tính không gian của mảng kết quả, độ phức tạp không gian là $O(n)$ hoặc $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def generatePossibleNextMoves(self, currentState: str) -> List[str]:
        s = list(currentState)
        ans = []
        for i, (a, b) in enumerate(pairwise(s)):
            if a == b == "+":
                s[i] = s[i + 1] = "-"
                ans.append("".join(s))
                s[i] = s[i + 1] = "+"
        return ans
```

#### Java

```java
class Solution {
    public List<String> generatePossibleNextMoves(String currentState) {
        List<String> ans = new ArrayList<>();
        char[] s = currentState.toCharArray();
        for (int i = 0; i < s.length - 1; ++i) {
            if (s[i] == '+' && s[i + 1] == '+') {
                s[i] = '-';
                s[i + 1] = '-';
                ans.add(new String(s));
                s[i] = '+';
                s[i + 1] = '+';
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
    vector<string> generatePossibleNextMoves(string s) {
        vector<string> ans;
        for (int i = 0; i < s.size() - 1; ++i) {
            if (s[i] == '+' && s[i + 1] == '+') {
                s[i] = s[i + 1] = '-';
                ans.emplace_back(s);
                s[i] = s[i + 1] = '+';
            }
        }
        return ans;
    }
};
```

#### Go

```go
func generatePossibleNextMoves(currentState string) (ans []string) {
	s := []byte(currentState)
	for i := 0; i < len(s)-1; i++ {
		if s[i] == '+' && s[i+1] == '+' {
			s[i], s[i+1] = '-', '-'
			ans = append(ans, string(s))
			s[i], s[i+1] = '+', '+'
		}
	}
	return
}
```

#### TypeScript

```ts
function generatePossibleNextMoves(currentState: string): string[] {
    const s = currentState.split('');
    const ans: string[] = [];
    for (let i = 0; i < s.length - 1; ++i) {
        if (s[i] === '+' && s[i + 1] === '+') {
            s[i] = s[i + 1] = '-';
            ans.push(s.join(''));
            s[i] = s[i + 1] = '+';
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
