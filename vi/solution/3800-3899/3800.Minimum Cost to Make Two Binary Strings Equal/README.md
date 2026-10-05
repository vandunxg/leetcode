---
comments: true
difficulty: Medium
rating: 1840
source: Weekly Contest 483 Q3
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [3800. Minimum Cost to Make Two Binary Strings Equal](https://leetcode.com/problems/minimum-cost-to-make-two-binary-strings-equal)

[中文文档](/solution/3800-3899/3800.Minimum%20Cost%20to%20Make%20Two%20Binary%20Strings%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi nhị phân <code>s</code> và <code>t</code>, cả hai đều có độ dài <code>n</code>, cùng ba số nguyên <strong>dương</strong> <code>flipCost</code>, <code>swapCost</code> và <code>crossCost</code>.</p>

<p>Bạn được phép thực hiện các thao tác sau trên hai chuỗi <code>s</code> và <code>t</code> nhiều lần tùy ý (theo bất kỳ thứ tự nào):</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> bất kỳ và lật <code>s[i]</code> hoặc <code>t[i]</code> (đổi <code>&#39;0&#39;</code> thành <code>&#39;1&#39;</code> hoặc <code>&#39;1&#39;</code> thành <code>&#39;0&#39;</code>). Chi phí của thao tác này là <code>flipCost</code>.</li>
	<li>Chọn hai chỉ số <strong>phân biệt</strong> <code>i</code> và <code>j</code>, rồi đổi chỗ <code>s[i]</code> và <code>s[j]</code>, hoặc <code>t[i]</code> và <code>t[j]</code>. Chi phí của thao tác này là <code>swapCost</code>.</li>
	<li>Chọn một chỉ số <code>i</code> và đổi chỗ <code>s[i]</code> với <code>t[i]</code>. Chi phí của thao tác này là <code>crossCost</code>.</li>
</ul>

<p>Hãy trả về một số nguyên biểu thị tổng chi phí <strong>nhỏ nhất</strong> cần thiết để biến hai chuỗi <code>s</code> và <code>t</code> thành bằng nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;01000&quot;, t = &quot;10111&quot;, flipCost = 10, swapCost = 2, crossCost = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể thực hiện các thao tác sau:</p>

<ul>
	<li>Đổi chỗ <code>s[0]</code> và <code>s[1]</code> (<code>swapCost = 2</code>). Sau thao tác này, <code>s = &quot;10000&quot;</code> và <code>t = &quot;10111&quot;</code>.</li>
	<li>Đổi chỗ chéo <code>s[2]</code> và <code>t[2]</code> (<code>crossCost = 2</code>). Sau thao tác này, <code>s = &quot;10100&quot;</code> và <code>t = &quot;10011&quot;</code>.</li>
	<li>Đổi chỗ <code>s[2]</code> và <code>s[3]</code> (<code>swapCost = 2</code>). Sau thao tác này, <code>s = &quot;10010&quot;</code> và <code>t = &quot;10011&quot;</code>.</li>
	<li>Lật <code>s[4]</code> (<code>flipCost = 10</code>). Sau thao tác này, <code>s = t = &quot;10011&quot;</code>.</li>
</ul>

<p>Tổng chi phí là <code>2 + 2 + 2 + 10 = 16</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;001&quot;, t = &quot;110&quot;, flipCost = 2, swapCost = 100, crossCost = 100</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lật tất cả các bit của <code>s</code> sẽ làm hai chuỗi bằng nhau, với tổng chi phí là <code>3 * flipCost = 3 * 2 = 6</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1010&quot;, t = &quot;1010&quot;, flipCost = 5, swapCost = 5, crossCost = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hai chuỗi đã bằng nhau nên không cần thực hiện thao tác nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == s.length == t.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code>​​​​​​​</li>
	<li><code>1 &lt;= flipCost, swapCost, crossCost &lt;= 10<sup>9</sup></code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các ký tự <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ các vị trí mà $s$ và $t$ khác nhau mới cần xử lý. Với $n \le 10^5$, việc tìm kiếm các chuỗi thao tác là không khả thi.
>
> Các bit giống nhau có thể giữ nguyên. Các vị trí khác nhau thuộc một trong hai loại: $s[i]=\texttt{0}$ và $t[i]=\texttt{1}$, hoặc ngược lại. Gọi số lượng của chúng lần lượt là $d_0$ và $d_1$.
>
> Một phép lật sửa được bất kỳ vị trí khác nhau nào; phép đổi chỗ trong cùng một chuỗi ghép một vị trí thuộc loại này với một vị trí thuộc loại kia; còn đổi chỗ chéo thay đổi chênh lệch giữa hai số lượng. Vì vậy, đáp án là chi phí nhỏ nhất trong ba phương án: lật tất cả, ghép từng cặp rồi lật phần còn dư, hoặc cân bằng bằng các phép đổi chỗ chéo trước khi ghép cặp.
>
> Ta chỉ cần đếm hai loại vị trí khác nhau và so sánh ba công thức chi phí đóng, không cần mô phỏng từng thao tác.

<!-- thinking:end -->
<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(
        self, s: str, t: str, flipCost: int, swapCost: int, crossCost: int
    ) -> int:
        diff = [0] * 2
        for c1, c2 in zip(s, t):
            if c1 != c2:
                diff[int(c1)] += 1
        ans = (diff[0] + diff[1]) * flipCost
        mx = max(diff)
        mn = min(diff)
        ans = min(ans, mn * swapCost + (mx - mn) * flipCost)
        avg = (mx + mn) // 2
        ans = min(
            ans,
            (avg - mn) * crossCost + avg * swapCost + (mx + mn - avg * 2) * flipCost,
        )
        return ans
```

#### Java

```java
class Solution {
    public long minimumCost(String s, String t, int flipCost, int swapCost, int crossCost) {
        long[] diff = new long[2];
        int n = s.length();
        for (int i = 0; i < n; i++) {
            char c1 = s.charAt(i), c2 = t.charAt(i);
            if (c1 != c2) {
                diff[c1 - '0']++;
            }
        }

        long ans = (diff[0] + diff[1]) * flipCost;

        long mx = Math.max(diff[0], diff[1]);
        long mn = Math.min(diff[0], diff[1]);
        ans = Math.min(ans, mn * swapCost + (mx - mn) * flipCost);

        long avg = (mx + mn) / 2;
        ans = Math.min(
            ans, (avg - mn) * crossCost + avg * swapCost + (mx + mn - avg * 2) * flipCost);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumCost(string s, string t, int flipCost, int swapCost, int crossCost) {
        long long diff[2] = {0, 0};
        int n = s.size();
        for (int i = 0; i < n; i++) {
            if (s[i] != t[i]) {
                diff[s[i] - '0']++;
            }
        }

        long long ans = (diff[0] + diff[1]) * flipCost;

        long long mx = max(diff[0], diff[1]);
        long long mn = min(diff[0], diff[1]);
        ans = min(ans, mn * 1LL * swapCost + (mx - mn) * flipCost);

        long long avg = (mx + mn) / 2;
        ans = min(ans, (avg - mn) * crossCost + avg * swapCost + (mx + mn - avg * 2) * flipCost);

        return ans;
    }
};
```

#### Go

```go
func minimumCost(s string, t string, flipCost int, swapCost int, crossCost int) int64 {
	var diff [2]int64
	n := len(s)
	for i := 0; i < n; i++ {
		if s[i] != t[i] {
			diff[s[i]-'0']++
		}
	}

	ans := (diff[0] + diff[1]) * int64(flipCost)

	mx := max(diff[0], diff[1])
	mn := min(diff[0], diff[1])
	ans = min(ans, mn*int64(swapCost)+(mx-mn)*int64(flipCost))

	avg := (mx + mn) / 2
	ans = min(ans, (avg-mn)*int64(crossCost)+avg*int64(swapCost)+(mx+mn-avg*2)*int64(flipCost))

	return ans
}
```

#### TypeScript

```ts
function minimumCost(
    s: string,
    t: string,
    flipCost: number,
    swapCost: number,
    crossCost: number,
): number {
    const diff: number[] = [0, 0];
    const n = s.length;

    for (let i = 0; i < n; i++) {
        if (s[i] !== t[i]) {
            diff[s.charCodeAt(i) - 48]++;
        }
    }

    let ans = (diff[0] + diff[1]) * flipCost;

    const mx = Math.max(diff[0], diff[1]);
    const mn = Math.min(diff[0], diff[1]);
    ans = Math.min(ans, mn * swapCost + (mx - mn) * flipCost);

    const avg = (mx + mn) >> 1;
    ans = Math.min(ans, (avg - mn) * crossCost + avg * swapCost + (mx + mn - avg * 2) * flipCost);

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
