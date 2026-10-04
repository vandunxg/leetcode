---
comments: true
difficulty: Medium
rating: 1422
source: Weekly Contest 372 Q2
tags:
    - Greedy
    - Two Pointers
    - String
---

<!-- problem:start -->

# [2938. Separate Black and White Balls](https://leetcode.com/problems/separate-black-and-white-balls)

[中文文档](/solution/2900-2999/2938.Separate%20Black%20and%20White%20Balls/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> quả bóng trên bàn, mỗi quả bóng có màu đen hoặc trắng.</p>

<p>Bạn được cung cấp một chuỗi nhị phân <strong>được đánh chỉ số từ 0</strong> <code>s</code> có độ dài <code>n</code>, trong đó <code>1</code> và <code>0</code> lần lượt biểu thị các quả bóng đen và trắng.</p>

<p>Trong mỗi bước, bạn có thể chọn hai quả bóng kề nhau và đổi chỗ cho nhau.</p>

<p>Hãy trả về <em>số bước <strong>tối thiểu</strong> để gom tất cả bóng đen sang bên phải và tất cả bóng trắng sang bên trái</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;101&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể gom tất cả bóng đen sang bên phải như sau:
- Đổi chỗ s[0] và s[1], s = &quot;011&quot;.
Ban đầu, các số 1 chưa được gom lại với nhau, nên cần ít nhất 1 bước để gom chúng sang bên phải.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;100&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể gom tất cả bóng đen sang bên phải như sau:
- Đổi chỗ s[0] và s[1], s = &quot;010&quot;.
- Đổi chỗ s[1] và s[2], s = &quot;001&quot;.
Có thể chứng minh rằng số bước tối thiểu cần thiết là 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0111&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Tất cả bóng đen đã được gom về bên phải.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng bằng cách đếm

<!-- thinking:start -->

> **Tư duy**
>
> Các phép đổi chỗ kề nhau đưa mọi $1$ sang phải (tương đương đưa mọi $0$ sang trái) có chi phí bằng số lượng $0$ nằm bên phải mỗi $1$. $n \le 10^5$. Khi duyệt từ phải sang trái, một $1$ vẫn cần đi qua $n-i-cnt$ vị trí trống ở bên phải.
>
> $cnt$ là số lượng số 1 đã gặp; tổng các khoảng trống đó chính là số phép đổi chỗ tối thiểu. Không cần di chuyển các quả bóng một cách tường minh.

<!-- thinking:end -->

Ta xét việc đưa tất cả các số '1' về phía ngoài cùng bên phải. Ta dùng biến $cnt$ để ghi nhận số lượng số '1' hiện đã được đưa về phía ngoài cùng bên phải, và biến $ans$ để ghi nhận số lần di chuyển.

Duyệt chuỗi từ phải sang trái. Nếu vị trí hiện tại là '1', ta tăng $cnt$ thêm một, đồng thời cộng $n - i - cnt$ vào $ans$, trong đó $n$ là độ dài chuỗi. Cuối cùng, trả về $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSteps(self, s: str) -> int:
        n = len(s)
        ans = cnt = 0
        for i in range(n - 1, -1, -1):
            if s[i] == '1':
                cnt += 1
                ans += n - i - cnt
        return ans
```

#### Java

```java
class Solution {
    public long minimumSteps(String s) {
        long ans = 0;
        int cnt = 0;
        int n = s.length();
        for (int i = n - 1; i >= 0; --i) {
            if (s.charAt(i) == '1') {
                ++cnt;
                ans += n - i - cnt;
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
    long long minimumSteps(string s) {
        long long ans = 0;
        int cnt = 0;
        int n = s.size();
        for (int i = n - 1; i >= 0; --i) {
            if (s[i] == '1') {
                ++cnt;
                ans += n - i - cnt;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumSteps(s string) (ans int64) {
	n := len(s)
	cnt := 0
	for i := n - 1; i >= 0; i-- {
		if s[i] == '1' {
			cnt++
			ans += int64(n - i - cnt)
		}
	}
	return
}
```

#### TypeScript

```ts
function minimumSteps(s: string): number {
    const n = s.length;
    let [ans, cnt] = [0, 0];
    for (let i = n - 1; ~i; --i) {
        if (s[i] === '1') {
            ++cnt;
            ans += n - i - cnt;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
