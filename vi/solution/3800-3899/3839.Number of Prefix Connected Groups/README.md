---
comments: true
difficulty: Medium
rating: 1401
source: Biweekly Contest 176 Q2
tags:
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [3839. Number of Prefix Connected Groups](https://leetcode.com/problems/number-of-prefix-connected-groups)

[中文文档](/solution/3800-3899/3839.Number%20of%20Prefix%20Connected%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng các chuỗi <code>words</code> và một số nguyên <code>k</code>.</p>

<p>Hai chuỗi <code>a</code> và <code>b</code> tại <strong>các chỉ số khác nhau</strong> được gọi là <strong><span data-keyword="string-prefix">liên kết theo tiền tố</span></strong> nếu <code>a[0..k-1] == b[0..k-1]</code>.</p>

<p>Một <strong>nhóm liên thông</strong> là một tập hợp các chuỗi sao cho mọi cặp chuỗi đều liên kết theo tiền tố.</p>

<p>Hãy trả về <strong>số lượng nhóm liên thông</strong> được tạo từ các chuỗi đã cho và <strong>chứa ít nhất</strong> hai chuỗi.</p>

<p><strong>Ghi chú:</strong></p>

<ul>
	<li>Các chuỗi có độ dài nhỏ hơn <code>k</code> không thể tham gia nhóm nào và sẽ bị bỏ qua.</li>
	<li>Các chuỗi trùng lặp được xem là những chuỗi riêng biệt.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;apple&quot;,&quot;apply&quot;,&quot;banana&quot;,&quot;bandit&quot;], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi có cùng <code>k = 2</code> ký tự đầu tiên được xếp vào cùng một nhóm:</p>

<ul>
	<li><code>words[0] = &quot;apple&quot;</code> và <code>words[1] = &quot;apply&quot;</code> có chung tiền tố <code>&quot;ap&quot;</code>.</li>
	<li><code>words[2] = &quot;banana&quot;</code> và <code>words[3] = &quot;bandit&quot;</code> có chung tiền tố <code>&quot;ba&quot;</code>.</li>
</ul>

<p>Vì vậy, có 2 nhóm liên thông, mỗi nhóm chứa ít nhất hai chuỗi.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;car&quot;,&quot;cat&quot;,&quot;cartoon&quot;], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi được xét theo tiền tố có độ dài <code>k = 3</code>:</p>

<ul>
	<li><code>words[0] = &quot;car&quot;</code> và <code>words[2] = &quot;cartoon&quot;</code> có chung tiền tố <code>&quot;car&quot;</code>.</li>
	<li><code>words[1] = &quot;cat&quot;</code> không có tiền tố dài 3 trùng với chuỗi nào khác.</li>
</ul>

<p>Vì vậy, có 1 nhóm liên thông.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = </span>[&quot;bat&quot;,&quot;dog&quot;,&quot;dog&quot;,&quot;doggy&quot;,&quot;bat&quot;]<span class="example-io">, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi được xét theo tiền tố có độ dài <code>k = 3</code>:</p>

<ul>
	<li><code>words[0] = &quot;bat&quot;</code> và <code>words[4] = &quot;bat&quot;</code> tạo thành một nhóm.</li>
	<li><code>words[1] = &quot;dog&quot;</code>, <code>words[2] = &quot;dog&quot;</code> và <code>words[3] = &quot;doggy&quot;</code> có chung tiền tố <code>&quot;dog&quot;</code>.</li>
</ul>

<p>Vì vậy, có 2 nhóm liên thông, mỗi nhóm chứa ít nhất hai chuỗi.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 5000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
	<li>Tất cả chuỗi trong <code>words</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Các chuỗi có độ dài ít nhất $k$ và có cùng $k$ ký tự đầu tiên tạo thành một nhóm; ta đếm các nhóm có ít nhất hai phần tử. Vì $n \le 5000$, không cần kiểm tra tiền tố theo từng cặp.
>
> Một nhóm được xác định hoàn toàn bởi tiền tố có độ dài $k$, không phụ thuộc vào phần hậu tố.
>
> Với mỗi chuỗi có độ dài $\ge k$, ta đếm $w[:k]$ trong một hash map, sau đó đếm các key có tần suất lớn hơn $1$.
>
> Các chuỗi ngắn hơn k sẽ bị bỏ qua.

<!-- thinking:end -->

Ta sử dụng một bảng băm $\textit{cnt}$ để đếm số lần xuất hiện của tiền tố gồm $k$ ký tự đầu tiên của mỗi chuỗi có độ dài lớn hơn hoặc bằng $k$. Cuối cùng, ta đếm số key trong $\textit{cnt}$ có giá trị lớn hơn $1$, đó chính là số nhóm liên thông.

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{words}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def prefixConnected(self, words: List[str], k: int) -> int:
        cnt = Counter()
        for w in words:
            if len(w) >= k:
                cnt[w[:k]] += 1
        return sum(v > 1 for v in cnt.values())
```

#### Java

```java
class Solution {
    public int prefixConnected(String[] words, int k) {
        Map<String, Integer> cnt = new HashMap<>();
        for (var w : words) {
            if (w.length() >= k) {
                cnt.merge(w.substring(0, k), 1, Integer::sum);
            }
        }
        int ans = 0;
        for (var v : cnt.values()) {
            if (v > 1) {
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
    int prefixConnected(vector<string>& words, int k) {
        unordered_map<string, int> cnt;
        for (const auto& w : words) {
            if (w.size() >= k) {
                ++cnt[w.substr(0, k)];
            }
        }
        int ans = 0;
        for (const auto& [_, v] : cnt) {
            if (v > 1) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func prefixConnected(words []string, k int) int {
	cnt := make(map[string]int)
	for _, w := range words {
		if len(w) >= k {
			cnt[w[:k]]++
		}
	}
	ans := 0
	for _, v := range cnt {
		if v > 1 {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function prefixConnected(words: string[], k: number): number {
    const cnt = new Map<string, number>();

    for (const w of words) {
        if (w.length >= k) {
            const key = w.substring(0, k);
            cnt.set(key, (cnt.get(key) ?? 0) + 1);
        }
    }

    let ans = 0;
    for (const v of cnt.values()) {
        if (v > 1) {
            ans++;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
