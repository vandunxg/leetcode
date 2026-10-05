---
comments: true
difficulty: Medium
rating: 1908
source: Weekly Contest 473 Q3
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [3728. Stable Subarrays With Equal Boundary and Interior Sum](https://leetcode.com/problems/stable-subarrays-with-equal-boundary-and-interior-sum)

[中文文档](/solution/3700-3799/3728.Stable%20Subarrays%20With%20Equal%20Boundary%20and%20Interior%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>capacity</code>.</p>

<p>Một <span data-keyword="subarray-nonempty">mảng con</span> <code>capacity[l..r]</code> được gọi là <strong>stable</strong> nếu:</p>

<ul>
	<li>Độ dài của nó <strong>ít nhất</strong> là 3.</li>
	<li>Phần tử <strong>đầu tiên</strong> và <strong>cuối cùng</strong> đều bằng <strong>tổng</strong> của tất cả các phần tử nằm <strong>giữa</strong> chúng (nghiêm ngặt), tức là <code>capacity[l] = capacity[r] = capacity[l + 1] + capacity[l + 2] + ... + capacity[r - 1]</code>.</li>
</ul>

<p>Trả về một số nguyên biểu thị số lượng <strong>mảng con stable</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">capacity = [9,3,3,3,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>[9,3,3,3,9]</code> là stable vì phần tử đầu tiên và cuối cùng đều là 9, còn tổng các phần tử nằm giữa chúng là <code>3 + 3 + 3 = 9</code>.</li>
	<li><code>[3,3,3]</code> là stable vì phần tử đầu tiên và cuối cùng đều là 3, còn tổng các phần tử nằm giữa chúng là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">capacity = [1,2,3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có mảng con nào có độ dài ít nhất 3 mà phần tử đầu tiên và cuối cùng bằng nhau, nên đáp án là 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">capacity = [-4,4,0,0,-8,-4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>[-4,4,0,0,-8,-4]</code> là stable vì phần tử đầu tiên và cuối cùng đều là -4, còn tổng các phần tử nằm giữa chúng là <code>4 + 0 + 0 + (-8) = -4</code></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= capacity.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= capacity[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hash Table + Enumeration

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10^5$ khiến việc duyệt tất cả các đoạn có độ dài ít nhất $3$ là không thể. Sau khi biểu diễn điều kiện bằng prefix sum, với mỗi điểm cuối $r$, ta cần tìm một điểm đầu có cùng giá trị biên và có tổng các phần tử bên trong bằng giá trị đó. Khi duyệt $r$, ta thêm ứng viên $l=r-2$ vào hash map rồi truy vấn cặp $(\textit{capacity}[r],s[r])$.

<!-- thinking:end -->

Ta định nghĩa một mảng prefix sum $\textit{s}$, trong đó $s[i]$ biểu thị tổng của $i$ phần tử đầu tiên trong mảng $\text{capacity}$, tức là $s[i] = \text{capacity}[0] + \text{capacity}[1] + \ldots + \text{capacity}[i-1]$. Ban đầu, $s[0] = 0$.

Theo đề bài, một mảng con $\text{capacity}[l..r]$ là stable nếu:

$$
\text{capacity}[l] = \text{capacity}[r] = \text{capacity}[l + 1] + \text{capacity}[l + 2] + \ldots + \text{capacity}[r - 1]
$$

Tức là:

$$
\text{capacity}[l] = \text{capacity}[r] = s[r] - s[l + 1]
$$

Ta có thể duyệt điểm cuối $r$. Với mỗi $r$, ta tính điểm đầu $l = r - 2$ và lưu thông tin của các điểm đầu thỏa mãn điều kiện vào một hash table. Cụ thể, ta dùng hash table $\text{cnt}$ để ghi nhận số lần xuất hiện của mỗi cặp key-value $(\text{capacity}[l], \text{capacity}[l] + s[l + 1])$.

Khi duyệt điểm cuối $r$, ta có thể truy vấn hash table $\text{cnt}$ để lấy số điểm đầu thỏa mãn điều kiện, tức là số lần xuất hiện của cặp key-value $(\text{capacity}[r], s[r])$, rồi cộng vào đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countStableSubarrays(self, capacity: List[int]) -> int:
        s = list(accumulate(capacity, initial=0))
        n = len(capacity)
        ans = 0
        cnt = defaultdict(int)
        for r in range(2, n):
            l = r - 2
            cnt[(capacity[l], capacity[l] + s[l + 1])] += 1
            ans += cnt[(capacity[r], s[r])]
        return ans
```

#### Java

```java
class Solution {
    public long countStableSubarrays(int[] capacity) {
        int n = capacity.length;
        long[] s = new long[n + 1];
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + capacity[i - 1];
        }
        Map<Pair<Integer, Long>, Integer> cnt = new HashMap<>();
        long ans = 0;
        for (int r = 2; r < n; ++r) {
            int l = r - 2;
            cnt.merge(new Pair<>(capacity[l], capacity[l] + s[l + 1]), 1, Integer::sum);
            ans += cnt.getOrDefault(new Pair<>(capacity[r], s[r]), 0);
        }
        return ans;
    }
}
```

#### C++

```cpp
struct PairHash {
    size_t operator()(const pair<int, long long>& p) const {
        return hash<int>()(p.first) ^ (hash<long long>()(p.second) << 1);
    }
};

class Solution {
public:
    long long countStableSubarrays(vector<int>& capacity) {
        int n = capacity.size();
        vector<long long> s(n + 1, 0);
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + capacity[i - 1];
        }

        unordered_map<pair<int, long long>, int, PairHash> cnt;
        long long ans = 0;

        for (int r = 2; r < n; ++r) {
            int l = r - 2;
            pair<int, long long> keyL = {capacity[l], capacity[l] + s[l + 1]};
            cnt[keyL] += 1;

            pair<int, long long> keyR = {capacity[r], s[r]};
            ans += cnt.count(keyR) ? cnt[keyR] : 0;
        }

        return ans;
    }
};
```

#### Go

```go
func countStableSubarrays(capacity []int) (ans int64) {
	n := len(capacity)
	s := make([]int64, n+1)
	for i := 1; i <= n; i++ {
		s[i] = s[i-1] + int64(capacity[i-1])
	}

	type key struct {
		first  int
		second int64
	}

	cnt := make(map[key]int)

	for r := 2; r < n; r++ {
		l := r - 2
		keyL := key{capacity[l], int64(capacity[l]) + s[l+1]}
		cnt[keyL] += 1

		keyR := key{capacity[r], s[r]}
		ans += int64(cnt[keyR])
	}

	return
}
```

#### TypeScript

```ts
function countStableSubarrays(capacity: number[]): number {
    const n = capacity.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 1; i <= n; i++) {
        s[i] = s[i - 1] + capacity[i - 1];
    }

    const cnt = new Map<string, number>();
    let ans = 0;

    for (let r = 2; r < n; r++) {
        const l = r - 2;
        const keyL = `${capacity[l]},${capacity[l] + s[l + 1]}`;
        cnt.set(keyL, (cnt.get(keyL) || 0) + 1);

        const keyR = `${capacity[r]},${s[r]}`;
        ans += cnt.get(keyR) || 0;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
