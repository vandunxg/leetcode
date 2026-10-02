---
comments: true
difficulty: Hard
rating: 2362
source: Weekly Contest 198 Q3
tags:
    - Greedy
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [1520. Maximum Number of Non-Overlapping Substrings](https://leetcode.com/problems/maximum-number-of-non-overlapping-substrings)

[中文文档](/solution/1500-1599/1520.Maximum%20Number%20of%20Non-Overlapping%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> gồm các chữ cái thường, hãy tìm số lượng lớn nhất các mảng con <strong>không rỗng</strong> của <code>s</code> thỏa mãn các điều kiện sau:</p>

<ol>
<li>Các mảng con không giao nhau, nghĩa là với mọi hai mảng con <code>s[i..j]</code> và <code>s[x..y]</code>, hoặc <code>j &lt; x</code> hoặc <code>i &gt; y</code> đúng.</li>
<li>Mảng con chứa một ký tự <code>c</code> phải chứa tất cả các lần xuất hiện của <code>c</code>.</li>
</ol>

<p>Hãy tìm <em>số lượng lớn nhất các mảng con thỏa mãn điều kiện trên</em>. Nếu có nhiều nghiệm cùng số lượng mảng con, <em>hãy trả về nghiệm có tổng độ dài nhỏ nhất.</em> Có thể chứng minh rằng nghiệm có tổng độ dài nhỏ nhất là duy nhất.</p>

<p>Lưu ý rằng có thể trả về các mảng con theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;adefaddaccc&quot;
<strong>Output:</strong> [&quot;e&quot;,&quot;f&quot;,&quot;ccc&quot;]
<b>Giải thích:</b>&nbsp;Các mảng con sau đây là tất cả mảng con có thể thỏa mãn điều kiện:
[
&nbsp; &quot;adefaddaccc&quot;
&nbsp; &quot;adefadda&quot;,
&nbsp; &quot;ef&quot;,
&nbsp; &quot;e&quot;,
  &quot;f&quot;,
&nbsp; &quot;ccc&quot;,
]
Nếu chọn chuỗi đầu tiên, ta không thể chọn thêm gì và chỉ được 1 mảng con. Nếu chọn &quot;adefadda&quot;, phần còn lại là &quot;ccc&quot;, mảng con duy nhất không giao nhau, nên được 2 mảng con. Chọn &quot;ef&quot; cũng không tối ưu vì có thể tách thành hai. Do đó, cách tối ưu là chọn [&quot;e&quot;,&quot;f&quot;,&quot;ccc&quot;], thu được 3 mảng con. Không có nghiệm nào khác có cùng số lượng mảng con.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abbaccd&quot;
<strong>Output:</strong> [&quot;d&quot;,&quot;bb&quot;,&quot;cc&quot;]
<b>Giải thích: </b>Mặc dù tập mảng con [&quot;d&quot;,&quot;abba&quot;,&quot;cc&quot;] cũng có độ dài 3, nó bị xem là sai vì có tổng độ dài lớn hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> contains only lowercase English letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta muốn có càng nhiều mảng con không giao nhau càng tốt, mỗi mảng chứa mọi lần xuất hiện của các ký tự mà nó dùng. Vì $n\le 10^5$, không thể thử mọi mảng con.
>
> Mỗi ký tự có chỉ số đầu và cuối. Bắt đầu từ một biên trái, mở rộng đoạn đến lần xuất hiện ngoài cùng bên phải của mọi ký tự bên trong, ta thu được đoạn hợp lệ tối thiểu. Chọn tham lam các đoạn theo đầu phải sẽ tối đa hóa số lượng và tối thiểu hóa tổng độ dài.

<!-- thinking:end -->

Trước tiên, ta dùng mảng hoặc hash table $\textit{first}$ và $\textit{last}$ để ghi lại lần xuất hiện đầu tiên và cuối cùng của mỗi chữ cái trong $s$.

Tiếp theo, liệt kê từng chữ cái $c$ xuất hiện, lấy $\textit{first}[c]$ làm biên trái ứng viên $l$ và $\textit{last}[c]$ làm biên phải ban đầu $r$. Duyệt từ $l$ đến $r$. Với mỗi chữ cái $ch$ gặp được:

- Nếu $\textit{first}[ch] < l$, chữ cái này cũng xuất hiện trước $l$, nên đoạn hiện tại không thể bao phủ mọi lần xuất hiện của nó và không hợp lệ;
- Otherwise update $r = \max(r, \textit{last}[ch])$ to include every occurrence of $ch$.

Nếu duyệt thành công, ta thu được đoạn mảng con hợp lệ $[l, r]$.

Các đoạn hợp lệ chỉ có thể rời nhau hoặc lồng nhau, không bao giờ giao nhau một phần. Vì vậy, ta sắp xếp chúng theo đầu phải tăng dần và chọn tham lam: đặt $\textit{end}$ là đầu phải của đoạn đã chọn gần nhất, ban đầu bằng $-1$. Duyệt các đoạn đã sắp xếp từ trái sang phải; nếu đầu trái hiện tại lớn hơn $\textit{end}$, thêm mảng con tương ứng vào đáp án và cập nhật $\textit{end}$.

Sắp xếp theo đầu phải luôn chọn đoạn kết thúc sớm nhất, nên ta thu được số mảng con không giao nhau lớn nhất. Các đoạn lồng nhau ngắn hơn có đầu phải nhỏ hơn và được chọn trước, vì vậy tổng độ dài cũng nhỏ nhất.

Độ phức tạp thời gian là $O(n \times |\Sigma|)$ và độ phức tạp không gian là $O(|\Sigma|)$. Ở đây, $n$ là độ dài của $s$, còn $|\Sigma|$ là kích thước tập ký tự. Trong bài này, $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxNumOfSubstrings(self, s: str) -> List[str]:
        first, last = {}, {}
        for i, c in enumerate(s):
            if c not in first:
                first[c] = i
            last[c] = i
        segs = []
        for l in first.values():
            r, i = last[s[l]], l
            while i <= r:
                if first[s[i]] < l:
                    break
                r = max(r, last[s[i]])
                i += 1
            if i > r:
                segs.append((l, r))
        segs.sort(key=lambda x: x[1])
        ans, end = [], -1
        for l, r in segs:
            if l > end:
                ans.append(s[l : r + 1])
                end = r
        return ans
```

#### Java

```java
class Solution {
    public List<String> maxNumOfSubstrings(String s) {
        int n = s.length();
        int[] first = new int[26];
        int[] last = new int[26];
        Arrays.fill(first, -1);
        for (int i = 0; i < n; ++i) {
            int x = s.charAt(i) - 'a';
            if (first[x] == -1) {
                first[x] = i;
            }
            last[x] = i;
        }
        List<int[]> segs = new ArrayList<>();
        for (int x = 0; x < 26; ++x) {
            if (first[x] == -1) {
                continue;
            }
            int l = first[x], r = last[x];
            int i = l;
            for (; i <= r; ++i) {
                int y = s.charAt(i) - 'a';
                if (first[y] < l) {
                    break;
                }
                r = Math.max(r, last[y]);
            }
            if (i > r) {
                segs.add(new int[] {l, r});
            }
        }
        segs.sort((a, b) -> a[1] - b[1]);
        List<String> ans = new ArrayList<>();
        int end = -1;
        for (int[] e : segs) {
            int l = e[0], r = e[1];
            if (l > end) {
                ans.add(s.substring(l, r + 1));
                end = r;
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
    vector<string> maxNumOfSubstrings(string s) {
        int n = s.size();
        int first[26], last[26];
        memset(first, -1, sizeof(first));
        for (int i = 0; i < n; ++i) {
            int x = s[i] - 'a';
            if (first[x] == -1) {
                first[x] = i;
            }
            last[x] = i;
        }
        vector<pair<int, int>> segs;
        for (int x = 0; x < 26; ++x) {
            if (first[x] == -1) {
                continue;
            }
            int l = first[x], r = last[x];
            int i = l;
            for (; i <= r; ++i) {
                int y = s[i] - 'a';
                if (first[y] < l) {
                    break;
                }
                r = max(r, last[y]);
            }
            if (i > r) {
                segs.emplace_back(l, r);
            }
        }
        sort(segs.begin(), segs.end(), [](const auto& a, const auto& b) {
            return a.second < b.second;
        });
        vector<string> ans;
        int end = -1;
        for (auto [l, r] : segs) {
            if (l > end) {
                ans.emplace_back(s.substr(l, r - l + 1));
                end = r;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxNumOfSubstrings(s string) (ans []string) {
	first := [26]int{}
	last := [26]int{}
	for i := range first {
		first[i] = -1
	}
	for i := range s {
		x := int(s[i] - 'a')
		if first[x] == -1 {
			first[x] = i
		}
		last[x] = i
	}
	var segs [][2]int
	for x, l := range first {
		if l == -1 {
			continue
		}
		r := last[x]
		i := l
		for ; i <= r; i++ {
			y := int(s[i] - 'a')
			if first[y] < l {
				break
			}
			r = max(r, last[y])
		}
		if i > r {
			segs = append(segs, [2]int{l, r})
		}
	}
	sort.Slice(segs, func(i, j int) bool { return segs[i][1] < segs[j][1] })
	end := -1
	for _, e := range segs {
		l, r := e[0], e[1]
		if l > end {
			ans = append(ans, s[l:r+1])
			end = r
		}
	}
	return
}
```

#### TypeScript

```ts
function maxNumOfSubstrings(s: string): string[] {
    const n = s.length;
    const idx = (c: string) => c.charCodeAt(0) - 97;
    const first = Array(26).fill(-1);
    const last = Array(26).fill(0);
    for (let i = 0; i < n; ++i) {
        const x = idx(s[i]);
        if (first[x] === -1) {
            first[x] = i;
        }
        last[x] = i;
    }
    const segs: number[][] = [];
    for (let x = 0; x < 26; ++x) {
        if (first[x] === -1) {
            continue;
        }
        let l = first[x],
            r = last[x];
        let i = l;
        for (; i <= r; ++i) {
            const y = idx(s[i]);
            if (first[y] < l) {
                break;
            }
            r = Math.max(r, last[y]);
        }
        if (i > r) {
            segs.push([l, r]);
        }
    }
    segs.sort((a, b) => a[1] - b[1]);
    const ans: string[] = [];
    let end = -1;
    for (const [l, r] of segs) {
        if (l > end) {
            ans.push(s.slice(l, r + 1));
            end = r;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_num_of_substrings(s: String) -> Vec<String> {
        let n = s.len();
        let bs = s.as_bytes();
        let mut first = [-1; 26];
        let mut last = [0; 26];
        for i in 0..n {
            let x = (bs[i] - b'a') as usize;
            if first[x] == -1 {
                first[x] = i as i32;
            }
            last[x] = i as i32;
        }
        let mut segs = vec![];
        for x in 0..26 {
            if first[x] == -1 {
                continue;
            }
            let l = first[x];
            let mut r = last[x];
            let mut i = l;
            while i <= r {
                let y = (bs[i as usize] - b'a') as usize;
                if first[y] < l {
                    break;
                }
                r = r.max(last[y]);
                i += 1;
            }
            if i > r {
                segs.push((l, r));
            }
        }
        segs.sort_by_key(|&(_, r)| r);
        let mut ans = vec![];
        let mut end = -1;
        for (l, r) in segs {
            if l > end {
                ans.push(s[l as usize..=r as usize].to_string());
                end = r;
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public IList<string> MaxNumOfSubstrings(string s) {
        int n = s.Length;
        int[] first = new int[26];
        int[] last = new int[26];
        Array.Fill(first, -1);
        for (int i = 0; i < n; ++i) {
            int x = s[i] - 'a';
            if (first[x] == -1) {
                first[x] = i;
            }
            last[x] = i;
        }
        List<int[]> segs = new List<int[]>();
        for (int x = 0; x < 26; ++x) {
            if (first[x] == -1) {
                continue;
            }
            int l = first[x], r = last[x];
            int i = l;
            for (; i <= r; ++i) {
                int y = s[i] - 'a';
                if (first[y] < l) {
                    break;
                }
                r = Math.Max(r, last[y]);
            }
            if (i > r) {
                segs.Add(new int[] { l, r });
            }
        }
        segs.Sort((a, b) => a[1] - b[1]);
        IList<string> ans = new List<string>();
        int end = -1;
        foreach (var e in segs) {
            int l = e[0], r = e[1];
            if (l > end) {
                ans.Add(s.Substring(l, r - l + 1));
                end = r;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
