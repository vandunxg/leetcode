---
comments: true
difficulty: Easy
rating: 1384
source: Biweekly Contest 166 Q1
---

<!-- problem:start -->

# [3692. Majority Frequency Characters](https://leetcode.com/problems/majority-frequency-characters)

[中文文档](/solution/3600-3699/3692.Majority%20Frequency%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường.</p>

<p><strong>Nhóm tần suất</strong> ứng với một giá trị <code>k</code> là tập hợp các ký tự xuất hiện đúng <code>k</code> lần trong s.</p>

<p><strong>Nhóm tần suất đa số</strong> là nhóm tần suất chứa nhiều <strong>ký tự khác nhau</strong> nhất.</p>

<p>Trả về một chuỗi chứa tất cả các ký tự trong nhóm tần suất đa số, theo <strong>bất kỳ</strong> thứ tự nào. Nếu có từ hai nhóm tần suất trở lên có cùng kích thước lớn nhất, chọn nhóm có tần suất <code>k</code> <strong>lớn hơn</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aaabbbccdddde&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;ab&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Tần suất (k)</th>
			<th style="border: 1px solid black;">Các ký tự khác nhau trong nhóm</th>
			<th style="border: 1px solid black;">Kích thước nhóm</th>
			<th style="border: 1px solid black;">Đa số?</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">{d}</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Không</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">{a, b}</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><strong>Có</strong></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">{c}</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Không</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">{e}</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Không</td>
		</tr>
	</tbody>
</table>

<p>Cả hai ký tự <code>&#39;a&#39;</code> và <code>&#39;b&#39;</code> đều có cùng tần suất là 3, nên chúng thuộc nhóm tần suất đa số. <code>&quot;ba&quot;</code> cũng là một đáp án hợp lệ.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;abcd&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Tần suất (k)</th>
			<th style="border: 1px solid black;">Các ký tự khác nhau trong nhóm</th>
			<th style="border: 1px solid black;">Kích thước nhóm</th>
			<th style="border: 1px solid black;">Đa số?</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">{a, b, c, d}</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;"><strong>Có</strong></td>
		</tr>
	</tbody>
</table>

<p>Tất cả các ký tự đều có cùng tần suất là 1, nên tất cả đều thuộc nhóm tần suất đa số.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;pfpfgi&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;fp&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Tần suất (k)</th>
			<th style="border: 1px solid black;">Các ký tự khác nhau trong nhóm</th>
			<th style="border: 1px solid black;">Kích thước nhóm</th>
			<th style="border: 1px solid black;">Đa số?</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">{p, f}</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;"><strong>Có</strong></td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">{g, i}</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Không (cùng kích thước, tần suất nhỏ hơn)</td>
		</tr>
	</tbody>
</table>

<p>Cả hai ký tự <code>&#39;p&#39;</code> và <code>&#39;f&#39;</code> đều có cùng tần suất là 2, nên chúng thuộc nhóm tần suất đa số. Nhóm có tần suất 1 có cùng kích thước, nhưng ta chọn tần suất lớn hơn là 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Nhóm các ký tự theo tần suất, giữ lại nhóm lớn nhất, và khi có hòa thì giữ nhóm có tần suất lớn hơn. Chỉ cần hai map.
>
> $\textit{cnt}$ đếm số lần xuất hiện của các ký tự; $f[v]$ tập hợp các ký tự có số lần xuất hiện là $v$. Duyệt qua $f$ để theo dõi kích thước nhóm tốt nhất và tần suất tương ứng.
>
> Nối các ký tự trong nhóm đó. Thứ tự bên trong không quan trọng.

<!-- thinking:end -->

Trước tiên, ta dùng một mảng hoặc bảng băm $\textit{cnt}$ để đếm tần suất của từng ký tự trong chuỗi. Sau đó, ta dùng một bảng băm khác $\textit{f}$ để nhóm các ký tự có cùng tần suất $k$ vào cùng một danh sách, tức là $\textit{f}[k]$ lưu tất cả các ký tự có tần suất $k$.

Tiếp theo, ta duyệt qua bảng băm $\textit{f}$ để tìm nhóm tần suất có kích thước lớn nhất. Nếu nhiều nhóm tần suất có cùng kích thước lớn nhất, ta chọn nhóm có tần suất $k$ lớn hơn. Cuối cùng, ta nối tất cả các ký tự trong nhóm tần suất đó thành một chuỗi và trả về chuỗi này.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{s}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def majorityFrequencyGroup(self, s: str) -> str:
        cnt = Counter(s)
        f = defaultdict(list)
        for c, v in cnt.items():
            f[v].append(c)
        mx = mv = 0
        ans = []
        for v, cs in f.items():
            if mx < len(cs) or (mx == len(cs) and mv < v):
                mx = len(cs)
                mv = v
                ans = cs
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String majorityFrequencyGroup(String s) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        Map<Integer, StringBuilder> f = new HashMap<>();
        for (int i = 0; i < cnt.length; ++i) {
            if (cnt[i] > 0) {
                f.computeIfAbsent(cnt[i], k -> new StringBuilder()).append((char) ('a' + i));
            }
        }
        int mx = 0;
        int mv = 0;
        String ans = "";
        for (var e : f.entrySet()) {
            int v = e.getKey();
            var cs = e.getValue();
            if (mx < cs.length() || (mx == cs.length() && mv < v)) {
                mx = cs.length();
                mv = v;
                ans = cs.toString();
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
    string majorityFrequencyGroup(string s) {
        vector<int> cnt(26, 0);
        for (char c : s) {
            ++cnt[c - 'a'];
        }

        unordered_map<int, string> f;
        for (int i = 0; i < 26; ++i) {
            if (cnt[i] > 0) {
                f[cnt[i]].push_back('a' + i);
            }
        }

        int mx = 0, mv = 0;
        string ans;
        for (auto& e : f) {
            int v = e.first;
            string& cs = e.second;
            if (mx < (int) cs.size() || (mx == (int) cs.size() && mv < v)) {
                mx = cs.size();
                mv = v;
                ans = cs;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func majorityFrequencyGroup(s string) string {
	cnt := make([]int, 26)
	for _, c := range s {
		cnt[c-'a']++
	}

	f := make(map[int][]byte)
	for i, v := range cnt {
		if v > 0 {
			f[v] = append(f[v], byte('a'+i))
		}
	}

	mx, mv := 0, 0
	var ans []byte
	for v, cs := range f {
		if len(cs) > mx || (len(cs) == mx && v > mv) {
			mx = len(cs)
			mv = v
			ans = cs
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function majorityFrequencyGroup(s: string): string {
    const cnt: Record<string, number> = {};
    for (const c of s) {
        cnt[c] = (cnt[c] || 0) + 1;
    }
    const f = new Map<number, string[]>();
    for (const [c, v] of Object.entries(cnt)) {
        if (!f.has(v)) {
            f.set(v, []);
        }
        f.get(v)!.push(c);
    }
    let [mx, mv] = [0, 0];
    let ans = '';
    f.forEach((cs, v) => {
        if (mx < cs.length || (mx == cs.length && mv < v)) {
            mx = cs.length;
            mv = v;
            ans = cs.join('');
        }
    });
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
