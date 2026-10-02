---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - Two Pointers
    - String
    - Sliding Window
---

<!-- problem:start -->

# [567. Permutation in String](https://leetcode.com/problems/permutation-in-string)

[中文文档](/solution/0500-0599/0567.Permutation%20in%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s1</code> và <code>s2</code>, hãy trả về <code>true</code> nếu <code>s2</code> chứa một <span data-keyword="permutation-string">hoán vị</span> của <code>s1</code>, ngược lại trả về <code>false</code>.</p>

<p>Nói cách khác, trả về <code>true</code> nếu một trong các hoán vị của <code>s1</code> là chuỗi con của <code>s2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;ab&quot;, s2 = &quot;eidbaooo&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> s2 chứa một hoán vị của s1 (&quot;ba&quot;).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;ab&quot;, s2 = &quot;eidboaoo&quot;
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length, s2.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s1</code> và <code>s2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Một hoán vị của $s1$ là cửa sổ có độ dài $|s1|$ với số lần xuất hiện của từng ký tự giống nhau. Sắp xếp lại từng cửa sổ sẽ tốn công không cần thiết.
>
> Sliding window lưu số lượng ký tự còn thiếu so với $s1$, còn $\textit{need}$ là số ký tự vẫn chưa khớp. Thêm ký tự ở đầu phải, bỏ ký tự ở đầu trái và thành công khi $\textit{need}=0$. Cửa sổ có độ dài cố định và chỉ cần một lượt duyệt.

<!-- thinking:end -->

Ta dùng mảng $\textit{cnt}$ để ghi lại các ký tự cần khớp và số lần xuất hiện của chúng, đồng thời dùng biến $\textit{need}$ để ghi số ký tự khác nhau vẫn cần khớp. Ban đầu, $\textit{cnt}$ chứa số lần xuất hiện của các ký tự trong chuỗi $\textit{s1}$, còn $\textit{need}$ là số ký tự khác nhau trong $\textit{s1}$.

Sau đó, ta duyệt chuỗi $\textit{s2}$. Với mỗi ký tự, ta giảm giá trị tương ứng trong $\textit{cnt}$. Nếu giá trị sau khi giảm bằng $0$, nghĩa là số lần xuất hiện của ký tự hiện tại trong $\textit{s1}$ đã được đáp ứng, nên ta giảm $\textit{need}$. Nếu chỉ số hiện tại $i$ lớn hơn hoặc bằng độ dài của $\textit{s1}$, ta cần tăng giá trị tương ứng trong $\textit{cnt}$ cho $\textit{s2}[i-\textit{s1}]$. Nếu giá trị sau khi tăng bằng $1$, nghĩa là số lần xuất hiện của ký tự hiện tại trong $\textit{s1}$ không còn được đáp ứng, nên ta tăng $\textit{need}$. Trong quá trình duyệt, nếu $\textit{need}$ bằng $0$, nghĩa là số lần xuất hiện của mọi ký tự đều khớp và ta đã tìm thấy chuỗi con hợp lệ, nên trả về $\text{true}$.

Nếu duyệt xong mà không tìm thấy chuỗi con hợp lệ, ta trả về $\text{false}$.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của chuỗi $\textit{s1}$ và $\textit{s2}$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự. Trong bài này, tập ký tự gồm các chữ cái viết thường nên độ phức tạp không gian là hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        cnt = Counter(s1)
        need = len(cnt)
        m = len(s1)
        for i, c in enumerate(s2):
            cnt[c] -= 1
            if cnt[c] == 0:
                need -= 1
            if i >= m:
                cnt[s2[i - m]] += 1
                if cnt[s2[i - m]] == 1:
                    need += 1
            if need == 0:
                return True
        return False
```

#### Java

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int need = 0;
        int[] cnt = new int[26];
        for (char c : s1.toCharArray()) {
            if (++cnt[c - 'a'] == 1) {
                ++need;
            }
        }
        int m = s1.length(), n = s2.length();
        for (int i = 0; i < n; ++i) {
            int c = s2.charAt(i) - 'a';
            if (--cnt[c] == 0) {
                --need;
            }
            if (i >= m) {
                c = s2.charAt(i - m) - 'a';
                if (++cnt[c] == 1) {
                    ++need;
                }
            }
            if (need == 0) {
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
    bool checkInclusion(string s1, string s2) {
        int need = 0;
        int cnt[26]{};
        for (char c : s1) {
            if (++cnt[c - 'a'] == 1) {
                ++need;
            }
        }
        int m = s1.size(), n = s2.size();
        for (int i = 0; i < n; ++i) {
            int c = s2[i] - 'a';
            if (--cnt[c] == 0) {
                --need;
            }
            if (i >= m) {
                c = s2[i - m] - 'a';
                if (++cnt[c] == 1) {
                    ++need;
                }
            }
            if (need == 0) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func checkInclusion(s1 string, s2 string) bool {
	need := 0
	cnt := [26]int{}

	for _, c := range s1 {
		if cnt[c-'a']++; cnt[c-'a'] == 1 {
			need++
		}
	}

	m, n := len(s1), len(s2)
	for i := 0; i < n; i++ {
		c := s2[i] - 'a'
		if cnt[c]--; cnt[c] == 0 {
			need--
		}
		if i >= m {
			c = s2[i-m] - 'a'
			if cnt[c]++; cnt[c] == 1 {
				need++
			}
		}
		if need == 0 {
			return true
		}
	}
	return false
}
```

#### TypeScript

```ts
function checkInclusion(s1: string, s2: string): boolean {
    let need = 0;
    const cnt: number[] = Array(26).fill(0);
    const a = 'a'.charCodeAt(0);
    for (const c of s1) {
        if (++cnt[c.charCodeAt(0) - a] === 1) {
            need++;
        }
    }

    const [m, n] = [s1.length, s2.length];
    for (let i = 0; i < n; i++) {
        let c = s2.charCodeAt(i) - a;
        if (--cnt[c] === 0) {
            need--;
        }
        if (i >= m) {
            c = s2.charCodeAt(i - m) - a;
            if (++cnt[c] === 1) {
                need++;
            }
        }
        if (need === 0) {
            return true;
        }
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn check_inclusion(s1: String, s2: String) -> bool {
        let mut need = 0;
        let mut cnt = vec![0; 26];

        for c in s1.chars() {
            let index = (c as u8 - b'a') as usize;
            if cnt[index] == 0 {
                need += 1;
            }
            cnt[index] += 1;
        }

        let m = s1.len();
        let n = s2.len();
        let s2_bytes = s2.as_bytes();

        for i in 0..n {
            let c = (s2_bytes[i] - b'a') as usize;
            cnt[c] -= 1;
            if cnt[c] == 0 {
                need -= 1;
            }

            if i >= m {
                let c = (s2_bytes[i - m] - b'a') as usize;
                cnt[c] += 1;
                if cnt[c] == 1 {
                    need += 1;
                }
            }

            if need == 0 {
                return true;
            }
        }

        false
    }
}
```

#### C#

```cs
public class Solution {
    public bool CheckInclusion(string s1, string s2) {
        int need = 0;
        int[] cnt = new int[26];

        foreach (char c in s1) {
            if (++cnt[c - 'a'] == 1) {
                need++;
            }
        }

        int m = s1.Length, n = s2.Length;
        for (int i = 0; i < n; i++) {
            int c = s2[i] - 'a';
            if (--cnt[c] == 0) {
                need--;
            }

            if (i >= m) {
                c = s2[i - m] - 'a';
                if (++cnt[c] == 1) {
                    need++;
                }
            }

            if (need == 0) {
                return true;
            }
        }
        return false;
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s1
     * @param String $s2
     * @return Boolean
     */
    function checkInclusion($s1, $s2) {
        $need = 0;
        $cnt = array_fill(0, 26, 0);

        for ($i = 0; $i < strlen($s1); $i++) {
            $index = ord($s1[$i]) - ord('a');
            if (++$cnt[$index] == 1) {
                $need++;
            }
        }

        $m = strlen($s1);
        $n = strlen($s2);

        for ($i = 0; $i < $n; $i++) {
            $c = ord($s2[$i]) - ord('a');
            if (--$cnt[$c] == 0) {
                $need--;
            }

            if ($i >= $m) {
                $c = ord($s2[$i - $m]) - ord('a');
                if (++$cnt[$c] == 1) {
                    $need++;
                }
            }

            if ($need == 0) {
                return true;
            }
        }

        return false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
