---
comments: true
difficulty: Medium
rating: 1828
source: Weekly Contest 275 Q3
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [2135. Count Words Obtained After Adding a Letter](https://leetcode.com/problems/count-words-obtained-after-adding-a-letter)

[中文文档](/solution/2100-2199/2135.Count%20Words%20Obtained%20After%20Adding%20a%20Letter/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng chuỗi <code>startWords</code> và <code>targetWords</code>, đều được đánh chỉ số từ <strong>0</strong>. Mỗi chuỗi chỉ gồm các <strong>chữ cái tiếng Anh viết thường</strong>.</p>

<p>Với mỗi chuỗi trong <code>targetWords</code>, hãy kiểm tra xem có thể chọn một chuỗi từ <code>startWords</code> rồi thực hiện một <strong>phép biến đổi</strong> trên chuỗi đó để thu được chuỗi trong <code>targetWords</code> hay không.</p>

<p><strong>Phép biến đổi</strong> gồm hai bước sau:</p>

<ol>
	<li><strong>Thêm vào cuối</strong> chuỗi một chữ cái viết thường <strong>chưa xuất hiện</strong> trong chuỗi.

    <ul>
    <li>Ví dụ, nếu chuỗi là <code>&quot;abc&quot;</code>, ta có thể thêm các chữ cái <code>&#39;d&#39;</code>, <code>&#39;e&#39;</code> hoặc <code>&#39;y&#39;</code>, nhưng không thể thêm <code>&#39;a&#39;</code>. Nếu thêm <code>&#39;d&#39;</code>, chuỗi kết quả sẽ là <code>&quot;abcd&quot;</code>.</li>
    </ul>
    </li>
    <li><strong>Sắp xếp lại</strong> các chữ cái của chuỗi mới theo <strong>bất kỳ thứ tự nào</strong>.
    <ul>
    <li>Ví dụ, có thể sắp xếp lại <code>&quot;abcd&quot;</code> thành <code>&quot;acbd&quot;</code>, <code>&quot;bacd&quot;</code>, <code>&quot;cbda&quot;</code>, v.v. Lưu ý rằng cũng có thể giữ nguyên thành <code>&quot;abcd&quot;</code>.</li>
    </ul>
    </li>

</ol>

<p>Hãy trả về <em><strong>số lượng chuỗi</strong> trong </em><code>targetWords</code><em> có thể thu được bằng cách thực hiện các phép biến đổi trên <strong>bất kỳ</strong> chuỗi nào trong </em><code>startWords</code>.</p>

<p><strong>Lưu ý</strong> rằng ta chỉ kiểm tra xem chuỗi trong <code>targetWords</code> có thể được tạo từ một chuỗi trong <code>startWords</code> bằng các phép biến đổi hay không. Các chuỗi trong <code>startWords</code> <strong>không</strong> thực sự thay đổi trong quá trình này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> startWords = [&quot;ant&quot;,&quot;act&quot;,&quot;tack&quot;], targetWords = [&quot;tack&quot;,&quot;act&quot;,&quot;acti&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- Để tạo targetWords[0] = &quot;tack&quot;, ta dùng startWords[1] = &quot;act&quot;, thêm &#39;k&#39; vào đó rồi sắp xếp lại &quot;actk&quot; thành &quot;tack&quot;.
- Không có chuỗi nào trong startWords có thể dùng để tạo targetWords[1] = &quot;act&quot;.
  Lưu ý rằng &quot;act&quot; có tồn tại trong startWords, nhưng ta <strong>bắt buộc</strong> phải thêm một chữ cái vào chuỗi trước khi sắp xếp lại.
- Để tạo targetWords[2] = &quot;acti&quot;, ta dùng startWords[1] = &quot;act&quot;, thêm &#39;i&#39; vào đó rồi sắp xếp lại thành chính &quot;acti&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> startWords = [&quot;ab&quot;,&quot;a&quot;], targetWords = [&quot;abc&quot;,&quot;abcd&quot;]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
- Để tạo targetWords[0] = &quot;abc&quot;, ta dùng startWords[0] = &quot;ab&quot;, thêm &#39;c&#39; vào đó rồi sắp xếp lại thành &quot;abc&quot;.
- Không có chuỗi nào trong startWords có thể dùng để tạo targetWords[1] = &quot;abcd&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= startWords.length, targetWords.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= startWords[i].length, targetWords[j].length &lt;= 26</code></li>
	<li>Mỗi chuỗi trong <code>startWords</code> và <code>targetWords</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Không có chữ cái nào xuất hiện quá một lần trong bất kỳ chuỗi nào của <code>startWords</code> hoặc <code>targetWords</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Bit Manipulation

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ cái viết thường trong mỗi từ đều khác nhau; xét theo thứ tự, một target chính là một từ bắt đầu cộng thêm một chữ cái. Có thể xóa từng chữ cái của target rồi sắp xếp hai phía để so sánh, nhưng cách đó tạo thêm nhiều chuỗi.
>
> Một mask 26 bit biểu diễn tập hợp các chữ cái. Ta lưu các mask của những từ bắt đầu vào một hash set; một target hợp lệ nếu sau khi xóa một bit của nó, kết quả nằm trong set.
>
> Trước tiên xây dựng set từ $\textit{startWords}$, sau đó thử xóa từng chữ cái của mỗi target.

<!-- thinking:end -->

Ta nhận thấy các chuỗi đã cho chỉ chứa chữ cái viết thường và mỗi chữ cái trong một chuỗi xuất hiện nhiều nhất một lần. Do đó, ta có thể biểu diễn một chuỗi bằng một số nhị phân có độ dài $26$, trong đó bit thứ $i$ bằng $1$ cho biết chuỗi chứa chữ cái viết thường thứ $i$, còn bằng $0$ cho biết chuỗi không chứa chữ cái viết thường thứ $i$.

Ta có thể chuyển mỗi chuỗi trong mảng $\textit{startWords}$ thành một số nhị phân rồi lưu các số này vào một set $\textit{s}$. Với mỗi chuỗi trong mảng $\textit{targetWords}$, trước tiên ta chuyển nó thành một số nhị phân, sau đó duyệt qua từng chữ cái trong chuỗi, xóa chữ cái đó khỏi số nhị phân và kiểm tra xem có số nhị phân nào trong set $\textit{s}$ sao cho kết quả XOR của số đó với số nhị phân của chữ cái đã xóa cũng nằm trong set $\textit{s}$ hay không. Nếu tồn tại một số như vậy, chuỗi này có thể thu được bằng cách thực hiện phép biến đổi trên một chuỗi nào đó trong $\textit{startWords}$, nên ta tăng đáp án lên một. Sau đó, ta bỏ qua chuỗi này và tiếp tục xử lý chuỗi tiếp theo.

Độ phức tạp thời gian là $O(n \times |\Sigma|)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng chuỗi $\textit{targetWords}$, còn $|\Sigma|$ là kích thước của tập ký tự trong chuỗi, bằng $26$ trong bài toán này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def wordCount(self, startWords: List[str], targetWords: List[str]) -> int:
        s = {sum(1 << (ord(c) - 97) for c in w) for w in startWords}
        ans = 0
        for w in targetWords:
            x = sum(1 << (ord(c) - 97) for c in w)
            for c in w:
                if x ^ (1 << (ord(c) - 97)) in s:
                    ans += 1
                    break
        return ans
```

#### Java

```java
class Solution {
    public int wordCount(String[] startWords, String[] targetWords) {
        Set<Integer> s = new HashSet<>();
        for (var w : startWords) {
            int x = 0;
            for (var c : w.toCharArray()) {
                x |= 1 << (c - 'a');
            }
            s.add(x);
        }
        int ans = 0;
        for (var w : targetWords) {
            int x = 0;
            for (var c : w.toCharArray()) {
                x |= 1 << (c - 'a');
            }
            for (var c : w.toCharArray()) {
                if (s.contains(x ^ (1 << (c - 'a')))) {
                    ++ans;
                    break;
                }
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
    int wordCount(vector<string>& startWords, vector<string>& targetWords) {
        unordered_set<int> s;
        for (auto& w : startWords) {
            int x = 0;
            for (char c : w) {
                x |= 1 << (c - 'a');
            }
            s.insert(x);
        }
        int ans = 0;
        for (auto& w : targetWords) {
            int x = 0;
            for (char c : w) {
                x |= 1 << (c - 'a');
            }
            for (char c : w) {
                if (s.contains(x ^ (1 << (c - 'a')))) {
                    ++ans;
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func wordCount(startWords []string, targetWords []string) (ans int) {
	s := map[int]bool{}
	for _, w := range startWords {
		x := 0
		for _, c := range w {
			x |= 1 << (c - 'a')
		}
		s[x] = true
	}
	for _, w := range targetWords {
		x := 0
		for _, c := range w {
			x |= 1 << (c - 'a')
		}
		for _, c := range w {
			if s[x^(1<<(c-'a'))] {
				ans++
				break
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function wordCount(startWords: string[], targetWords: string[]): number {
    const s = new Set<number>();
    for (const w of startWords) {
        let x = 0;
        for (const c of w) {
            x ^= 1 << (c.charCodeAt(0) - 97);
        }
        s.add(x);
    }
    let ans = 0;
    for (const w of targetWords) {
        let x = 0;
        for (const c of w) {
            x ^= 1 << (c.charCodeAt(0) - 97);
        }
        for (const c of w) {
            if (s.has(x ^ (1 << (c.charCodeAt(0) - 97)))) {
                ++ans;
                break;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
