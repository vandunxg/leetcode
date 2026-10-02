---
comments: true
difficulty: Medium
rating: 1438
source: Weekly Contest 179 Q2
tags:
    - Array
---

<!-- problem:start -->

# [1375. Number of Times Binary String Is Prefix-Aligned](https://leetcode.com/problems/number-of-times-binary-string-is-prefix-aligned)

[中文文档](/solution/1300-1399/1375.Number%20of%20Times%20Binary%20String%20Is%20Prefix-Aligned/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi nhị phân có độ dài <code>n</code> với chỉ số bắt đầu từ 1; ban đầu tất cả bit đều là <code>0</code>. Ta sẽ lần lượt lật các bit của chuỗi (tức đổi từ <code>0</code> thành <code>1</code>). Cho mảng số nguyên <code>flips</code> đánh chỉ số từ 1, trong đó <code>flips[i]</code> cho biết bit tại chỉ số <code>flips[i]</code> sẽ được lật ở bước thứ <code>i</code>.</p>

<p>Chuỗi nhị phân được gọi là <strong>prefix-aligned</strong> nếu sau bước thứ <code>i</code>, tất cả bit trong đoạn <strong>bao gồm cả hai đầu mút</strong> <code>[1, i]</code> đều bằng 1 và các bit còn lại đều bằng 0.</p>

<p>Trả về <em>số lần chuỗi nhị phân ở trạng thái <strong>prefix-aligned</strong> trong quá trình lật bit</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> flips = [3,2,4,1,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ban đầu chuỗi nhị phân là &quot;00000&quot;.
Sau bước 1: Chuỗi trở thành &quot;00100&quot;, chưa prefix-aligned.
Sau bước 2: Chuỗi trở thành &quot;01100&quot;, chưa prefix-aligned.
Sau bước 3: Chuỗi trở thành &quot;01110&quot;, chưa prefix-aligned.
Sau bước 4: Chuỗi trở thành &quot;11110&quot;, đạt trạng thái prefix-aligned.
Sau bước 5: Chuỗi trở thành &quot;11111&quot;, đạt trạng thái prefix-aligned.
Như vậy, chuỗi đạt trạng thái prefix-aligned 2 lần, nên ta trả về 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> flips = [4,1,2,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ban đầu chuỗi nhị phân là &quot;0000&quot;.
Sau bước 1: Chuỗi trở thành &quot;0001&quot;, chưa prefix-aligned.
Sau bước 2: Chuỗi trở thành &quot;1001&quot;, chưa prefix-aligned.
Sau bước 3: Chuỗi trở thành &quot;1101&quot;, chưa prefix-aligned.
Sau bước 4: Chuỗi trở thành &quot;1111&quot;, đạt trạng thái prefix-aligned.
Như vậy, chuỗi đạt trạng thái prefix-aligned 1 lần, nên ta trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == flips.length</code></li>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>flips</code> là một hoán vị của các số nguyên trong đoạn <code>[1, n]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> $flips$ là hoán vị của $1..n$; ở bước $i$, bit $flips[i]$ được bật. Prefix $[1..i]$ toàn bit 1 khi và chỉ khi giá trị lớn nhất trong $i$ lần lật đầu tiên bằng $i$. Theo dõi giá trị lớn nhất này và so sánh với $i$ để đếm số lần thỏa mãn.

<!-- thinking:end -->

Ta duyệt mảng $flips$ và theo dõi giá trị lớn nhất $mx$ trong các phần tử đã gặp. Nếu $mx$ bằng chỉ số hiện tại $i$, điều đó có nghĩa là cả $i$ bit đầu tiên đều đã được lật, tức prefix đang căn chỉnh; ta tăng đáp án lên 1.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng $flips$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numTimesAllBlue(self, flips: List[int]) -> int:
        ans = mx = 0
        for i, x in enumerate(flips, 1):
            mx = max(mx, x)
            ans += mx == i
        return ans
```

#### Java

```java
class Solution {
    public int numTimesAllBlue(int[] flips) {
        int ans = 0, mx = 0;
        for (int i = 1; i <= flips.length; ++i) {
            mx = Math.max(mx, flips[i - 1]);
            if (mx == i) {
                ++ans;
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
    int numTimesAllBlue(vector<int>& flips) {
        int ans = 0, mx = 0;
        for (int i = 1; i <= flips.size(); ++i) {
            mx = max(mx, flips[i - 1]);
            ans += mx == i;
        }
        return ans;
    }
};
```

#### Go

```go
func numTimesAllBlue(flips []int) (ans int) {
	mx := 0
	for i, x := range flips {
		mx = max(mx, x)
		if mx == i+1 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function numTimesAllBlue(flips: number[]): number {
    let ans = 0;
    let mx = 0;
    for (let i = 1; i <= flips.length; ++i) {
        mx = Math.max(mx, flips[i - 1]);
        if (mx === i) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
