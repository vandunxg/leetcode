---
comments: true
difficulty: Medium
rating: 2042
source: Weekly Contest 442 Q3
tags:
    - Array
    - Prefix Sum
    - Simulation
---

<!-- problem:start -->

# [3494. Find the Minimum Amount of Time to Brew Potions](https://leetcode.com/problems/find-the-minimum-amount-of-time-to-brew-potions)

[中文文档](/solution/3400-3499/3494.Find%20the%20Minimum%20Amount%20of%20Time%20to%20Brew%20Potions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>skill</code> và <code><font face="monospace">mana</font></code>, có độ dài lần lượt là <code>n</code> và <code>m</code>.</p>

<p>Trong phòng thí nghiệm, <code>n</code> wizard phải pha <code>m</code> potion <em>theo đúng thứ tự</em>. Mỗi potion có dung lượng mana là <code>mana[j]</code> và <strong>bắt buộc</strong> phải đi qua <strong>tất cả</strong> wizard lần lượt để được pha chế đúng cách. Thời gian wizard thứ <code>i<sup>th</sup></code> xử lý potion thứ <code>j<sup>th</sup></code> là <code>time<sub>ij</sub> = skill[i] * mana[j]</code>.</p>

<p>Vì quy trình pha chế rất nhạy cảm, một potion <strong>phải</strong> được chuyển cho wizard tiếp theo ngay sau khi wizard hiện tại hoàn tất công việc. Điều này có nghĩa là thời gian phải được <em>đồng bộ</em> để mỗi wizard bắt đầu xử lý potion <strong>đúng</strong> lúc potion đến tay họ. ​</p>

<p>Hãy trả về lượng thời gian <strong>nhỏ nhất</strong> cần thiết để pha chế đúng tất cả potion.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">skill = [1,5,2,4], mana = [5,1,4,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">110</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Số thứ tự potion</th>
			<th style="border: 1px solid black;">Thời điểm bắt đầu</th>
			<th style="border: 1px solid black;">Wizard 0 hoàn tất trước thời điểm</th>
			<th style="border: 1px solid black;">Wizard 1 hoàn tất trước thời điểm</th>
			<th style="border: 1px solid black;">Wizard 2 hoàn tất trước thời điểm</th>
			<th style="border: 1px solid black;">Wizard 3 hoàn tất trước thời điểm</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">30</td>
			<td style="border: 1px solid black;">40</td>
			<td style="border: 1px solid black;">60</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">52</td>
			<td style="border: 1px solid black;">53</td>
			<td style="border: 1px solid black;">58</td>
			<td style="border: 1px solid black;">60</td>
			<td style="border: 1px solid black;">64</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">54</td>
			<td style="border: 1px solid black;">58</td>
			<td style="border: 1px solid black;">78</td>
			<td style="border: 1px solid black;">86</td>
			<td style="border: 1px solid black;">102</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">86</td>
			<td style="border: 1px solid black;">88</td>
			<td style="border: 1px solid black;">98</td>
			<td style="border: 1px solid black;">102</td>
			<td style="border: 1px solid black;">110</td>
		</tr>
	</tbody>
</table>

<p>Để thấy vì sao wizard 0 không thể bắt đầu xử lý potion 1<sup>st</sup> trước thời điểm <code>t = 52</code>, hãy xét trường hợp các wizard bắt đầu chuẩn bị potion 1<sup>st</sup> tại thời điểm <code>t = 50</code>. Đến thời điểm <code>t = 58</code>, wizard 2 đã hoàn tất potion 1<sup>st</sup>, nhưng wizard 3 vẫn còn đang xử lý potion 0<sup>th</sup> cho đến thời điểm <code>t = 60</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">skill = [1,1,1], mana = [1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ol>
	<li>Việc chuẩn bị potion 0<sup>th</sup> bắt đầu tại thời điểm <code>t = 0</code> và hoàn tất vào thời điểm <code>t = 3</code>.</li>
	<li>Việc chuẩn bị potion 1<sup>st</sup> bắt đầu tại thời điểm <code>t = 1</code> và hoàn tất vào thời điểm <code>t = 4</code>.</li>
	<li>Việc chuẩn bị potion 2<sup>nd</sup> bắt đầu tại thời điểm <code>t = 2</code> và hoàn tất vào thời điểm <code>t = 5</code>.</li>
</ol>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">skill = [1,2,3,4], mana = [1,2]</span></p>

<p><strong>Đầu ra:</strong> 21</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == skill.length</code></li>
	<li><code>m == mana.length</code></li>
	<li><code>1 &lt;= n, m &lt;= 5000</code></li>
	<li><code>1 &lt;= mana[i], skill[i] &lt;= 5000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Các wizard pha chế theo một pipeline; potion được chuyển ngay lập tức. $n,m\le 5000$. Trạng thái là thời điểm hoàn tất của mỗi wizard đối với potion trước đó.
>
> Potion hiện tại không thể bắt đầu trước khi wizard đó hoàn tất potion trước, cũng không thể bắt đầu trước khi wizard trước đó hoàn tất potion hiện tại. Duyệt tiến sẽ cho thời điểm hoàn tất của potion.
>
> Không có khoảng thời gian nhàn rỗi, nên thời điểm hoàn tất được tính ngược từ wizard cuối: $f[i]=f[i+1]-\textit{skill}[i+1]\cdot x$. Sau mỗi potion, $f[n-1]$ là đáp án.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là thời điểm wizard $i$ hoàn tất potion trước đó.

Với potion hiện tại $x$, ta cần tính thời điểm hoàn tất của mỗi wizard. Gọi $\textit{tot}$ là thời điểm hoàn tất potion hiện tại, ban đầu $\textit{tot} = 0$.

Với mỗi wizard $i$, thời điểm bắt đầu xử lý potion hiện tại là $\max(\textit{tot}, f[i])$, còn thời gian cần thiết để xử lý potion này là $skill[i] \times mana[x]$. Do đó, thời điểm wizard hoàn tất potion này là $\max(\textit{tot}, f[i]) + skill[i] \times mana[x]$. Ta cập nhật $\textit{tot}$ thành giá trị này.

Vì quy trình pha chế yêu cầu potion phải được chuyển ngay cho wizard tiếp theo và việc xử lý phải bắt đầu ngay sau khi wizard hiện tại hoàn tất, ta cần cập nhật thời điểm hoàn tất $f[i]$ của potion trước đó tại mỗi wizard. Với wizard cuối $n-1$, ta cập nhật trực tiếp $f[n-1]$ thành $\textit{tot}$. Với các wizard còn lại $i$, ta có thể cập nhật $f[i]$ bằng cách duyệt ngược, cụ thể là $f[i] = f[i+1] - skill[i+1] \times mana[x]$.

Cuối cùng, $f[n-1]$ là tổng thời gian nhỏ nhất cần thiết để hoàn tất việc pha chế tất cả potion.

Độ phức tạp thời gian là $O(n \times m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ và $m$ lần lượt là số wizard và số potion.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minTime(self, skill: List[int], mana: List[int]) -> int:
        max = lambda a, b: a if a > b else b
        n = len(skill)
        f = [0] * n
        for x in mana:
            tot = 0
            for i in range(n):
                tot = max(tot, f[i]) + skill[i] * x
            f[-1] = tot
            for i in range(n - 2, -1, -1):
                f[i] = f[i + 1] - skill[i + 1] * x
        return f[-1]
```

#### Java

```java
class Solution {
    public long minTime(int[] skill, int[] mana) {
        int n = skill.length;
        long[] f = new long[n];
        for (int x : mana) {
            long tot = 0;
            for (int i = 0; i < n; ++i) {
                tot = Math.max(tot, f[i]) + skill[i] * x;
            }
            f[n - 1] = tot;
            for (int i = n - 2; i >= 0; --i) {
                f[i] = f[i + 1] - skill[i + 1] * x;
            }
        }
        return f[n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minTime(vector<int>& skill, vector<int>& mana) {
        int n = skill.size();
        vector<long long> f(n);
        for (int x : mana) {
            long long tot = 0;
            for (int i = 0; i < n; ++i) {
                tot = max(tot, f[i]) + 1LL * skill[i] * x;
            }
            f[n - 1] = tot;
            for (int i = n - 2; i >= 0; --i) {
                f[i] = f[i + 1] - 1LL * skill[i + 1] * x;
            }
        }
        return f[n - 1];
    }
};
```

#### Go

```go
func minTime(skill []int, mana []int) int64 {
	n := len(skill)
	f := make([]int64, n)
	for _, x := range mana {
		var tot int64
		for i := 0; i < n; i++ {
			tot = max(tot, f[i]) + int64(skill[i])*int64(x)
		}
		f[n-1] = tot
		for i := n - 2; i >= 0; i-- {
			f[i] = f[i+1] - int64(skill[i+1])*int64(x)
		}
	}
	return f[n-1]
}
```

#### TypeScript

```ts
function minTime(skill: number[], mana: number[]): number {
    const n = skill.length;
    const f: number[] = Array(n).fill(0);
    for (const x of mana) {
        let tot = 0;
        for (let i = 0; i < n; ++i) {
            tot = Math.max(tot, f[i]) + skill[i] * x;
        }
        f[n - 1] = tot;
        for (let i = n - 2; i >= 0; --i) {
            f[i] = f[i + 1] - skill[i + 1] * x;
        }
    }
    return f[n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
