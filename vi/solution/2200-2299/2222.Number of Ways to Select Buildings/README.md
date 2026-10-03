---
comments: true
difficulty: Medium
rating: 1656
source: Biweekly Contest 75 Q3
tags:
    - String
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [2222. Number of Ways to Select Buildings](https://leetcode.com/problems/number-of-ways-to-select-buildings)

[中文文档](/solution/2200-2299/2222.Number%20of%20Ways%20to%20Select%20Buildings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi nhị phân <code>s</code> được đánh chỉ số từ <strong>0</strong>, biểu diễn loại các tòa nhà dọc theo một con phố, trong đó:</p>

<ul>
	<li><code>s[i] = &#39;0&#39;</code> biểu thị tòa nhà thứ <code>i<sup>th</sup></code> là một văn phòng và</li>
	<li><code>s[i] = &#39;1&#39;</code> biểu thị tòa nhà thứ <code>i<sup>th</sup></code> là một nhà hàng.</li>
</ul>

<p>Là một quan chức thành phố, bạn muốn <strong>chọn</strong> 3 tòa nhà để kiểm tra ngẫu nhiên. Tuy nhiên, để đảm bảo tính đa dạng, <strong>không có hai</strong> tòa nhà liên tiếp nào trong số các tòa nhà <strong>được chọn</strong> có cùng loại.</p>

<ul>
	<li>Ví dụ, với <code>s = &quot;0<u><strong>0</strong></u>1<u><strong>1</strong></u>0<u><strong>1</strong></u>&quot;</code>, ta không thể chọn tòa nhà thứ <code>1<sup>st</sup></code>, thứ <code>3<sup>rd</sup></code> và thứ <code>5<sup>th</sup></code>, vì chúng tạo thành <code>&quot;0<strong><u>11</u></strong>&quot;</code>, không <strong>được phép</strong> do có hai tòa nhà liên tiếp cùng loại.</li>
</ul>

<p>Trả về <em><b>số cách hợp lệ</b></em> để chọn 3 tòa nhà.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;001101&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Các tập chỉ số được chọn sau đây là hợp lệ:
- [0,2,4] từ &quot;<u><strong>0</strong></u>0<strong><u>1</u></strong>1<strong><u>0</u></strong>1&quot; tạo thành &quot;010&quot;
- [0,3,4] từ &quot;<u><strong>0</strong></u>01<u><strong>10</strong></u>1&quot; tạo thành &quot;010&quot;
- [1,2,4] từ &quot;0<u><strong>01</strong></u>1<u><strong>0</strong></u>1&quot; tạo thành &quot;010&quot;
- [1,3,4] từ &quot;0<u><strong>0</strong></u>1<u><strong>10</strong></u>1&quot; tạo thành &quot;010&quot;
- [2,4,5] từ &quot;00<u><strong>1</strong></u>1<u><strong>01</strong></u>&quot; tạo thành &quot;101&quot;
- [3,4,5] từ &quot;001<u><strong>101</strong></u>&quot; tạo thành &quot;101&quot;
Không còn lựa chọn nào khác hợp lệ. Vì vậy, tổng cộng có 6 cách.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;11100&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Có thể chứng minh rằng không có lựa chọn hợp lệ nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta chọn ba chỉ số sao cho các màu của những tòa nhà được chọn liền kề nhau khác nhau, tức là các subsequence $010$ hoặc $101$. Vì $n \le 10^5$, không thể liệt kê tất cả các bộ ba. Cố định tòa nhà ở giữa: hai phía phải có màu đối lập, và tích của hai số lượng đó chính là số cách chọn.
>
> Đếm số tòa nhà của cả hai màu vào histogram bên phải $r$, rồi quét từ trái sang phải: loại tòa nhà hiện tại $x$ khỏi $r$, cộng $l[x\oplus 1]\times r[x\oplus 1]$ vào đáp án, sau đó tăng $l[x]$.

<!-- thinking:end -->

Theo mô tả bài toán, ta cần chọn $3$ tòa nhà sao cho hai tòa nhà liền kề không cùng loại.

Ta có thể liệt kê tòa nhà ở giữa, giả sử đó là $x$, khi đó loại của các tòa nhà ở bên trái và bên phải chỉ có thể là $x \oplus 1$, trong đó $\oplus$ là phép XOR. Vì vậy, ta có thể dùng hai mảng $l$ và $r$ để lần lượt ghi nhận số lượng tòa nhà của mỗi loại ở bên trái và bên phải. Sau đó, ta liệt kê tòa nhà ở giữa và tính đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfWays(self, s: str) -> int:
        l = [0, 0]
        r = [s.count("0"), s.count("1")]
        ans = 0
        for x in map(int, s):
            r[x] -= 1
            ans += l[x ^ 1] * r[x ^ 1]
            l[x] += 1
        return ans
```

#### Java

```java
class Solution {
    public long numberOfWays(String s) {
        int n = s.length();
        int[] l = new int[2];
        int[] r = new int[2];
        for (int i = 0; i < n; ++i) {
            r[s.charAt(i) - '0']++;
        }
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            int x = s.charAt(i) - '0';
            r[x]--;
            ans += 1L * l[x ^ 1] * r[x ^ 1];
            l[x]++;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long numberOfWays(string s) {
        int n = s.size();
        int l[2]{};
        int r[2]{};
        r[0] = ranges::count(s, '0');
        r[1] = n - r[0];
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            int x = s[i] - '0';
            r[x]--;
            ans += 1LL * l[x ^ 1] * r[x ^ 1];
            l[x]++;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfWays(s string) (ans int64) {
	n := len(s)
	l := [2]int{}
	r := [2]int{}
	r[0] = strings.Count(s, "0")
	r[1] = n - r[0]
	for _, c := range s {
		x := int(c - '0')
		r[x]--
		ans += int64(l[x^1] * r[x^1])
		l[x]++
	}
	return
}
```

#### TypeScript

```ts
function numberOfWays(s: string): number {
    const n = s.length;
    const l: number[] = [0, 0];
    const r: number[] = [s.split('').filter(c => c === '0').length, 0];
    r[1] = n - r[0];
    let ans: number = 0;
    for (const c of s) {
        const x = c === '0' ? 0 : 1;
        r[x]--;
        ans += l[x ^ 1] * r[x ^ 1];
        l[x]++;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
