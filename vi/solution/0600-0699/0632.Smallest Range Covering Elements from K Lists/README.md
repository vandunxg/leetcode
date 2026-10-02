---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
    - Hash Table
    - Sorting
    - Sliding Window
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [632. Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists)

[中文文档](/solution/0600-0699/0632.Smallest%20Range%20Covering%20Elements%20from%20K%20Lists/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>k</code> danh sách số nguyên đã được sắp xếp theo <strong>thứ tự không giảm</strong>. Hãy tìm khoảng <b>nhỏ nhất</b> chứa ít nhất một số từ mỗi danh sách trong <code>k</code> danh sách.</p>

<p>Khoảng <code>[a, b]</code> được xem là nhỏ hơn khoảng <code>[c, d]</code> nếu <code>b - a &lt; d - c</code>, hoặc nếu <code>b - a == d - c</code> thì <code>a &lt; c</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]
<strong>Đầu ra:</strong> [20,24]
<strong>Giải thích: </strong>
Danh sách 1: [4, 10, 15, 24,26], có 24 nằm trong khoảng [20,24].
Danh sách 2: [0, 9, 12, 20], có 20 nằm trong khoảng [20,24].
Danh sách 3: [5, 18, 22, 30], có 22 nằm trong khoảng [20,24].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[1,2,3],[1,2,3],[1,2,3]]
<strong>Đầu ra:</strong> [1,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums.length == k</code></li>
	<li><code>1 &lt;= k &lt;= 3500</code></li>
	<li><code>1 &lt;= nums[i].length &lt;= 50</code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i][j] &lt;= 10<sup>5</sup></code></li>
	<li><code>nums[i]</code> được sắp xếp theo <strong>thứ tự không giảm</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần chọn một phần tử từ mỗi danh sách đã sắp xếp sao cho khoảng bao phủ là ngắn nhất. Việc thử mọi tổ hợp chỉ số không khả thi.
>
> Tạo các cặp $(value, group)$ rồi sắp xếp. Bài toán trở thành tìm cửa sổ ngắn nhất chứa đủ cả $k$ nhóm: mở rộng đầu phải, rồi thu hẹp đầu trái khi cửa sổ vẫn chứa đủ $k$ nhóm.

<!-- thinking:end -->

Với mỗi số $x$ thuộc nhóm $i$, ta tạo phần tử $(x, i)$ và lưu vào mảng mới $t$. Sau đó, sắp xếp $t$ theo giá trị số (tương tự như gộp nhiều mảng đã sắp xếp thành một mảng mới có thứ tự).

Tiếp theo, duyệt từng phần tử trong $t$ và xét nhóm chứa số đó. Dùng hash table để ghi nhận các nhóm có mặt trong sliding window. Nếu có đủ $k$ nhóm, cửa sổ hiện tại thỏa mãn yêu cầu. Khi đó, tính hai đầu mút của cửa sổ và cập nhật đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là tổng số phần tử trong tất cả các mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestRange(self, nums: List[List[int]]) -> List[int]:
        t = [(x, i) for i, v in enumerate(nums) for x in v]
        t.sort()
        cnt = Counter()
        ans = [-inf, inf]
        j = 0
        for i, (b, v) in enumerate(t):
            cnt[v] += 1
            while len(cnt) == len(nums):
                a = t[j][0]
                x = b - a - (ans[1] - ans[0])
                if x < 0 or (x == 0 and a < ans[0]):
                    ans = [a, b]
                w = t[j][1]
                cnt[w] -= 1
                if cnt[w] == 0:
                    cnt.pop(w)
                j += 1
        return ans
```

#### Java

```java
class Solution {
    public int[] smallestRange(List<List<Integer>> nums) {
        int n = 0;
        for (var v : nums) {
            n += v.size();
        }
        int[][] t = new int[n][2];
        int k = nums.size();
        for (int i = 0, j = 0; i < k; ++i) {
            for (int x : nums.get(i)) {
                t[j++] = new int[] {x, i};
            }
        }
        Arrays.sort(t, (a, b) -> a[0] - b[0]);
        int j = 0;
        Map<Integer, Integer> cnt = new HashMap<>();
        int[] ans = new int[] {-1000000, 1000000};
        for (int[] e : t) {
            int b = e[0];
            int v = e[1];
            cnt.merge(v, 1, Integer::sum);
            while (cnt.size() == k) {
                int a = t[j][0];
                int w = t[j][1];
                int x = b - a - (ans[1] - ans[0]);
                if (x < 0 || (x == 0 && a < ans[0])) {
                    ans[0] = a;
                    ans[1] = b;
                }
                if (cnt.merge(w, -1, Integer::sum) == 0) {
                    cnt.remove(w);
                }
                ++j;
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
    vector<int> smallestRange(vector<vector<int>>& nums) {
        int n = 0;
        for (auto& v : nums) n += v.size();
        vector<pair<int, int>> t(n);
        int k = nums.size();
        for (int i = 0, j = 0; i < k; ++i) {
            for (int v : nums[i]) {
                t[j++] = {v, i};
            }
        }
        sort(t.begin(), t.end());
        int j = 0;
        unordered_map<int, int> cnt;
        vector<int> ans = {-1000000, 1000000};
        for (int i = 0; i < n; ++i) {
            int b = t[i].first;
            int v = t[i].second;
            ++cnt[v];
            while (cnt.size() == k) {
                int a = t[j].first;
                int w = t[j].second;
                int x = b - a - (ans[1] - ans[0]);
                if (x < 0 || (x == 0 && a < ans[0])) {
                    ans[0] = a;
                    ans[1] = b;
                }
                if (--cnt[w] == 0) {
                    cnt.erase(w);
                }
                ++j;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func smallestRange(nums [][]int) []int {
	t := [][]int{}
	for i, x := range nums {
		for _, v := range x {
			t = append(t, []int{v, i})
		}
	}
	sort.Slice(t, func(i, j int) bool { return t[i][0] < t[j][0] })
	ans := []int{-1000000, 1000000}
	j := 0
	cnt := map[int]int{}
	for _, x := range t {
		b, v := x[0], x[1]
		cnt[v]++
		for len(cnt) == len(nums) {
			a, w := t[j][0], t[j][1]
			x := b - a - (ans[1] - ans[0])
			if x < 0 || (x == 0 && a < ans[0]) {
				ans[0], ans[1] = a, b
			}
			cnt[w]--
			if cnt[w] == 0 {
				delete(cnt, w)
			}
			j++
		}
	}
	return ans
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_range(nums: Vec<Vec<i32>>) -> Vec<i32> {
        let mut t = vec![];
        for (i, x) in nums.iter().enumerate() {
            for &v in x {
                t.push((v, i));
            }
        }
        t.sort_unstable();
        let (mut ans, n) = (vec![-1000000, 1000000], nums.len());
        let mut j = 0;
        let mut cnt = std::collections::HashMap::new();

        for (b, v) in t.iter() {
            let (b, v) = (*b, *v);
            if let Some(x) = cnt.get_mut(&v) {
                *x += 1;
            } else {
                cnt.insert(v, 1);
            }
            while cnt.len() == n {
                let (a, w) = t[j];
                let x = b - a - (ans[1] - ans[0]);
                if x < 0 || (x == 0 && a < ans[0]) {
                    ans = vec![a, b];
                }
                if let Some(x) = cnt.get_mut(&w) {
                    *x -= 1;
                }
                if cnt[&w] == 0 {
                    cnt.remove(&w);
                }
                j += 1;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Priority Queue (Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu tất cả phần tử. Vì mỗi danh sách đã được sắp xếp, chỉ cần min-heap chứa con trỏ hiện tại của mỗi danh sách và giá trị lớn nhất đã gặp để xác định một khoảng ứng viên; sau đó tiến con trỏ của danh sách có giá trị nhỏ nhất. Không gian sử dụng là $O(k)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
const smallestRange = (nums: number[][]): number[] => {
    const pq = new PriorityQueue<number[]>((a, b) => a[0] - b[0]);
    const n = nums.length;
    let [l, r, max] = [0, Number.POSITIVE_INFINITY, Number.NEGATIVE_INFINITY];

    for (let j = 0; j < n; j++) {
        const x = nums[j][0];
        pq.enqueue([x, j, 0]);
        max = Math.max(max, x);
    }

    while (pq.size() === n) {
        const [min, j, i] = pq.dequeue();

        if (max - min < r - l) {
            [l, r] = [min, max];
        }

        const iNext = i + 1;
        if (iNext < nums[j].length) {
            const next = nums[j][iNext];
            pq.enqueue([next, j, iNext]);
            max = Math.max(max, next);
        }
    }

    return [l, r];
};
```

#### JavaScript

```js
const smallestRange = nums => {
    const pq = new PriorityQueue((a, b) => a[0] - b[0]);
    const n = nums.length;
    let [l, r, max] = [0, Number.POSITIVE_INFINITY, Number.NEGATIVE_INFINITY];

    for (let j = 0; j < n; j++) {
        const x = nums[j][0];
        pq.enqueue([x, j, 0]);
        max = Math.max(max, x);
    }

    while (pq.size() === n) {
        const [min, j, i] = pq.dequeue();

        if (max - min < r - l) {
            [l, r] = [min, max];
        }

        const iNext = i + 1;
        if (iNext < nums[j].length) {
            const next = nums[j][iNext];
            pq.enqueue([next, j, iNext]);
            max = Math.max(max, next);
        }
    }

    return [l, r];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
