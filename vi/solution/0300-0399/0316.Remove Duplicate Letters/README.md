---
comments: true
difficulty: Medium
tags:
    - Stack
    - Greedy
    - String
    - Monotonic Stack
---

<!-- problem:start -->

# [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters)

[中文文档](/solution/0300-0399/0316.Remove%20Duplicate%20Letters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, hãy loại bỏ các lần xuất hiện trùng lặp để mỗi ký tự có trong chuỗi chỉ xuất hiện đúng một lần. Kết quả phải là <span data-keyword="lexicographically-smaller-string"><strong>chuỗi nhỏ nhất theo thứ tự từ điển</strong></span> trong tất cả các kết quả có thể.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;bcabc&quot;
<strong>Đầu ra:</strong> &quot;abc&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;cbacdcbc&quot;
<strong>Đầu ra:</strong> &quot;acdb&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống với bài 1081: <a href="https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/" target="_blank">https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm dãy con nhỏ nhất theo thứ tự từ điển, trong đó mỗi chữ cái xuất hiện đúng một lần. Có quá nhiều dãy con để thử hết.
>
> Ghi lại chỉ số xuất hiện cuối cùng của mỗi chữ cái. Duyệt từ trái sang phải: bỏ qua chữ cái đã có trong stack; nếu chưa có, liên tục pop phần tử trên cùng lớn hơn nó khi phần tử đó còn xuất hiện ở phía sau, rồi push chữ cái hiện tại. Monotonic stack giúp phần đầu kết quả nhỏ nhất có thể; chỉ số xuất hiện cuối bảo đảm chữ cái bị pop vẫn có thể được thêm lại.

<!-- thinking:end -->

Ta dùng mảng `last` để lưu vị trí xuất hiện cuối cùng của mỗi ký tự, một stack để lưu chuỗi kết quả, và mảng `vis` hoặc biến nguyên `mask` để ghi nhận ký tự hiện tại đã có trong stack hay chưa.

Duyệt chuỗi $s$. Với mỗi ký tự $c$ chưa có trong stack, kiểm tra phần tử trên cùng có lớn hơn $c$ hay không. Nếu lớn hơn và phần tử đó còn xuất hiện về sau, pop nó khỏi stack rồi push $c$ vào.

Cuối cùng, nối các phần tử trong stack thành chuỗi và trả về kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        last = {c: i for i, c in enumerate(s)}
        stk = []
        vis = set()
        for i, c in enumerate(s):
            if c in vis:
                continue
            while stk and stk[-1] > c and last[stk[-1]] > i:
                vis.remove(stk.pop())
            stk.append(c)
            vis.add(c)
        return ''.join(stk)
```

#### Java

```java
class Solution {
    public String removeDuplicateLetters(String s) {
        int n = s.length();
        int[] last = new int[26];
        for (int i = 0; i < n; ++i) {
            last[s.charAt(i) - 'a'] = i;
        }
        Deque<Character> stk = new ArrayDeque<>();
        int mask = 0;
        for (int i = 0; i < n; ++i) {
            char c = s.charAt(i);
            if (((mask >> (c - 'a')) & 1) == 1) {
                continue;
            }
            while (!stk.isEmpty() && stk.peek() > c && last[stk.peek() - 'a'] > i) {
                mask ^= 1 << (stk.pop() - 'a');
            }
            stk.push(c);
            mask |= 1 << (c - 'a');
        }
        StringBuilder ans = new StringBuilder();
        for (char c : stk) {
            ans.append(c);
        }
        return ans.reverse().toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeDuplicateLetters(string s) {
        int n = s.size();
        int last[26] = {0};
        for (int i = 0; i < n; ++i) {
            last[s[i] - 'a'] = i;
        }
        string ans;
        int mask = 0;
        for (int i = 0; i < n; ++i) {
            char c = s[i];
            if ((mask >> (c - 'a')) & 1) {
                continue;
            }
            while (!ans.empty() && ans.back() > c && last[ans.back() - 'a'] > i) {
                mask ^= 1 << (ans.back() - 'a');
                ans.pop_back();
            }
            ans.push_back(c);
            mask |= 1 << (c - 'a');
        }
        return ans;
    }
};
```

#### Go

```go
func removeDuplicateLetters(s string) string {
	last := make([]int, 26)
	for i, c := range s {
		last[c-'a'] = i
	}
	stk := []rune{}
	vis := make([]bool, 128)
	for i, c := range s {
		if vis[c] {
			continue
		}
		for len(stk) > 0 && stk[len(stk)-1] > c && last[stk[len(stk)-1]-'a'] > i {
			vis[stk[len(stk)-1]] = false
			stk = stk[:len(stk)-1]
		}
		stk = append(stk, c)
		vis[c] = true
	}
	return string(stk)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 dùng chỉ số xuất hiện cuối để kiểm tra ký tự còn xuất hiện hay không. Thay vào đó, đếm tần suất trước rồi giảm dần khi duyệt; pop phần tử trên cùng khi tần suất còn lại của nó lớn hơn 0. Cách kiểm tra này tương đương mà không cần lưu vị trí xuất hiện cuối.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        count, in_stack = [0] * 128, [False] * 128
        stack = []
        for c in s:
            count[ord(c)] += 1
        for c in s:
            count[ord(c)] -= 1
            if in_stack[ord(c)]:
                continue
            while len(stack) and stack[-1] > c:
                peek = stack[-1]
                if count[ord(peek)] < 1:
                    break
                in_stack[ord(peek)] = False
                stack.pop()
            stack.append(c)
            in_stack[ord(c)] = True
        return ''.join(stack)
```

#### Go

```go
func removeDuplicateLetters(s string) string {
	count, in_stack, stack := make([]int, 128), make([]bool, 128), make([]rune, 0)
	for _, c := range s {
		count[c] += 1
	}

	for _, c := range s {
		count[c] -= 1
		if in_stack[c] {
			continue
		}
		for len(stack) > 0 && stack[len(stack)-1] > c && count[stack[len(stack)-1]] > 0 {
			peek := stack[len(stack)-1]
			stack = stack[0 : len(stack)-1]
			in_stack[peek] = false
		}
		stack = append(stack, c)
		in_stack[c] = true
	}
	return string(stack)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
