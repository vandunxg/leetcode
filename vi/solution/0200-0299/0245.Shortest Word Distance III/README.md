---
comments: true
difficulty: Medium
tags:
    - Array
    - String
---

<!-- problem:start -->

# [245. Shortest Word Distance III 🔒](https://leetcode.com/problems/shortest-word-distance-iii)

[中文文档](/solution/0200-0299/0245.Shortest%20Word%20Distance%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>wordsDict</code> và hai chuỗi đã xuất hiện trong mảng là <code>word1</code> và <code>word2</code>, hãy trả về <em>khoảng cách ngắn nhất giữa vị trí xuất hiện của hai từ này trong danh sách</em>.</p>

<p><strong>Lưu ý</strong> rằng <code>word1</code> và <code>word2</code> có thể giống nhau. Đảm bảo rằng chúng biểu thị <strong>hai từ riêng biệt</strong> trong danh sách.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> wordsDict = ["practice", "makes", "perfect", "coding", "makes"], word1 = "makes", word2 = "coding"
<strong>Đầu ra:</strong> 1
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> wordsDict = ["practice", "makes", "perfect", "coding", "makes"], word1 = "makes", word2 = "makes"
<strong>Đầu ra:</strong> 3
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= wordsDict.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= wordsDict[i].length &lt;= 10</code></li>
	<li><code>wordsDict[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>word1</code> và <code>word2</code> đều nằm trong <code>wordsDict</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> $word1$ có thể bằng $word2$, khi đó đáp án là khoảng cách giữa hai lần xuất hiện liên tiếp của từ đó; nếu không, đó là khoảng cách thông thường giữa hai từ.
>
> Theo dõi chỉ số xuất hiện gần nhất tương ứng: dùng một pointer khi hai từ trùng nhau, hai pointer khi chúng khác nhau.

<!-- thinking:end -->

Trước tiên, chúng ta kiểm tra xem $\textit{word1}$ và $\textit{word2}$ có bằng nhau hay không:

- Nếu bằng nhau, duyệt qua mảng $\textit{wordsDict}$ để tìm hai chỉ số $i$ và $j$ của $\textit{word1}$, rồi tính giá trị nhỏ nhất của $i-j$.
- Nếu không bằng nhau, duyệt qua mảng $\textit{wordsDict}$ để tìm chỉ số $i$ của $\textit{word1}$ và $j$ của $\textit{word2}$, rồi tính giá trị nhỏ nhất của $i-j$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{wordsDict}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestWordDistance(self, wordsDict: List[str], word1: str, word2: str) -> int:
        ans = len(wordsDict)
        if word1 == word2:
            j = -1
            for i, w in enumerate(wordsDict):
                if w == word1:
                    if j != -1:
                        ans = min(ans, i - j)
                    j = i
        else:
            i = j = -1
            for k, w in enumerate(wordsDict):
                if w == word1:
                    i = k
                if w == word2:
                    j = k
                if i != -1 and j != -1:
                    ans = min(ans, abs(i - j))
        return ans
```

#### Java

```java
class Solution {
    public int shortestWordDistance(String[] wordsDict, String word1, String word2) {
        int ans = wordsDict.length;
        if (word1.equals(word2)) {
            for (int i = 0, j = -1; i < wordsDict.length; ++i) {
                if (wordsDict[i].equals(word1)) {
                    if (j != -1) {
                        ans = Math.min(ans, i - j);
                    }
                    j = i;
                }
            }
        } else {
            for (int k = 0, i = -1, j = -1; k < wordsDict.length; ++k) {
                if (wordsDict[k].equals(word1)) {
                    i = k;
                }
                if (wordsDict[k].equals(word2)) {
                    j = k;
                }
                if (i != -1 && j != -1) {
                    ans = Math.min(ans, Math.abs(i - j));
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
    int shortestWordDistance(vector<string>& wordsDict, string word1, string word2) {
        int n = wordsDict.size();
        int ans = n;
        if (word1 == word2) {
            for (int i = 0, j = -1; i < n; ++i) {
                if (wordsDict[i] == word1) {
                    if (j != -1) {
                        ans = min(ans, i - j);
                    }
                    j = i;
                }
            }
        } else {
            for (int k = 0, i = -1, j = -1; k < n; ++k) {
                if (wordsDict[k] == word1) {
                    i = k;
                }
                if (wordsDict[k] == word2) {
                    j = k;
                }
                if (i != -1 && j != -1) {
                    ans = min(ans, abs(i - j));
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func shortestWordDistance(wordsDict []string, word1 string, word2 string) int {
	ans := len(wordsDict)
	if word1 == word2 {
		j := -1
		for i, w := range wordsDict {
			if w == word1 {
				if j != -1 {
					ans = min(ans, i-j)
				}
				j = i
			}
		}
	} else {
		i, j := -1, -1
		for k, w := range wordsDict {
			if w == word1 {
				i = k
			}
			if w == word2 {
				j = k
			}
			if i != -1 && j != -1 {
				ans = min(ans, abs(i-j))
			}
		}
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function shortestWordDistance(wordsDict: string[], word1: string, word2: string): number {
    let ans = wordsDict.length;
    if (word1 === word2) {
        let j = -1;
        for (let i = 0; i < wordsDict.length; i++) {
            if (wordsDict[i] === word1) {
                if (j !== -1) {
                    ans = Math.min(ans, i - j);
                }
                j = i;
            }
        }
    } else {
        let i = -1,
            j = -1;
        for (let k = 0; k < wordsDict.length; k++) {
            if (wordsDict[k] === word1) {
                i = k;
            }
            if (wordsDict[k] === word2) {
                j = k;
            }
            if (i !== -1 && j !== -1) {
                ans = Math.min(ans, Math.abs(i - j));
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
