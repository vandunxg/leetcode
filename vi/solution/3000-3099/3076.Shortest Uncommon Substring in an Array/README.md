---
comments: true
difficulty: Medium
rating: 1635
source: Weekly Contest 388 Q3
tags:
    - Trie
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [3076. Shortest Uncommon Substring in an Array](https://leetcode.com/problems/shortest-uncommon-substring-in-an-array)

[中文文档](/solution/3000-3099/3076.Shortest%20Uncommon%20Substring%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>arr</code> kích thước <code>n</code>, gồm các chuỗi <strong>không rỗng</strong>.</p>

<p>Hãy tìm một mảng chuỗi <code>answer</code> kích thước <code>n</code> sao cho:</p>

<ul>
	<li><code>answer[i]</code> là <strong>chuỗi con</strong> <span data-keyword="substring">ngắn nhất</span> của <code>arr[i]</code> <strong>không</strong> xuất hiện dưới dạng chuỗi con trong bất kỳ chuỗi nào khác của <code>arr</code>. Nếu có nhiều chuỗi con như vậy, <code>answer[i]</code> phải là chuỗi con <span data-keyword="lexicographically-smaller-string">nhỏ nhất theo thứ tự từ điển</span>. Nếu không tồn tại chuỗi con phù hợp, <code>answer[i]</code> phải là chuỗi rỗng.</li>
</ul>

<p>Trả về <em>mảng </em><code>answer</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [&quot;cab&quot;,&quot;ad&quot;,&quot;bad&quot;,&quot;c&quot;]
<strong>Đầu ra:</strong> [&quot;ab&quot;,&quot;&quot;,&quot;ba&quot;,&quot;&quot;]
<strong>Giải thích:</strong> Ta có:
- Với chuỗi &quot;cab&quot;, chuỗi con ngắn nhất không xuất hiện trong bất kỳ chuỗi nào khác là &quot;ca&quot; hoặc &quot;ab&quot;, ta chọn chuỗi con nhỏ hơn theo thứ tự từ điển là &quot;ab&quot;.
- Với chuỗi &quot;ad&quot;, không có chuỗi con nào không xuất hiện trong bất kỳ chuỗi nào khác.
- Với chuỗi &quot;bad&quot;, chuỗi con ngắn nhất không xuất hiện trong bất kỳ chuỗi nào khác là &quot;ba&quot;.
- Với chuỗi &quot;c&quot;, không có chuỗi con nào không xuất hiện trong bất kỳ chuỗi nào khác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [&quot;abc&quot;,&quot;bcd&quot;,&quot;abcd&quot;]
<strong>Đầu ra:</strong> [&quot;&quot;,&quot;&quot;,&quot;abcd&quot;]
<strong>Giải thích:</strong>
- Với chuỗi &quot;abc&quot;, không có chuỗi con nào không xuất hiện trong bất kỳ chuỗi nào khác.
- Với chuỗi &quot;bcd&quot;, không có chuỗi con nào không xuất hiện trong bất kỳ chuỗi nào khác.
- Với chuỗi &quot;abcd&quot;, chuỗi con ngắn nhất không xuất hiện trong bất kỳ chuỗi nào khác là &quot;abcd&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == arr.length</code></li>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= arr[i].length &lt;= 20</code></li>
	<li><code>arr[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 100$ và $m \le 20$, nên mỗi chuỗi có $O(m^2)$ chuỗi con. Việc kiểm tra từng chuỗi con trong các chuỗi còn lại là chấp nhận được.
>
> Ta cần chuỗi ngắn nhất, và nếu có cùng độ dài thì chọn chuỗi nhỏ nhất theo thứ tự từ điển, nên ta liệt kê theo độ dài tăng dần rồi theo vị trí bắt đầu, và dừng ngay khi tìm được ứng viên.
>
> Ba vòng lặp tạo $\textit{sub}$ và giữ lại nó nếu không có chuỗi nào khác chứa nó.

<!-- thinking:end -->

Vì dữ liệu nhỏ, ta có thể liệt kê trực tiếp tất cả chuỗi con của từng chuỗi, sau đó xác định xem chuỗi con đó có xuất hiện trong các chuỗi khác hay không.

Cụ thể, trước tiên ta duyệt qua từng chuỗi `arr[i]`, sau đó duyệt độ dài $j$ của mỗi chuỗi con từ nhỏ đến lớn, rồi duyệt vị trí bắt đầu $l$ của chuỗi con. Khi đó, ta lấy được chuỗi con hiện tại bằng `sub = arr[i][l:l+j]`. Tiếp theo, ta kiểm tra xem `sub` có phải là chuỗi con của các chuỗi khác hay không. Nếu có, ta bỏ qua chuỗi con hiện tại; nếu không, ta cập nhật đáp án.

Độ phức tạp thời gian là $O(n^2 \times m^4)$, và độ phức tạp không gian là $O(m)$. Trong đó, $n$ là độ dài của mảng chuỗi `arr`, còn $m$ là độ dài lớn nhất của chuỗi. Trong bài toán này, $m \le 20$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestSubstrings(self, arr: List[str]) -> List[str]:
        ans = [""] * len(arr)
        for i, s in enumerate(arr):
            m = len(s)
            for j in range(1, m + 1):
                for l in range(m - j + 1):
                    sub = s[l : l + j]
                    if not ans[i] or ans[i] > sub:
                        if all(k == i or sub not in t for k, t in enumerate(arr)):
                            ans[i] = sub
                if ans[i]:
                    break
        return ans
```

#### Java

```java
class Solution {
    public String[] shortestSubstrings(String[] arr) {
        int n = arr.length;
        String[] ans = new String[n];
        Arrays.fill(ans, "");
        for (int i = 0; i < n; ++i) {
            int m = arr[i].length();
            for (int j = 1; j <= m && ans[i].isEmpty(); ++j) {
                for (int l = 0; l <= m - j; ++l) {
                    String sub = arr[i].substring(l, l + j);
                    if (ans[i].isEmpty() || sub.compareTo(ans[i]) < 0) {
                        boolean ok = true;
                        for (int k = 0; k < n && ok; ++k) {
                            if (k != i && arr[k].contains(sub)) {
                                ok = false;
                            }
                        }
                        if (ok) {
                            ans[i] = sub;
                        }
                    }
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
    vector<string> shortestSubstrings(vector<string>& arr) {
        int n = arr.size();
        vector<string> ans(n);
        for (int i = 0; i < n; ++i) {
            int m = arr[i].size();
            for (int j = 1; j <= m && ans[i].empty(); ++j) {
                for (int l = 0; l <= m - j; ++l) {
                    string sub = arr[i].substr(l, j);
                    if (ans[i].empty() || sub < ans[i]) {
                        bool ok = true;
                        for (int k = 0; k < n && ok; ++k) {
                            if (k != i && arr[k].find(sub) != string::npos) {
                                ok = false;
                            }
                        }
                        if (ok) {
                            ans[i] = sub;
                        }
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func shortestSubstrings(arr []string) []string {
	ans := make([]string, len(arr))
	for i, s := range arr {
		m := len(s)
		for j := 1; j <= m && len(ans[i]) == 0; j++ {
			for l := 0; l <= m-j; l++ {
				sub := s[l : l+j]
				if len(ans[i]) == 0 || ans[i] > sub {
					ok := true
					for k, t := range arr {
						if k != i && strings.Contains(t, sub) {
							ok = false
							break
						}
					}
					if ok {
						ans[i] = sub
					}
				}
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function shortestSubstrings(arr: string[]): string[] {
    const n: number = arr.length;
    const ans: string[] = Array(n).fill('');
    for (let i = 0; i < n; ++i) {
        const m: number = arr[i].length;
        for (let j = 1; j <= m && ans[i] === ''; ++j) {
            for (let l = 0; l <= m - j; ++l) {
                const sub: string = arr[i].slice(l, l + j);
                if (ans[i] === '' || sub.localeCompare(ans[i]) < 0) {
                    let ok: boolean = true;
                    for (let k = 0; k < n && ok; ++k) {
                        if (k !== i && arr[k].includes(sub)) {
                            ok = false;
                        }
                    }
                    if (ok) {
                        ans[i] = sub;
                    }
                }
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
