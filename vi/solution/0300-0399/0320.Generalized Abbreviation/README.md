---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - String
    - Backtracking
---

<!-- problem:start -->

# [320. Generalized Abbreviation 🔒](https://leetcode.com/problems/generalized-abbreviation)

[中文文档](/solution/0300-0399/0320.Generalized%20Abbreviation/README.md)

## Mô tả

<!-- description:start -->

<p>Có thể tạo <strong>dạng viết tắt tổng quát</strong> của một từ bằng cách chọn tùy ý các <span data-keyword="substring-nonempty">chuỗi con</span> <strong>không chồng lấn</strong> và <strong>không liền kề</strong>, rồi thay mỗi chuỗi con bằng độ dài tương ứng.</p>

<ul>
	<li>Ví dụ, có thể viết tắt <code>&quot;abcde&quot;</code> thành:

    <ul>
    	<li><code>&quot;a3e&quot;</code> (thay <code>&quot;bcd&quot;</code> bằng <code>&quot;3&quot;</code>)</li>
    	<li><code>&quot;1bcd1&quot;</code> (thay cả <code>&quot;a&quot;</code> và <code>&quot;e&quot;</code> bằng <code>&quot;1&quot;</code>)</li>
    	<li><code>&quot;5&quot;</code> (thay <code>&quot;abcde&quot;</code> bằng <code>&quot;5&quot;</code>)</li>
    	<li><code>&quot;abcde&quot;</code> (không thay chuỗi con nào)</li>
    </ul>
    </li>
    <li>Tuy nhiên, các dạng viết tắt sau <strong>không hợp lệ</strong>:
    <ul>
    	<li><code>&quot;23&quot;</code> (thay <code>&quot;ab&quot;</code> bằng <code>&quot;2&quot;</code> và <code>&quot;cde&quot;</code> bằng <code>&quot;3&quot;</code>) không hợp lệ vì hai chuỗi con được chọn nằm liền kề nhau.</li>
    	<li><code>&quot;22de&quot;</code> (thay <code>&quot;ab&quot;</code> bằng <code>&quot;2&quot;</code> và <code>&quot;bc&quot;</code> bằng <code>&quot;2&quot;</code>) không hợp lệ vì hai chuỗi con được chọn chồng lấn.</li>
    </ul>
    </li>

</ul>

<p>Cho chuỗi <code>word</code>, hãy trả về <em>danh sách tất cả các <strong>dạng viết tắt tổng quát</strong> có thể tạo ra từ</em> <code>word</code>. Có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> word = "word"
<strong>Đầu ra:</strong> ["4","3d","2r1","2rd","1o2","1o1d","1or1","1ord","w3","w2d","w1r1","w1rd","wo2","wo1d","wor1","word"]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> word = "a"
<strong>Đầu ra:</strong> ["1","a"]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 15</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chữ cái được giữ lại hoặc gộp vào một nhóm bị lược bỏ và biểu diễn bằng số lượng. Vì $n\le 15$, có thể liệt kê mọi cách.
>
> $dfs(i)$ xử lý hậu tố: giữ lại $word[i]$ rồi đệ quy, hoặc gộp đoạn $[i,j)$ thành một số rồi nối $word[j]$ (nếu có) và phần còn lại. Với hậu tố rỗng, trả về $[""]$ làm chuỗi cơ sở để ghép.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i)$ để trả về mọi dạng viết tắt có thể của chuỗi $word[i:]$.

Hàm $dfs(i)$ hoạt động như sau:

Nếu $i \geq n$, nghĩa là đã xử lý hết chuỗi $word$, ta trả về danh sách chỉ chứa chuỗi rỗng.

Nếu không, ta có thể giữ lại $word[i]$, thêm $word[i]$ vào đầu mỗi chuỗi trong danh sách do $dfs(i + 1)$ trả về, rồi thêm các kết quả đó vào đáp án.

Ta cũng có thể lược bỏ $word[i]$ cùng một số ký tự phía sau. Giả sử ta lược bỏ $word[i..j)$; khi đó ký tự thứ $j$ không bị lược bỏ. Ta thêm $j - i$ vào đầu mỗi chuỗi trong danh sách do $dfs(j + 1)$ trả về, rồi thêm các kết quả thu được vào đáp án.

Cuối cùng, ta gọi $dfs(0)$ trong hàm chính.

Độ phức tạp thời gian là $O(n \times 2^n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $word$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def generateAbbreviations(self, word: str) -> List[str]:
        def dfs(i: int) -> List[str]:
            if i >= n:
                return [""]
            ans = [word[i] + s for s in dfs(i + 1)]
            for j in range(i + 1, n + 1):
                for s in dfs(j + 1):
                    ans.append(str(j - i) + (word[j] if j < n else "") + s)
            return ans

        n = len(word)
        return dfs(0)
```

#### Java

```java
class Solution {
    private String word;
    private int n;

    public List<String> generateAbbreviations(String word) {
        this.word = word;
        n = word.length();
        return dfs(0);
    }

    private List<String> dfs(int i) {
        if (i >= n) {
            return List.of("");
        }
        List<String> ans = new ArrayList<>();
        for (String s : dfs(i + 1)) {
            ans.add(String.valueOf(word.charAt(i)) + s);
        }
        for (int j = i + 1; j <= n; ++j) {
            for (String s : dfs(j + 1)) {
                ans.add((j - i) + "" + (j < n ? String.valueOf(word.charAt(j)) : "") + s);
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
    vector<string> generateAbbreviations(string word) {
        int n = word.size();
        function<vector<string>(int)> dfs = [&](int i) -> vector<string> {
            if (i >= n) {
                return {""};
            }
            vector<string> ans;
            for (auto& s : dfs(i + 1)) {
                string p(1, word[i]);
                ans.emplace_back(p + s);
            }
            for (int j = i + 1; j <= n; ++j) {
                for (auto& s : dfs(j + 1)) {
                    string p = j < n ? string(1, word[j]) : "";
                    ans.emplace_back(to_string(j - i) + p + s);
                }
            }
            return ans;
        };
        return dfs(0);
    }
};
```

#### Go

```go
func generateAbbreviations(word string) []string {
	n := len(word)
	var dfs func(int) []string
	dfs = func(i int) []string {
		if i >= n {
			return []string{""}
		}
		ans := []string{}
		for _, s := range dfs(i + 1) {
			ans = append(ans, word[i:i+1]+s)
		}
		for j := i + 1; j <= n; j++ {
			for _, s := range dfs(j + 1) {
				p := ""
				if j < n {
					p = word[j : j+1]
				}
				ans = append(ans, strconv.Itoa(j-i)+p+s)
			}
		}
		return ans
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function generateAbbreviations(word: string): string[] {
    const n = word.length;
    const dfs = (i: number): string[] => {
        if (i >= n) {
            return [''];
        }
        const ans: string[] = [];
        for (const s of dfs(i + 1)) {
            ans.push(word[i] + s);
        }
        for (let j = i + 1; j <= n; ++j) {
            for (const s of dfs(j + 1)) {
                ans.push((j - i).toString() + (j < n ? word[j] : '') + s);
            }
        }
        return ans;
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Đệ quy tương ứng với $2^n$ cách chọn giữ hoặc lược bỏ. Bitmask đánh dấu các vị trí bị lược bỏ; mỗi dãy bit $1$ liên tiếp được thay bằng số lượng tương ứng, còn bit $0$ biểu thị giữ lại chữ cái. Cách lặp không cần call stack.

<!-- thinking:end -->

Vì độ dài chuỗi $word$ không vượt quá $15$, ta có thể dùng cách duyệt nhị phân để liệt kê mọi dạng viết tắt. Dùng số nhị phân $i$ có độ dài $n$ để biểu diễn một dạng viết tắt: bit $0$ nghĩa là giữ ký tự tương ứng, còn bit $1$ nghĩa là lược bỏ ký tự đó. Ta duyệt mọi $i$ trong khoảng $[0, 2^n)$, chuyển mỗi giá trị thành dạng viết tắt tương ứng rồi thêm vào danh sách đáp án.

Độ phức tạp thời gian là $O(n \times 2^n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $word$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def generateAbbreviations(self, word: str) -> List[str]:
        n = len(word)
        ans = []
        for i in range(1 << n):
            cnt = 0
            s = []
            for j in range(n):
                if i >> j & 1:
                    cnt += 1
                else:
                    if cnt:
                        s.append(str(cnt))
                        cnt = 0
                    s.append(word[j])
            if cnt:
                s.append(str(cnt))
            ans.append("".join(s))
        return ans
```

#### Java

```java
class Solution {
    public List<String> generateAbbreviations(String word) {
        int n = word.length();
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < 1 << n; ++i) {
            StringBuilder s = new StringBuilder();
            int cnt = 0;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    ++cnt;
                } else {
                    if (cnt > 0) {
                        s.append(cnt);
                        cnt = 0;
                    }
                    s.append(word.charAt(j));
                }
            }
            if (cnt > 0) {
                s.append(cnt);
            }
            ans.add(s.toString());
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> generateAbbreviations(string word) {
        int n = word.size();
        vector<string> ans;
        for (int i = 0; i < 1 << n; ++i) {
            string s;
            int cnt = 0;
            for (int j = 0; j < n; ++j) {
                if (i >> j & 1) {
                    ++cnt;
                } else {
                    if (cnt) {
                        s += to_string(cnt);
                        cnt = 0;
                    }
                    s.push_back(word[j]);
                }
            }
            if (cnt) {
                s += to_string(cnt);
            }
            ans.push_back(s);
        }
        return ans;
    }
};
```

#### Go

```go
func generateAbbreviations(word string) (ans []string) {
	n := len(word)
	for i := 0; i < 1<<n; i++ {
		s := &strings.Builder{}
		cnt := 0
		for j := 0; j < n; j++ {
			if i>>j&1 == 1 {
				cnt++
			} else {
				if cnt > 0 {
					s.WriteString(strconv.Itoa(cnt))
					cnt = 0
				}
				s.WriteByte(word[j])
			}
		}
		if cnt > 0 {
			s.WriteString(strconv.Itoa(cnt))
		}
		ans = append(ans, s.String())
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
