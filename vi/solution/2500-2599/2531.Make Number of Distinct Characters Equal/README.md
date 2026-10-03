---
comments: true
difficulty: Medium
rating: 1775
source: Weekly Contest 327 Q3
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2531. Make Number of Distinct Characters Equal](https://leetcode.com/problems/make-number-of-distinct-characters-equal)

[中文文档](/solution/2500-2599/2531.Make%20Number%20of%20Distinct%20Characters%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <strong>được đánh chỉ số từ 0</strong> là <code>word1</code> và <code>word2</code>.</p>

<p>Một <strong>phép di chuyển</strong> gồm việc chọn hai chỉ số <code>i</code> và <code>j</code> sao cho <code>0 &lt;= i &lt; word1.length</code> và <code>0 &lt;= j &lt; word2.length</code>, rồi đổi chỗ <code>word1[i]</code> với <code>word2[j]</code>.</p>

<p>Trả về <code>true</code> <em>nếu có thể làm cho số lượng ký tự phân biệt trong</em> <code>word1</code> <em>và</em> <code>word2</code> <em>bằng nhau sau <strong>chính xác một</strong> phép di chuyển.</em> Trả về <code>false</code> <em>nếu không.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;ac&quot;, word2 = &quot;b&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Mọi cặp hoán đổi đều tạo ra hai ký tự phân biệt trong chuỗi thứ nhất và một ký tự trong chuỗi thứ hai.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;abcc&quot;, word2 = &quot;aab&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta đổi chỗ chỉ số 2 của chuỗi thứ nhất với chỉ số 0 của chuỗi thứ hai. Hai chuỗi thu được là word1 = &quot;abac&quot; và word2 = &quot;cab&quot;, cả hai đều có 3 ký tự phân biệt.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;abcde&quot;, word2 = &quot;fghij&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hai chuỗi sau khi đổi chỗ luôn có 5 ký tự phân biệt, bất kể ta đổi chỗ những chỉ số nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word1.length, word2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>word1</code> và <code>word2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải đổi chỗ chính xác một ký tự để số lượng chữ cái phân biệt trong hai chuỗi bằng nhau. Chuỗi có thể rất dài, nhưng chỉ có $26$ chữ cái cần xét — điều quan trọng là tần suất, không phải vị trí.
>
> Ta đếm tần suất và số lượng ký tự phân biệt $x,y$. Sau đó, liệt kê chữ cái $c_1$ lấy từ chuỗi thứ nhất và $c_2$ lấy từ chuỗi thứ hai. Nếu hai chữ cái giống nhau thì số lượng không đổi, nên chỉ cần kiểm tra $x=y$; nếu khác nhau, điều chỉnh số lượng mỗi phía dựa trên việc một chữ cái biến mất hoặc xuất hiện, rồi kiểm tra xem hai số lượng mới có bằng nhau không.

<!-- thinking:end -->

Trước tiên, ta sử dụng hai mảng $\textit{cnt1}$ và $\textit{cnt2}$ có độ dài $26$ để ghi lại tần suất của mỗi ký tự trong hai chuỗi $\textit{word1}$ và $\textit{word2}$.

Sau đó, ta đếm số lượng ký tự phân biệt trong $\textit{word1}$ và $\textit{word2}$, lần lượt ký hiệu là $x$ và $y$.

Tiếp theo, ta liệt kê từng ký tự $c1$ trong $\textit{word1}$ và từng ký tự $c2$ trong $\textit{word2}$. Nếu $c1 = c2$, ta chỉ cần kiểm tra xem $x$ và $y$ có bằng nhau không; nếu khác nhau, ta cần kiểm tra xem $x - (\textit{cnt1}[c1] = 1) + (\textit{cnt1}[c2] = 0)$ và $y - (\textit{cnt2}[c2] = 1) + (\textit{cnt2}[c1] = 0)$ có bằng nhau không. Nếu bằng nhau, ta đã tìm được đáp án và trả về $\text{true}$.

Nếu đã liệt kê tất cả các ký tự mà vẫn không tìm được cách phù hợp, ta trả về $\text{false}$.

Độ phức tạp thời gian là $O(m + n + |\Sigma|^2)$, trong đó $m$ và $n$ lần lượt là độ dài của các chuỗi $\textit{word1}$ và $\textit{word2}$, còn $\Sigma$ là tập ký tự. Trong bài toán này, tập ký tự gồm các chữ cái viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isItPossible(self, word1: str, word2: str) -> bool:
        cnt1 = Counter(word1)
        cnt2 = Counter(word2)
        x, y = len(cnt1), len(cnt2)
        for c1, v1 in cnt1.items():
            for c2, v2 in cnt2.items():
                if c1 == c2:
                    if x == y:
                        return True
                else:
                    a = x - (v1 == 1) + (cnt1[c2] == 0)
                    b = y - (v2 == 1) + (cnt2[c1] == 0)
                    if a == b:
                        return True
        return False
```

#### Java

```java
class Solution {
    public boolean isItPossible(String word1, String word2) {
        int[] cnt1 = new int[26];
        int[] cnt2 = new int[26];
        int x = 0, y = 0;
        for (int i = 0; i < word1.length(); ++i) {
            if (++cnt1[word1.charAt(i) - 'a'] == 1) {
                ++x;
            }
        }
        for (int i = 0; i < word2.length(); ++i) {
            if (++cnt2[word2.charAt(i) - 'a'] == 1) {
                ++y;
            }
        }
        for (int i = 0; i < 26; ++i) {
            for (int j = 0; j < 26; ++j) {
                if (cnt1[i] > 0 && cnt2[j] > 0) {
                    if (i == j) {
                        if (x == y) {
                            return true;
                        }
                    } else {
                        int a = x - (cnt1[i] == 1 ? 1 : 0) + (cnt1[j] == 0 ? 1 : 0);
                        int b = y - (cnt2[j] == 1 ? 1 : 0) + (cnt2[i] == 0 ? 1 : 0);
                        if (a == b) {
                            return true;
                        }
                    }
                }
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
    bool isItPossible(string word1, string word2) {
        int cnt1[26]{};
        int cnt2[26]{};
        int x = 0, y = 0;
        for (char& c : word1) {
            if (++cnt1[c - 'a'] == 1) {
                ++x;
            }
        }
        for (char& c : word2) {
            if (++cnt2[c - 'a'] == 1) {
                ++y;
            }
        }
        for (int i = 0; i < 26; ++i) {
            for (int j = 0; j < 26; ++j) {
                if (cnt1[i] > 0 && cnt2[j] > 0) {
                    if (i == j) {
                        if (x == y) {
                            return true;
                        }
                    } else {
                        int a = x - (cnt1[i] == 1 ? 1 : 0) + (cnt1[j] == 0 ? 1 : 0);
                        int b = y - (cnt2[j] == 1 ? 1 : 0) + (cnt2[i] == 0 ? 1 : 0);
                        if (a == b) {
                            return true;
                        }
                    }
                }
            }
        }
        return false;
    }
};
```

#### Go

```go
func isItPossible(word1 string, word2 string) bool {
	cnt1 := [26]int{}
	cnt2 := [26]int{}
	x, y := 0, 0
	for _, c := range word1 {
		cnt1[c-'a']++
		if cnt1[c-'a'] == 1 {
			x++
		}
	}
	for _, c := range word2 {
		cnt2[c-'a']++
		if cnt2[c-'a'] == 1 {
			y++
		}
	}
	for i := range cnt1 {
		for j := range cnt2 {
			if cnt1[i] > 0 && cnt2[j] > 0 {
				if i == j {
					if x == y {
						return true
					}
				} else {
					a := x
					if cnt1[i] == 1 {
						a--
					}
					if cnt1[j] == 0 {
						a++
					}

					b := y
					if cnt2[j] == 1 {
						b--
					}
					if cnt2[i] == 0 {
						b++
					}

					if a == b {
						return true
					}
				}
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function isItPossible(word1: string, word2: string): boolean {
    const cnt1: number[] = Array(26).fill(0);
    const cnt2: number[] = Array(26).fill(0);
    let [x, y] = [0, 0];

    for (const c of word1) {
        if (++cnt1[c.charCodeAt(0) - 'a'.charCodeAt(0)] === 1) {
            ++x;
        }
    }

    for (const c of word2) {
        if (++cnt2[c.charCodeAt(0) - 'a'.charCodeAt(0)] === 1) {
            ++y;
        }
    }

    for (let i = 0; i < 26; ++i) {
        for (let j = 0; j < 26; ++j) {
            if (cnt1[i] > 0 && cnt2[j] > 0) {
                if (i === j) {
                    if (x === y) {
                        return true;
                    }
                } else {
                    const a = x - (cnt1[i] === 1 ? 1 : 0) + (cnt1[j] === 0 ? 1 : 0);
                    const b = y - (cnt2[j] === 1 ? 1 : 0) + (cnt2[i] === 0 ? 1 : 0);
                    if (a === b) {
                        return true;
                    }
                }
            }
        }
    }

    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
