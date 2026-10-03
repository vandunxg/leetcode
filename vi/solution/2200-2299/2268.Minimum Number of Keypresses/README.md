---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Hash Table
    - String
    - Counting
    - Sorting
---

<!-- problem:start -->

# [2268. Minimum Number of Keypresses 🔒](https://leetcode.com/problems/minimum-number-of-keypresses)

[中文文档](/solution/2200-2299/2268.Minimum%20Number%20of%20Keypresses/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một bàn phím gồm <code>9</code> nút, được đánh số từ <code>1</code> đến <code>9</code>, mỗi nút được ánh xạ với các chữ cái tiếng Anh viết thường. Bạn có thể chọn các ký tự được ánh xạ với từng nút miễn là:</p>

<ul>
	<li>Cả 26 chữ cái tiếng Anh viết thường đều được ánh xạ.</li>
	<li>Mỗi ký tự được ánh xạ với <strong>chính xác</strong> <code>1</code> nút.</li>
	<li>Mỗi nút ánh xạ với <strong>tối đa</strong> <code>3</code> ký tự.</li>
</ul>

<p>Để nhập ký tự đầu tiên được ánh xạ với một nút, bạn nhấn nút đó một lần. Để nhập ký tự thứ hai, bạn nhấn nút đó hai lần, vân vân.</p>

<p>Cho một chuỗi <code>s</code>, hãy trả về <em><strong>số lần nhấn phím nhỏ nhất</strong> cần thiết để nhập </em><code>s</code><em> bằng bàn phím của bạn.</em></p>

<p><strong>Lưu ý</strong> rằng các ký tự được ánh xạ với mỗi nút và thứ tự ánh xạ của chúng không thể thay đổi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2268.Minimum%20Number%20of%20Keypresses/images/image-20220505184346-1.png" style="width: 300px; height: 293px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;apple&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Một cách tối ưu để thiết lập bàn phím được minh họa ở trên.
Nhập &#39;a&#39; bằng cách nhấn nút 1 một lần.
Nhập &#39;p&#39; bằng cách nhấn nút 6 một lần.
Nhập &#39;p&#39; bằng cách nhấn nút 6 một lần.
Nhập &#39;l&#39; bằng cách nhấn nút 5 một lần.
Nhập &#39;e&#39; bằng cách nhấn nút 3 một lần.
Cần tổng cộng 5 lần nhấn nút, nên trả về 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2268.Minimum%20Number%20of%20Keypresses/images/image-20220505203823-1.png" style="width: 300px; height: 288px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;abcdefghijkl&quot;
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Một cách tối ưu để thiết lập bàn phím được minh họa ở trên.
Các chữ cái từ &#39;a&#39; đến &#39;i&#39; đều có thể được nhập bằng cách nhấn một nút một lần.
Nhập &#39;j&#39; bằng cách nhấn nút 1 hai lần.
Nhập &#39;k&#39; bằng cách nhấn nút 2 hai lần.
Nhập &#39;l&#39; bằng cách nhấn nút 3 hai lần.
Cần tổng cộng 15 lần nhấn nút, nên trả về 15.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Có chín phím, mỗi phím chứa tối đa ba chữ cái; số lần nhấn của một chữ cái chính là vị trí của nó trên phím. Vì $|s| \le 10^5$, các chữ cái xuất hiện thường xuyên nên được đặt ở các vị trí đầu tiên.
>
> Sắp xếp tần suất theo thứ tự giảm dần: chín chữ cái đầu tiên có chi phí $1$, chín chữ cái tiếp theo có chi phí $2$, cứ thế tiếp tục. Nhân tần suất của chữ cái thứ $i$ với tầng hiện tại $k$, đồng thời tăng $k$ sau mỗi chín chữ cái.

<!-- thinking:end -->

Trước tiên, ta đếm số lần xuất hiện của mỗi ký tự trong chuỗi $s$ và lưu kết quả vào một mảng hoặc hash table $\textit{cnt}$.

Bài toán yêu cầu tối thiểu hóa số lần nhấn phím, vì vậy $9$ ký tự xuất hiện nhiều nhất nên được ánh xạ với các phím từ $1$ đến $9$, các ký tự xuất hiện nhiều thứ $10$ đến $18$ nên được ánh xạ với các phím từ $1$ đến $9$ một lần nữa, và cứ thế tiếp tục.

Do đó, ta có thể sắp xếp các giá trị trong $\textit{cnt}$ theo thứ tự giảm dần, sau đó phân bổ chúng cho các phím theo thứ tự từ $1$ đến $9$, tăng số lần nhấn phím lên $1$ sau mỗi $9$ ký tự được phân bổ.

Độ phức tạp thời gian là $O(n + |\Sigma| \times \log |\Sigma|)$, còn độ phức tạp không gian là $O(|\Sigma|)$. Ở đây, $n$ là độ dài của chuỗi $s$, và $\Sigma$ là tập các ký tự xuất hiện trong chuỗi $s$. Trong bài toán này, $\Sigma$ là tập các chữ cái viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumKeypresses(self, s: str) -> int:
        cnt = Counter(s)
        ans, k = 0, 1
        for i, x in enumerate(sorted(cnt.values(), reverse=True), 1):
            ans += k * x
            if i % 9 == 0:
                k += 1
        return ans
```

#### Java

```java
class Solution {
    public int minimumKeypresses(String s) {
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        Arrays.sort(cnt);
        int ans = 0, k = 1;
        for (int i = 1; i <= 26; ++i) {
            ans += k * cnt[26 - i];
            if (i % 9 == 0) {
                ++k;
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
    int minimumKeypresses(string s) {
        int cnt[26]{};
        for (char& c : s) {
            ++cnt[c - 'a'];
        }
        sort(begin(cnt), end(cnt), greater<int>());
        int ans = 0, k = 1;
        for (int i = 1; i <= 26; ++i) {
            ans += k * cnt[i - 1];
            if (i % 9 == 0) {
                ++k;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumKeypresses(s string) (ans int) {
	cnt := make([]int, 26)
	for _, c := range s {
		cnt[c-'a']++
	}
	sort.Ints(cnt)
	k := 1
	for i := 1; i <= 26; i++ {
		ans += k * cnt[26-i]
		if i%9 == 0 {
			k++
		}
	}
	return
}
```

#### TypeScript

```ts
function minimumKeypresses(s: string): number {
    const cnt: number[] = Array(26).fill(0);
    const a = 'a'.charCodeAt(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0) - a];
    }
    cnt.sort((a, b) => b - a);
    let [ans, k] = [0, 1];
    for (let i = 1; i <= 26; ++i) {
        ans += k * cnt[i - 1];
        if (i % 9 === 0) {
            ++k;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
