---
comments: true
difficulty: Medium
rating: 1445
source: Biweekly Contest 135 Q2
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [3223. Minimum Length of String After Operations](https://leetcode.com/problems/minimum-length-of-string-after-operations)

[中文文档](/solution/3200-3299/3223.Minimum%20Length%20of%20String%20After%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code>.</p>

<p>Bạn có thể thực hiện quy trình sau trên <code>s</code> <strong>bất kỳ</strong> số lần nào:</p>

<ul>
    <li>Chọn một chỉ số <code>i</code> trong chuỗi sao cho có <strong>ít nhất</strong> một ký tự ở bên trái chỉ số <code>i</code> bằng với <code>s[i]</code>, và <strong>ít nhất</strong> một ký tự ở bên phải cũng bằng với <code>s[i]</code>.</li>
    <li>Xóa lần xuất hiện <code>s[i]</code> <strong>gần nhất</strong> nằm ở <strong>bên trái</strong> của <code>i</code>.</li>
    <li>Xóa lần xuất hiện <code>s[i]</code> <strong>gần nhất</strong> nằm ở <strong>bên phải</strong> của <code>i</code>.</li>
</ul>

<p>Trả về độ dài <strong>nhỏ nhất</strong> của chuỗi <code>s</code> cuối cùng mà bạn có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abaacbcbb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong><br />
Ta thực hiện các thao tác sau:</p>

<ul>
    <li>Chọn chỉ số 2, sau đó xóa các ký tự ở chỉ số 0 và 3. Chuỗi nhận được là <code>s = &quot;bacbcbb&quot;</code>.</li>
    <li>Chọn chỉ số 3, sau đó xóa các ký tự ở chỉ số 0 và 5. Chuỗi nhận được là <code>s = &quot;acbcb&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong><br />
Ta không thể thực hiện thao tác nào, nên trả về độ dài của chuỗi ban đầu.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 2 * 10<sup>5</sup></code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác xóa một ký tự trùng khớp ở mỗi bên của ký tự được chọn. Vì $n\le 2\times 10^5$, việc sửa chuỗi trực tiếp sẽ gây ra quá nhiều thao tác dịch chuyển. Việc xóa các ký tự giống nhau không phụ thuộc vào các ký tự khác, nên phần còn lại chỉ phụ thuộc vào số lần xuất hiện của ký tự đó.
>
> Nếu số lần xuất hiện là lẻ thì sau các lần xóa đối xứng còn lại $1$ ký tự; nếu là chẵn thì còn lại $2$ ký tự. Cộng kết quả của $26$ số đếm sẽ cho độ dài nhỏ nhất; không cần mô phỏng thứ tự xóa.

<!-- thinking:end -->

Ta có thể đếm số lần xuất hiện của mỗi ký tự trong chuỗi, sau đó duyệt qua mảng số đếm. Nếu một ký tự xuất hiện số lần lẻ, cuối cùng sẽ còn lại $1$ ký tự; nếu một ký tự xuất hiện số lần chẵn, sẽ còn lại $2$ ký tự. Cộng số ký tự còn lại của tất cả các ký tự để có được độ dài cuối cùng của chuỗi.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $|\Sigma|$ là kích thước của tập ký tự, bằng $26$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumLength(self, s: str) -> int:
        cnt = Counter(s)
        return sum(1 if x & 1 else 2 for x in cnt.values())
```

#### Java

```java
class Solution {
    public int minimumLength(String s) {
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        int ans = 0;
        for (int x : cnt) {
            if (x > 0) {
                ans += x % 2 == 1 ? 1 : 2;
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
    int minimumLength(string s) {
        int cnt[26]{};
        for (char& c : s) {
            ++cnt[c - 'a'];
        }
        int ans = 0;
        for (int x : cnt) {
            if (x) {
                ans += x % 2 ? 1 : 2;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumLength(s string) (ans int) {
    cnt := [26]int{}
    for _, c := range s {
        cnt[c-'a']++
    }
    for _, x := range cnt {
        if x > 0 {
            if x&1 == 1 {
                ans += 1
            } else {
                ans += 2
            }
        }
    }
    return
}
```

#### TypeScript

```ts
function minimumLength(s: string): number {
    const cnt = new Map<string, number>();
    for (const c of s) {
        cnt.set(c, (cnt.get(c) || 0) + 1);
    }
    let ans = 0;
    for (const x of cnt.values()) {
        ans += x & 1 ? 1 : 2;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
