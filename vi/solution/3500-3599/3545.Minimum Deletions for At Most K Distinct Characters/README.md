---
comments: true
difficulty: Easy
rating: 1210
source: Weekly Contest 449 Q1
tags:
    - Greedy
    - Hash Table
    - String
    - Counting
    - Sorting
---

<!-- problem:start -->

# [3545. Minimum Deletions for At Most K Distinct Characters](https://leetcode.com/problems/minimum-deletions-for-at-most-k-distinct-characters)

[中文文档](/solution/3500-3599/3545.Minimum%20Deletions%20for%20At%20Most%20K%20Distinct%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường và một số nguyên <code>k</code>.</p>

<p>Nhiệm vụ của bạn là xóa một số ký tự trong chuỗi (có thể không xóa ký tự nào) sao cho số lượng ký tự <strong>phân biệt</strong> trong chuỗi kết quả <strong>không vượt quá</strong> <code>k</code>.</p>

<p>Hãy trả về số lần xóa <strong>ít nhất</strong> cần thực hiện để đạt được điều này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><code>s</code> có ba ký tự phân biệt: <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code>, mỗi ký tự xuất hiện 1 lần.</li>
    <li>Vì chuỗi chỉ được có nhiều nhất <code>k = 2</code> ký tự phân biệt, ta xóa mọi lần xuất hiện của một ký tự bất kỳ trong chuỗi.</li>
    <li>Ví dụ, xóa mọi lần xuất hiện của <code>&#39;c&#39;</code> sẽ tạo ra một chuỗi có không quá <code>k</code> ký tự phân biệt. Vì vậy, đáp án là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aabb&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><code>s</code> có hai ký tự phân biệt (<code>&#39;a&#39;</code> và <code>&#39;b&#39;</code>), mỗi ký tự xuất hiện 2 lần.</li>
    <li>Vì chuỗi có thể có nhiều nhất <code>k = 2</code> ký tự phân biệt, ta không cần xóa ký tự nào. Do đó, đáp án là 0.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;yyyzz&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><code>s</code> có hai ký tự phân biệt (<code>&#39;y&#39;</code> và <code>&#39;z&#39;</code>), với số lần xuất hiện lần lượt là 3 và 2.</li>
    <li>Vì chuỗi chỉ được có nhiều nhất <code>k = 1</code> ký tự phân biệt, ta xóa mọi lần xuất hiện của một ký tự bất kỳ trong chuỗi.</li>
    <li>Xóa mọi lần xuất hiện của <code>&#39;z&#39;</code> sẽ tạo ra một chuỗi có không quá <code>k</code> ký tự phân biệt. Vì vậy, đáp án là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 16</code></li>
    <li><code>1 &lt;= k &lt;= 16</code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<p> </p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Có thể giữ lại nhiều nhất $k$ ký tự phân biệt, nên số lần xóa bằng tổng số lần xuất hiện của các loại ký tự bị loại bỏ. Để số lần xóa là ít nhất, ta loại bỏ các loại ký tự xuất hiện ít nhất.
>
> Đếm, sắp xếp các tần suất, rồi tính tổng tất cả trừ $k$ tần suất lớn nhất. Vì kích thước bảng chữ cái là $26$, việc sắp xếp có thời gian hằng số.

<!-- thinking:end -->

Ta có thể dùng một mảng $\textit{cnt}$ để đếm số lần xuất hiện của từng ký tự. Sau đó, ta sắp xếp mảng này và trả về tổng của $26 - k$ phần tử đầu tiên.

Độ phức tạp thời gian là $O(|\Sigma| \times \log |\Sigma|)$, và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $|\Sigma|$ là kích thước của tập ký tự. Trong bài toán này, $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDeletion(self, s: str, k: int) -> int:
        return sum(sorted(Counter(s).values())[:-k])
```

#### Java

```java
class Solution {
    public int minDeletion(String s, int k) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        Arrays.sort(cnt);
        int ans = 0;
        for (int i = 0; i + k < 26; ++i) {
            ans += cnt[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDeletion(string s, int k) {
        vector<int> cnt(26);
        for (char c : s) {
            ++cnt[c - 'a'];
        }
        ranges::sort(cnt);
        int ans = 0;
        for (int i = 0; i + k < 26; ++i) {
            ans += cnt[i];
        }
        return ans;
    }
};
```

#### Go

```go
func minDeletion(s string, k int) (ans int) {
    cnt := make([]int, 26)
    for _, c := range s {
        cnt[c-'a']++
    }
    sort.Ints(cnt)
    for i := 0; i+k < len(cnt); i++ {
        ans += cnt[i]
    }
    return
}
```

#### TypeScript

```ts
function minDeletion(s: string, k: number): number {
    const cnt: number[] = Array(26).fill(0);
    for (const c of s) {
        ++cnt[c.charCodeAt(0) - 97];
    }
    cnt.sort((a, b) => a - b);
    return cnt.slice(0, 26 - k).reduce((a, b) => a + b, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
