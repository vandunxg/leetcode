---
comments: true
difficulty: Medium
rating: 1312
source: Weekly Contest 496 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [3889. Mirror Frequency Distance](https://leetcode.com/problems/mirror-frequency-distance)

[中文文档](/solution/3800-3899/3889.Mirror%20Frequency%20Distance/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và chữ số.</p>

<p>Với mỗi ký tự, <strong>ký tự đối xứng</strong> được định nghĩa bằng cách đảo ngược thứ tự của tập ký tự tương ứng:</p>

<ul>
	<li>Với chữ cái, ký tự đối xứng là chữ cái ở cùng vị trí tính từ cuối bảng chữ cái.
	<ul>
		<li>Ví dụ, ký tự đối xứng của <code>&#39;a&#39;</code> là <code>&#39;z&#39;</code>, của <code>&#39;b&#39;</code> là <code>&#39;y&#39;</code>, và tiếp tục như vậy.</li>
	</ul>
	</li>
	<li>Với chữ số, ký tự đối xứng là chữ số ở cùng vị trí tính từ cuối đoạn <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>.
	<ul>
		<li>Ví dụ, ký tự đối xứng của <code>&#39;0&#39;</code> là <code>&#39;9&#39;</code>, của <code>&#39;1&#39;</code> là <code>&#39;8&#39;</code>, và tiếp tục như vậy.</li>
	</ul>
	</li>
</ul>

<p>Với mỗi ký tự <strong>khác nhau</strong> <code>c</code> trong chuỗi:</p>

<ul>
	<li>Gọi <code>m</code> là ký tự <strong>đối xứng</strong> của nó.</li>
	<li>Gọi <code>freq(x)</code> là số lần ký tự <code>x</code> xuất hiện trong chuỗi.</li>
	<li>Tính <strong>độ chênh lệch tuyệt đối</strong> giữa <strong>tần suất</strong> của chúng, được định nghĩa là: <code>|freq(c) - freq(m)|</code></li>
</ul>

<p>Các cặp đối xứng <code>(c, m)</code> và <code>(m, c)</code> là cùng một cặp và chỉ được tính <strong>một lần</strong>.</p>

<p>Trả về một số nguyên biểu thị tổng các giá trị trên mọi <strong>cặp đối xứng khác nhau</strong> như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;ab1z9&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mỗi cặp đối xứng:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>c</code></th>
			<th style="border: 1px solid black;"><code>m</code></th>
			<th style="border: 1px solid black;"><code>freq(c)</code></th>
			<th style="border: 1px solid black;"><code>freq(m)</code></th>
			<th style="border: 1px solid black;"><code>|freq(c) - freq(m)|</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">a</td>
			<td style="border: 1px solid black;">z</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">b</td>
			<td style="border: 1px solid black;">y</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">8</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">9</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Vậy đáp án là <code>0 + 1 + 1 + 1 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;4m7n&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>c</code></th>
			<th style="border: 1px solid black;"><code>m</code></th>
			<th style="border: 1px solid black;"><code>freq(c)</code></th>
			<th style="border: 1px solid black;"><code>freq(m)</code></th>
			<th style="border: 1px solid black;"><code>|freq(c) - freq(m)|</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">m</td>
			<td style="border: 1px solid black;">n</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Vậy đáp án là <code>1 + 0 + 1 = 2</code>.​​​​​​​</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;byby&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>c</code></th>
			<th style="border: 1px solid black;"><code>m</code></th>
			<th style="border: 1px solid black;"><code>freq(c)</code></th>
			<th style="border: 1px solid black;"><code>freq(m)</code></th>
			<th style="border: 1px solid black;"><code>|freq(c) - freq(m)|</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">b</td>
			<td style="border: 1px solid black;">y</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
	</tbody>
</table>

<p>Vậy đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Chữ cái và chữ số đối xứng trong tập ký tự riêng của chúng; ta tính tổng độ chênh lệch tần suất tuyệt đối trên các cặp đối xứng không có thứ tự. Chỉ cần một lần duyệt để đếm.
>
> $(c,m)$ và $(m,c)$ là cùng một cặp, nên cần đánh dấu các ký tự đã được xét.
>
> Đếm tần suất, sau đó với mỗi $c$ chưa được xét, cộng $|freq(c)-freq(m)|$ và đánh dấu $c$.
>
> Nếu ký tự đối xứng không xuất hiện, tần suất của nó được xem là $0$.

<!-- thinking:end -->

Trước tiên, ta dùng một hash table $\textit{freq}$ để đếm tần suất của từng ký tự trong chuỗi $s$.

Sau đó, ta duyệt qua từng cặp khóa-giá trị $(c, v)$ trong $\textit{freq}$, trong đó $c$ là ký tự và $v$ là số lần ký tự $c$ xuất hiện trong chuỗi $s$. Với mỗi ký tự $c$, ta tính ký tự đối xứng $m$ và tính $|freq(c) - freq(m)|$. Để tránh đếm một cặp đối xứng hai lần, ta dùng một hash set $\textit{vis}$ để theo dõi các ký tự đã được xét.

Cuối cùng, ta trả về tổng các độ chênh lệch tuyệt đối trên mọi cặp đối xứng khác nhau.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập các ký tự khác nhau xuất hiện trong chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mirrorFrequency(self, s: str) -> int:
        freq = Counter(s)
        ans = 0
        vis = set()
        for c, v in freq.items():
            m = (
                chr(ord("a") + 25 - (ord(c) - ord("a")))
                if c.isalpha()
                else str(9 - int(c))
            )
            if m in vis:
                continue
            vis.add(c)
            ans += abs(v - freq[m])
        return ans
```

#### Java

```java
class Solution {
    public int mirrorFrequency(String s) {
        Map<Character, Integer> freq = new HashMap<>();
        for (char c : s.toCharArray()) {
            freq.merge(c, 1, Integer::sum);
        }

        int ans = 0;
        Set<Character> vis = new HashSet<>();

        for (Map.Entry<Character, Integer> entry : freq.entrySet()) {
            char c = entry.getKey();
            int v = entry.getValue();

            char m;
            if (Character.isLetter(c)) {
                m = (char) ('a' + 25 - (c - 'a'));
            } else {
                m = (char) ('0' + (9 - (c - '0')));
            }

            if (vis.contains(m)) {
                continue;
            }
            vis.add(c);

            int mv = freq.getOrDefault(m, 0);
            ans += Math.abs(v - mv);
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int mirrorFrequency(string s) {
        unordered_map<char, int> freq;
        for (char c : s) {
            freq[c]++;
        }

        int ans = 0;
        unordered_set<char> vis;

        for (auto& [c, v] : freq) {
            char m;
            if (isalpha(c)) {
                m = 'a' + 25 - (c - 'a');
            } else {
                m = '0' + (9 - (c - '0'));
            }

            if (vis.count(m)) {
                continue;
            }
            vis.insert(c);

            int mv = freq.count(m) ? freq[m] : 0;
            ans += abs(v - mv);
        }

        return ans;
    }
};
```

#### Go

```go
func mirrorFrequency(s string) int {
	freq := make(map[rune]int)
	for _, c := range s {
		freq[c]++
	}

	ans := 0
	vis := make(map[rune]bool)

	for c, v := range freq {
		var m rune
		if c >= 'a' && c <= 'z' {
			m = 'a' + 25 - (c - 'a')
		} else {
			m = '0' + (9 - (c - '0'))
		}

		if vis[m] {
			continue
		}
		vis[c] = true

		mv := freq[m]
		ans += abs(v - mv)
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
function mirrorFrequency(s: string): number {
    const freq = new Map<string, number>();
    for (const c of s) {
        freq.set(c, (freq.get(c) || 0) + 1);
    }

    let ans = 0;
    const vis = new Set<string>();

    for (const [c, v] of freq.entries()) {
        let m: string;

        if (/[a-z]/.test(c)) {
            m = String.fromCharCode('a'.charCodeAt(0) + 25 - (c.charCodeAt(0) - 'a'.charCodeAt(0)));
        } else {
            m = String(9 - Number(c));
        }

        if (vis.has(m)) {
            continue;
        }
        vis.add(c);

        const mv = freq.get(m) || 0;
        ans += Math.abs(v - mv);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
