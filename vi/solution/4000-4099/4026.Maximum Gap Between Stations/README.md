---
comments: true
difficulty: Medium
rating: 1675
source: Weekly Contest 515 Q3
tags:
    - Greedy
    - Two Pointers
    - String
---

<!-- problem:start -->

# [4026. Maximum Gap Between Stations](https://leetcode.com/problems/maximum-gap-between-stations)

[中文文档](/solution/4000-4099/4026.Maximum%20Gap%20Between%20Stations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>skill</code> và <code>station</code> có độ dài lần lượt là <code>n</code> và <code>m</code>.</p>

<p><code>skill[i]</code> biểu thị kỹ năng của công nhân <code>i</code>, còn <code>station[j]</code> biểu thị kỹ năng được hỗ trợ bởi trạm <code>j</code>.</p>

<p>Bạn phải gán <strong>mọi</strong> công nhân vào một trạm <strong>riêng biệt</strong>. Gọi <code>j<sub>i</sub></code> là chỉ số của trạm được gán cho công nhân <code>i</code>. Một phép gán hợp lệ phải thỏa mãn:</p>

<ul>
	<li><code>station[j<sub>i</sub>] == skill[i]</code> với mọi <code>0 &lt;= i &lt; n</code>.</li>
	<li>Các chỉ số trạm được gán phải <strong>tăng nghiêm ngặt</strong> theo thứ tự công nhân, nghĩa là <code>j<sub>0</sub> &lt; j<sub>1</sub> &lt; ... &lt; j<sub>n - 1</sub></code>.</li>
</ul>

<p><strong>Khoảng cách</strong> của một phép gán là <strong>hiệu lớn nhất</strong> giữa chỉ số trạm được gán cho hai công nhân <strong>liên tiếp</strong>. Nói cách khác, đó là <code>max(j<sub>i</sub> - j<sub>i - 1</sub>)</code> với mọi <code>1 &lt;= i &lt; n</code>.</p>

<p>Nếu chỉ có một công nhân, khoảng cách bằng 0.</p>

<p>Trả về <strong>khoảng cách</strong> lớn nhất có thể có trong tất cả các phép gán hợp lệ. Đảm bảo rằng tồn tại <strong>ít nhất</strong> một phép gán hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">skill = &quot;aa&quot;, station = &quot;aaaa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Hai công nhân phải được gán vào hai trạm <code>&#39;a&#39;</code> khác nhau.</li>
	<li>Gán họ vào các trạm <code>[0, 3]</code> cho khoảng cách bằng 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">skill = &quot;xyz&quot;, station = &quot;xyzz&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Gán công nhân 0 vào trạm <code>j = 0</code>, và công nhân 1 vào trạm <code>j = 1</code>.</li>
	<li>Để tối đa hóa khoảng cách, gán công nhân 2 vào trạm <code>j = 3</code>.</li>
	<li>Phép gán này là <code>[0, 1, 3]</code> với các khoảng cách <code>[1, 2]</code>, nên khoảng cách là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">skill = &quot;cbc&quot;, station = &quot;cbcdbc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Gán công nhân 0 vào trạm <code>j = 0</code>, và công nhân 1 vào trạm <code>j = 1</code>.</li>
	<li>Để tối đa hóa khoảng cách, gán công nhân 2 vào trạm <code>j = 5</code>.</li>
	<li>Phép gán này là <code>[0, 1, 5]</code> với các khoảng cách <code>[1, 4]</code>, nên khoảng cách là 4.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>skill.length == n</code></li>
	<li><code>station.length == m</code></li>
	<li><code>1 &lt;= n &lt;= m &lt;= 10<sup>5</sup></code></li>
	<li><code>skill</code> và <code>station</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Đảm bảo tồn tại một phép gán hợp lệ cho mọi công nhân.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách lớn nhất chỉ có thể nằm giữa một cặp công nhân liên tiếp. Để nới rộng khoảng cách giữa $(i,i+1)$, các công nhân $0..i$ nên nhận những trạm khả dụng ngoài cùng bên trái, còn các công nhân $i+1..n-1$ nhận những trạm ngoài cùng bên phải, đồng thời vẫn phải khớp kỹ năng.
>
> Một lượt duyệt từ phải sang trái lưu trạm ngoài cùng bên phải mà công nhân $i$ có thể nhận sau khi các công nhân phía sau đã lấy những trạm bên phải hơn; sau đó một lượt duyệt từ trái sang phải gán công nhân $i$ vào vị trí khớp ngoài cùng bên trái và cập nhật đáp án bằng hiệu tương ứng.
>
> Một công nhân không tạo ra khoảng cách, nên đáp án là $0$.

<!-- thinking:end -->

Khoảng cách lớn nhất phải xuất hiện giữa một cặp công nhân liên tiếp nào đó $(i, i+1)$. Để tối đa hóa khoảng cách của cặp này, các công nhân $0, 1, \ldots, i$ nên được gán vào các trạm xa bên trái nhất có thể, còn các công nhân $i+1, \ldots, n-1$ được gán vào các trạm xa bên phải nhất có thể.

Vì vậy, ta duyệt từ phải sang trái và tính trước $\textit{suf}[i]$: trạm ngoài cùng bên phải mà công nhân $i$ có thể nhận, với giả sử các công nhân $i+1, \ldots, n-1$ chiếm những trạm còn xa bên phải hơn. Sau đó, ta duyệt từ trái sang phải, gán công nhân $i$ vào trạm khớp ngoài cùng bên trái hiện tại $\textit{pre}$, rồi cập nhật đáp án bằng $\textit{suf}[i+1] - \textit{pre}$.

Ta lấy giá trị lớn nhất trên tất cả các cặp liên tiếp. Nếu chỉ có một công nhân, đáp án là $0$.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ và $m$ lần lượt là độ dài của $\textit{skill}$ và $\textit{station}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumGap(self, skill: str, station: str) -> int:
        n, m = len(skill), len(station)
        suf = [0] * n
        j = m - 1
        for i in range(n - 1, 0, -1):
            while station[j] != skill[i]:
                j -= 1
            suf[i] = j
            j -= 1

        ans = pre = 0
        for i in range(n - 1):
            while station[pre] != skill[i]:
                pre += 1
            ans = max(ans, suf[i + 1] - pre)
            pre += 1
        return ans
```

#### Java

```java
class Solution {
    public int maximumGap(String skill, String station) {
        int n = skill.length();
        int m = station.length();

        int[] suf = new int[n];
        int j = m - 1;

        for (int i = n - 1; i > 0; i--) {
            while (station.charAt(j) != skill.charAt(i)) {
                j--;
            }

            suf[i] = j;
            j--;
        }

        int ans = 0;
        int pre = 0;

        for (int i = 0; i < n - 1; i++) {
            while (station.charAt(pre) != skill.charAt(i)) {
                pre++;
            }

            ans = Math.max(ans, suf[i + 1] - pre);
            pre++;
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumGap(string skill, string station) {
        int n = skill.size();
        int m = station.size();

        vector<int> suf(n);
        int j = m - 1;

        for (int i = n - 1; i > 0; i--) {
            while (station[j] != skill[i]) {
                j--;
            }

            suf[i] = j;
            j--;
        }

        int ans = 0;
        int pre = 0;

        for (int i = 0; i < n - 1; i++) {
            while (station[pre] != skill[i]) {
                pre++;
            }

            ans = max(ans, suf[i + 1] - pre);
            pre++;
        }

        return ans;
    }
};
```

#### Go

```go
func maximumGap(skill string, station string) int {
	n, m := len(skill), len(station)

	suf := make([]int, n)
	j := m - 1

	for i := n - 1; i > 0; i-- {
		for station[j] != skill[i] {
			j--
		}

		suf[i] = j
		j--
	}

	ans := 0
	pre := 0

	for i := 0; i < n-1; i++ {
		for station[pre] != skill[i] {
			pre++
		}

		ans = max(ans, suf[i+1]-pre)

		pre++
	}

	return ans
}
```

#### TypeScript

```ts
function maximumGap(skill: string, station: string): number {
    const n = skill.length;
    const m = station.length;

    const suf: number[] = Array(n).fill(0);
    let j = m - 1;

    for (let i = n - 1; i > 0; i--) {
        while (station[j] !== skill[i]) {
            j--;
        }

        suf[i] = j;
        j--;
    }

    let ans = 0;
    let pre = 0;

    for (let i = 0; i < n - 1; i++) {
        while (station[pre] !== skill[i]) {
            pre++;
        }

        ans = Math.max(ans, suf[i + 1] - pre);
        pre++;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
