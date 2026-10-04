---
comments: true
difficulty: Medium
rating: 1553
source: Biweekly Contest 144 Q2
tags:
    - Array
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [3361. Shift Distance Between Two Strings](https://leetcode.com/problems/shift-distance-between-two-strings)

[中文文档](/solution/3300-3399/3361.Shift%20Distance%20Between%20Two%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code> có cùng độ dài, cùng hai mảng số nguyên <code>nextCost</code> và <code>previousCost</code>.</p>

<p>Trong một thao tác, bạn có thể chọn một chỉ số <code>i</code> bất kỳ của <code>s</code> và thực hiện <strong>một trong hai</strong> hành động sau:</p>

<ul>
	<li>Dịch chuyển <code>s[i]</code> đến chữ cái tiếp theo trong bảng chữ cái. Nếu <code>s[i] == &#39;z&#39;</code>, hãy thay thế nó bằng <code>&#39;a&#39;</code>. Thao tác này có chi phí <code>nextCost[j]</code>, trong đó <code>j</code> là chỉ số của <code>s[i]</code> trong bảng chữ cái.</li>
	<li>Dịch chuyển <code>s[i]</code> đến chữ cái trước đó trong bảng chữ cái. Nếu <code>s[i] == &#39;a&#39;</code>, hãy thay thế nó bằng <code>&#39;z&#39;</code>. Thao tác này có chi phí <code>previousCost[j]</code>, trong đó <code>j</code> là chỉ số của <code>s[i]</code> trong bảng chữ cái.</li>
</ul>

<p><strong>Khoảng cách dịch chuyển</strong> là <strong>tổng chi phí nhỏ nhất</strong> của các thao tác cần thực hiện để biến đổi <code>s</code> thành <code>t</code>.</p>

<p>Trả về <strong>khoảng cách dịch chuyển</strong> từ <code>s</code> đến <code>t</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abab&quot;, t = &quot;baba&quot;, nextCost = [100,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0], previousCost = [1,100,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn chỉ số <code>i = 0</code> và dịch chuyển <code>s[0]</code> 25 lần về ký tự trước đó với tổng chi phí là 1.</li>
	<li>Chọn chỉ số <code>i = 1</code> và dịch chuyển <code>s[1]</code> 25 lần đến ký tự tiếp theo với tổng chi phí là 0.</li>
	<li>Chọn chỉ số <code>i = 2</code> và dịch chuyển <code>s[2]</code> 25 lần về ký tự trước đó với tổng chi phí là 1.</li>
	<li>Chọn chỉ số <code>i = 3</code> và dịch chuyển <code>s[3]</code> 25 lần đến ký tự tiếp theo với tổng chi phí là 0.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;leet&quot;, t = &quot;code&quot;, nextCost = [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1], previousCost = [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">31</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn chỉ số <code>i = 0</code> và dịch chuyển <code>s[0]</code> 9 lần về ký tự trước đó với tổng chi phí là 9.</li>
	<li>Chọn chỉ số <code>i = 1</code> và dịch chuyển <code>s[1]</code> 10 lần đến ký tự tiếp theo với tổng chi phí là 10.</li>
	<li>Chọn chỉ số <code>i = 2</code> và dịch chuyển <code>s[2]</code> 1 lần về ký tự trước đó với tổng chi phí là 1.</li>
	<li>Chọn chỉ số <code>i = 3</code> và dịch chuyển <code>s[3]</code> 11 lần đến ký tự tiếp theo với tổng chi phí là 11.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length == t.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>nextCost.length == previousCost.length == 26</code></li>
	<li><code>0 &lt;= nextCost[i], previousCost[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chữ cái có thể được dịch chuyển về phía trước hoặc phía sau trong bảng chữ cái với chi phí $\textit{nextCost}$ và $\textit{previousCost}$. Với $n \le 10^5$, cần tính chi phí cho mỗi ký tự trong $O(1)$ theo cả hai hướng.
>
> Nhân đôi các mảng chi phí và tính tổng tiền tố cho phép ta lấy chi phí của bất kỳ cung tròn nào.
>
> Chi phí đi tới là $s1[y]-s1[x]$ (cộng $26$ vào $y$ khi cần); chi phí đi lùi là công thức đối xứng. Ta cộng giá trị nhỏ hơn trong hai chi phí.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shiftDistance(
        self, s: str, t: str, nextCost: List[int], previousCost: List[int]
    ) -> int:
        m = 26
        s1 = [0] * (m << 1 | 1)
        s2 = [0] * (m << 1 | 1)
        for i in range(m << 1):
            s1[i + 1] = s1[i] + nextCost[i % m]
            s2[i + 1] = s2[i] + previousCost[(i + 1) % m]
        ans = 0
        for a, b in zip(s, t):
            x, y = ord(a) - ord("a"), ord(b) - ord("a")
            c1 = s1[y + m if y < x else y] - s1[x]
            c2 = s2[x + m if x < y else x] - s2[y]
            ans += min(c1, c2)
        return ans
```

#### Java

```java
class Solution {
    public long shiftDistance(String s, String t, int[] nextCost, int[] previousCost) {
        int m = 26;
        long[] s1 = new long[(m << 1) + 1];
        long[] s2 = new long[(m << 1) + 1];
        for (int i = 0; i < (m << 1); i++) {
            s1[i + 1] = s1[i] + nextCost[i % m];
            s2[i + 1] = s2[i] + previousCost[(i + 1) % m];
        }
        long ans = 0;
        for (int i = 0; i < s.length(); i++) {
            int x = s.charAt(i) - 'a';
            int y = t.charAt(i) - 'a';
            long c1 = s1[y + (y < x ? m : 0)] - s1[x];
            long c2 = s2[x + (x < y ? m : 0)] - s2[y];
            ans += Math.min(c1, c2);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long shiftDistance(string s, string t, vector<int>& nextCost, vector<int>& previousCost) {
        int m = 26;
        vector<long long> s1((m << 1) + 1);
        vector<long long> s2((m << 1) + 1);
        for (int i = 0; i < (m << 1); ++i) {
            s1[i + 1] = s1[i] + nextCost[i % m];
            s2[i + 1] = s2[i] + previousCost[(i + 1) % m];
        }

        long long ans = 0;
        for (int i = 0; i < s.size(); ++i) {
            int x = s[i] - 'a';
            int y = t[i] - 'a';
            long long c1 = s1[y + (y < x ? m : 0)] - s1[x];
            long long c2 = s2[x + (x < y ? m : 0)] - s2[y];
            ans += min(c1, c2);
        }

        return ans;
    }
};
```

#### Go

```go
func shiftDistance(s string, t string, nextCost []int, previousCost []int) (ans int64) {
	m := 26
	s1 := make([]int64, (m<<1)+1)
	s2 := make([]int64, (m<<1)+1)
	for i := 0; i < (m << 1); i++ {
		s1[i+1] = s1[i] + int64(nextCost[i%m])
		s2[i+1] = s2[i] + int64(previousCost[(i+1)%m])
	}
	for i := 0; i < len(s); i++ {
		x := int(s[i] - 'a')
		y := int(t[i] - 'a')
		z := y
		if y < x {
			z += m
		}
		c1 := s1[z] - s1[x]
		z = x
		if x < y {
			z += m
		}
		c2 := s2[z] - s2[y]
		ans += min(c1, c2)
	}
	return
}
```

#### TypeScript

```ts
function shiftDistance(s: string, t: string, nextCost: number[], previousCost: number[]): number {
    const m = 26;
    const s1: number[] = Array((m << 1) + 1).fill(0);
    const s2: number[] = Array((m << 1) + 1).fill(0);
    for (let i = 0; i < m << 1; i++) {
        s1[i + 1] = s1[i] + nextCost[i % m];
        s2[i + 1] = s2[i] + previousCost[(i + 1) % m];
    }
    let ans = 0;
    const a = 'a'.charCodeAt(0);
    for (let i = 0; i < s.length; i++) {
        const x = s.charCodeAt(i) - a;
        const y = t.charCodeAt(i) - a;
        const c1 = s1[y + (y < x ? m : 0)] - s1[x];
        const c2 = s2[x + (x < y ? m : 0)] - s2[y];
        ans += Math.min(c1, c2);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
