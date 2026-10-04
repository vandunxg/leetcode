---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2838. Maximum Coins Heroes Can Collect 🔒](https://leetcode.com/problems/maximum-coins-heroes-can-collect)

[中文文档](/solution/2800-2899/2838.Maximum%20Coins%20Heroes%20Can%20Collect/README.md)

## Mô tả

<!-- description:start -->

<p>Có một trận chiến và <code>n</code> anh hùng đang cố gắng đánh bại <code>m</code> quái vật. Cho hai mảng số nguyên <strong>được đánh số từ 1</strong>, gồm các giá trị <strong>dương</strong>, <code><font face="monospace">heroes</font></code> và <code><font face="monospace">monsters</font></code> có độ dài lần lượt là <code>n</code> và <code>m</code>. <code><font face="monospace">heroes</font>[i]</code> là sức mạnh của <code>i<sup>th</sup></code> anh hùng, còn <code><font face="monospace">monsters</font>[i]</code> là sức mạnh của <code>i<sup>th</sup></code> quái vật.</p>

<p><code>i<sup>th</sup></code> anh hùng có thể đánh bại <code>j<sup>th</sup></code> quái vật nếu <code>monsters[j] &lt;= heroes[i]</code>.</p>

<p>Ngoài ra, cho mảng <code>coins</code> được <strong>đánh số từ 1</strong>, có độ dài <code>m</code> và gồm các số nguyên <strong>dương</strong>. <code>coins[i]</code> là số xu mà mỗi anh hùng nhận được sau khi đánh bại <code>i<sup>th</sup></code> quái vật.</p>

<p>Trả về<em> một mảng </em><code>ans</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>ans[i]</code><em> là số xu <strong>lớn nhất</strong> mà anh hùng </em><code>i<sup>th</sup></code><em> có thể thu thập được từ trận chiến này</em>.</p>

<p><strong>Lưu ý</strong></p>

<ul>
	<li>Sức khỏe của anh hùng không giảm sau khi đánh bại một quái vật.</li>
	<li>Nhiều anh hùng có thể đánh bại cùng một quái vật, nhưng mỗi anh hùng chỉ có thể đánh bại một quái vật tối đa một lần.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> heroes = [1,4,2], monsters = [1,1,5,2,3], coins = [2,3,4,5,6]
<strong>Đầu ra:</strong> [5,16,10]
<strong>Giải thích: </strong>Với mỗi anh hùng, ta liệt kê chỉ số của tất cả quái vật mà anh hùng đó có thể đánh bại:
Anh hùng <sup>thứ nhất</sup>: [1,2] vì sức mạnh của anh hùng này là 1 và monsters[1], monsters[2] &lt;= 1. Vì vậy, anh hùng này thu thập coins[1] + coins[2] = 5 xu.
Anh hùng <sup>thứ hai</sup>: [1,2,4,5] vì sức mạnh của anh hùng này là 4 và monsters[1], monsters[2], monsters[4], monsters[5] &lt;= 4. Vì vậy, anh hùng này thu thập coins[1] + coins[2] + coins[4] + coins[5] = 16 xu.
Anh hùng <sup>thứ ba</sup>: [1,2,4] vì sức mạnh của anh hùng này là 2 và monsters[1], monsters[2], monsters[4] &lt;= 2. Vì vậy, anh hùng này thu thập coins[1] + coins[2] + coins[4] = 10 xu.
Vậy đáp án là [5,16,10].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> heroes = [5], monsters = [2,3,1,2], coins = [10,6,5,2]
<strong>Đầu ra:</strong> [23]
<strong>Giải thích:</strong> Anh hùng này có thể đánh bại tất cả quái vật vì monsters[i] &lt;= 5. Vì vậy, anh hùng thu thập tất cả số xu: coins[1] + coins[2] + coins[3] + coins[4] = 23, và đáp án là [23].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> heroes = [4,4], monsters = [5,7,8], coins = [1,1,1]
<strong>Đầu ra:</strong> [0,0]
<strong>Giải thích:</strong> Trong ví dụ này, không anh hùng nào có thể đánh bại quái vật. Vậy đáp án là [0,0],
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == heroes.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m == monsters.length &lt;= 10<sup>5</sup></code></li>
	<li><code>coins.length == m</code></li>
	<li><code>1 &lt;= heroes[i], monsters[i], coins[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tổng tiền tố + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi anh hùng nhận xu từ mọi quái vật có sức mạnh không vượt quá sức mạnh của mình. Sắp xếp quái vật theo sức mạnh, tính tổng tiền tố của số xu tương ứng, rồi dùng tìm kiếm nhị phân để tìm quái vật cuối cùng mà mỗi anh hùng có thể đánh bại.

<!-- thinking:end -->

Ta có thể sắp xếp quái vật và các đồng xu theo thứ tự tăng dần của sức mạnh chiến đấu của quái vật, sau đó dùng tổng tiền tố để tính tổng số xu mà mỗi anh hùng có thể nhận được khi đánh bại $i$ quái vật đầu tiên.

Tiếp theo, với mỗi anh hùng, ta có thể dùng tìm kiếm nhị phân để tìm quái vật mạnh nhất mà anh hùng đó có thể đánh bại, rồi dùng tổng tiền tố để tính tổng số xu mà anh hùng đó có thể nhận được.

Độ phức tạp thời gian là $O((m + n) \times \log n)$, còn độ phức tạp không gian là $O(m)$. Ở đây, $m$ và $n$ lần lượt là số quái vật và số anh hùng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumCoins(
        self, heroes: List[int], monsters: List[int], coins: List[int]
    ) -> List[int]:
        m = len(monsters)
        idx = sorted(range(m), key=lambda i: monsters[i])
        s = list(accumulate((coins[i] for i in idx), initial=0))
        ans = []
        for h in heroes:
            i = bisect_right(idx, h, key=lambda i: monsters[i])
            ans.append(s[i])
        return ans
```

#### Java

```java
class Solution {
    public long[] maximumCoins(int[] heroes, int[] monsters, int[] coins) {
        int m = monsters.length;
        Integer[] idx = new Integer[m];
        for (int i = 0; i < m; ++i) {
            idx[i] = i;
        }

        Arrays.sort(idx, Comparator.comparingInt(j -> monsters[j]));
        long[] s = new long[m + 1];
        for (int i = 0; i < m; ++i) {
            s[i + 1] = s[i] + coins[idx[i]];
        }
        int n = heroes.length;
        long[] ans = new long[n];
        for (int k = 0; k < n; ++k) {
            int i = search(monsters, idx, heroes[k]);
            ans[k] = s[i];
        }
        return ans;
    }

    private int search(int[] nums, Integer[] idx, int x) {
        int l = 0, r = idx.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[idx[mid]] > x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> maximumCoins(vector<int>& heroes, vector<int>& monsters, vector<int>& coins) {
        int m = monsters.size();
        vector<int> idx(m);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) {
            return monsters[i] < monsters[j];
        });
        long long s[m + 1];
        s[0] = 0;
        for (int i = 1; i <= m; ++i) {
            s[i] = s[i - 1] + coins[idx[i - 1]];
        }
        vector<long long> ans;
        auto search = [&](int x) {
            int l = 0, r = m;
            while (l < r) {
                int mid = (l + r) >> 1;
                if (monsters[idx[mid]] > x) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            return l;
        };
        for (int h : heroes) {
            ans.push_back(s[search(h)]);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumCoins(heroes []int, monsters []int, coins []int) (ans []int64) {
	m := len(monsters)
	idx := make([]int, m)
	for i := range idx {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool { return monsters[idx[i]] < monsters[idx[j]] })
	s := make([]int64, m+1)
	for i, j := range idx {
		s[i+1] = s[i] + int64(coins[j])
	}
	for _, h := range heroes {
		i := sort.Search(m, func(i int) bool { return monsters[idx[i]] > h })
		ans = append(ans, s[i])
	}
	return
}
```

#### TypeScript

```ts
function maximumCoins(heroes: number[], monsters: number[], coins: number[]): number[] {
    const m = monsters.length;
    const idx: number[] = Array.from({ length: m }, (_, i) => i);
    idx.sort((i, j) => monsters[i] - monsters[j]);
    const s: number[] = Array(m + 1).fill(0);
    for (let i = 0; i < m; ++i) {
        s[i + 1] = s[i] + coins[idx[i]];
    }
    const search = (x: number): number => {
        let l = 0;
        let r = m;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (monsters[idx[mid]] > x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    return heroes.map(h => s[search(h)]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
