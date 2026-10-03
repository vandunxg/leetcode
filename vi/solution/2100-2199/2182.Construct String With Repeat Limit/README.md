---
comments: true
difficulty: Medium
rating: 1680
source: Weekly Contest 281 Q3
tags:
    - Greedy
    - Hash Table
    - String
    - Counting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2182. Construct String With Repeat Limit](https://leetcode.com/problems/construct-string-with-repeat-limit)

[中文文档](/solution/2100-2199/2182.Construct%20String%20With%20Repeat%20Limit/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên <code>repeatLimit</code>. Hãy tạo một chuỗi mới <code>repeatLimitedString</code> bằng các ký tự của <code>s</code> sao cho không có chữ cái nào xuất hiện <strong>quá</strong> <code>repeatLimit</code> lần <strong>liên tiếp</strong>. Bạn <strong>không</strong> cần sử dụng tất cả các ký tự trong <code>s</code>.</p>

<p>Trả về <em>chuỗi <strong>lớn nhất theo thứ tự từ điển</strong> </em><code>repeatLimitedString</code> <em>có thể tạo được</em>.</p>

<p>Một chuỗi <code>a</code> được gọi là <strong>lớn hơn theo thứ tự từ điển</strong> so với chuỗi <code>b</code> nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, chuỗi <code>a</code> có chữ cái xuất hiện sau chữ cái tương ứng trong <code>b</code> theo thứ tự bảng chữ cái. Nếu <code>min(a.length, b.length)</code> ký tự đầu tiên không khác nhau, thì chuỗi dài hơn là chuỗi lớn hơn theo thứ tự từ điển.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;cczazcc&quot;, repeatLimit = 3
<strong>Đầu ra:</strong> &quot;zzcccac&quot;
<strong>Giải thích:</strong> Ta sử dụng tất cả các ký tự trong s để tạo chuỗi repeatLimitedString &quot;zzcccac&quot;.
Chữ cái &#39;a&#39; xuất hiện liên tiếp nhiều nhất 1 lần.
Chữ cái &#39;c&#39; xuất hiện liên tiếp nhiều nhất 3 lần.
Chữ cái &#39;z&#39; xuất hiện liên tiếp nhiều nhất 2 lần.
Do đó, không có chữ cái nào xuất hiện liên tiếp quá repeatLimit lần và chuỗi là một repeatLimitedString hợp lệ.
Chuỗi này là repeatLimitedString lớn nhất theo thứ tự từ điển có thể tạo được, nên ta trả về &quot;zzcccac&quot;.
Lưu ý rằng chuỗi &quot;zzcccca&quot; lớn hơn theo thứ tự từ điển, nhưng chữ cái &#39;c&#39; xuất hiện liên tiếp quá 3 lần nên đây không phải là một repeatLimitedString hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aababab&quot;, repeatLimit = 2
<strong>Đầu ra:</strong> &quot;bbabaa&quot;
<strong>Giải thích:</strong> Ta chỉ sử dụng một số ký tự trong s để tạo chuỗi repeatLimitedString &quot;bbabaa&quot;.
Chữ cái &#39;a&#39; xuất hiện liên tiếp nhiều nhất 2 lần.
Chữ cái &#39;b&#39; xuất hiện liên tiếp nhiều nhất 2 lần.
Do đó, không có chữ cái nào xuất hiện liên tiếp quá repeatLimit lần và chuỗi là một repeatLimitedString hợp lệ.
Chuỗi này là repeatLimitedString lớn nhất theo thứ tự từ điển có thể tạo được, nên ta trả về &quot;bbabaa&quot;.
Lưu ý rằng chuỗi &quot;bbabaaa&quot; lớn hơn theo thứ tự từ điển, nhưng chữ cái &#39;a&#39; xuất hiện liên tiếp quá 2 lần nên đây không phải là một repeatLimitedString hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= repeatLimit &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Sử dụng nhiều ký tự nhất có thể theo thứ tự từ điển giảm dần, nhưng không để xuất hiện quá $\textit{repeatLimit}$ chữ cái giống nhau liên tiếp. Luôn thêm chữ cái lớn nhất còn lại; khi đạt đến giới hạn, chèn một chữ cái nhỏ hơn.
>
> Đếm $26$ chữ cái và duyệt từ `z` đến `a`. Con trỏ $j$ theo dõi một chữ cái nhỏ hơn vẫn còn lại. Thêm tối đa $\textit{repeatLimit}$ bản sao của $i$, sau đó thêm một $j$ nếu $i$ vẫn còn ký tự.
>
> $j$ chỉ di chuyển sang trái, vì vậy chi phí là tuyến tính theo độ dài chuỗi và kích thước bảng chữ cái.

<!-- thinking:end -->

Đầu tiên, ta sử dụng một mảng $cnt$ có độ dài $26$ để đếm số lần xuất hiện của mỗi ký tự trong chuỗi $s$. Sau đó, ta duyệt chữ cái thứ $i$ trong bảng chữ cái theo thứ tự giảm dần, mỗi lần lấy ra nhiều nhất $\min(cnt[i], repeatLimit)$ ký tự $i$. Nếu sau khi lấy ra mà $cnt[i]$ vẫn lớn hơn $0$, ta tiếp tục lấy chữ cái thứ $j$, trong đó $j$ là chỉ số lớn nhất thỏa mãn $j < i$ và $cnt[j] > 0$, cho đến khi không còn ký tự nào cần lấy.

Độ phức tạp thời gian là $O(n + |\Sigma|)$, và độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài của chuỗi $s$, còn $\Sigma$ là tập ký tự. Trong bài toán này, $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def repeatLimitedString(self, s: str, repeatLimit: int) -> str:
        cnt = [0] * 26
        for c in s:
            cnt[ord(c) - ord("a")] += 1
        ans = []
        j = 24
        for i in range(25, -1, -1):
            j = min(i - 1, j)
            while 1:
                x = min(repeatLimit, cnt[i])
                cnt[i] -= x
                ans.append(ascii_lowercase[i] * x)
                if cnt[i] == 0:
                    break
                while j >= 0 and cnt[j] == 0:
                    j -= 1
                if j < 0:
                    break
                cnt[j] -= 1
                ans.append(ascii_lowercase[j])
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String repeatLimitedString(String s, int repeatLimit) {
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        StringBuilder ans = new StringBuilder();
        for (int i = 25, j = 24; i >= 0; --i) {
            j = Math.min(j, i - 1);
            while (true) {
                for (int k = Math.min(cnt[i], repeatLimit); k > 0; --k) {
                    ans.append((char) ('a' + i));
                    --cnt[i];
                }
                if (cnt[i] == 0) {
                    break;
                }
                while (j >= 0 && cnt[j] == 0) {
                    --j;
                }
                if (j < 0) {
                    break;
                }
                ans.append((char) ('a' + j));
                --cnt[j];
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string repeatLimitedString(string s, int repeatLimit) {
        int cnt[26]{};
        for (char& c : s) {
            ++cnt[c - 'a'];
        }
        string ans;
        for (int i = 25, j = 24; ~i; --i) {
            j = min(j, i - 1);
            while (1) {
                for (int k = min(cnt[i], repeatLimit); k; --k) {
                    ans += 'a' + i;
                    --cnt[i];
                }
                if (cnt[i] == 0) {
                    break;
                }
                while (j >= 0 && cnt[j] == 0) {
                    --j;
                }
                if (j < 0) {
                    break;
                }
                ans += 'a' + j;
                --cnt[j];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func repeatLimitedString(s string, repeatLimit int) string {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	var ans []byte
	for i, j := 25, 24; i >= 0; i-- {
		j = min(j, i-1)
		for {
			for k := min(cnt[i], repeatLimit); k > 0; k-- {
				ans = append(ans, byte(i+'a'))
				cnt[i]--
			}
			if cnt[i] == 0 {
				break
			}
			for j >= 0 && cnt[j] == 0 {
				j--
			}
			if j < 0 {
				break
			}
			ans = append(ans, byte(j+'a'))
			cnt[j]--
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function repeatLimitedString(s: string, repeatLimit: number): string {
    const cnt: number[] = Array(26).fill(0);
    for (const c of s) {
        cnt[c.charCodeAt(0) - 97]++;
    }
    const ans: string[] = [];
    for (let i = 25, j = 24; ~i; --i) {
        j = Math.min(j, i - 1);
        while (true) {
            for (let k = Math.min(cnt[i], repeatLimit); k; --k) {
                ans.push(String.fromCharCode(97 + i));
                --cnt[i];
            }
            if (!cnt[i]) {
                break;
            }
            while (j >= 0 && !cnt[j]) {
                --j;
            }
            if (j < 0) {
                break;
            }
            ans.push(String.fromCharCode(97 + j));
            --cnt[j];
        }
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
