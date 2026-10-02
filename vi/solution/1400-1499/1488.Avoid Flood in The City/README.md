---
comments: true
difficulty: Medium
rating: 1973
source: Weekly Contest 194 Q3
tags:
    - Greedy
    - Array
    - Hash Table
    - Binary Search
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1488. Avoid Flood in The City](https://leetcode.com/problems/avoid-flood-in-the-city)

[中文文档](/solution/1400-1499/1488.Avoid%20Flood%20in%20The%20City/README.md)

## Mô tả

<!-- description:start -->

<p>Đất nước của bạn có 10<sup>9</sup> hồ. Ban đầu, tất cả các hồ đều trống, nhưng khi trời mưa trên hồ thứ <code>n<sup>th</sup></code>, hồ thứ <code>n<sup>th</sup></code> sẽ đầy nước. Nếu trời mưa trên một hồ đang <strong>đầy nước</strong>, lũ sẽ xảy ra. Mục tiêu của bạn là tránh lũ ở mọi hồ.</p>

<p>Cho một mảng số nguyên <code>rains</code>, trong đó:</p>

<ul>
	<li><code>rains[i] &gt; 0</code> nghĩa là hồ <code>rains[i]</code> sẽ có mưa.</li>
	<li><code>rains[i] == 0</code> nghĩa là ngày đó không có mưa và bạn&nbsp;<strong>phải</strong> chọn&nbsp;<strong>một hồ</strong> trong ngày đó và <strong>làm khô hồ</strong>.</li>
</ul>

<p>Trả về <em>một mảng <code>ans</code></em> sao cho:</p>

<ul>
	<li><code>ans.length == rains.length</code></li>
	<li><code>ans[i] == -1</code> nếu <code>rains[i] &gt; 0</code>.</li>
	<li><code>ans[i]</code> là hồ bạn chọn để làm khô vào ngày thứ <code>i</code> nếu <code>rains[i] == 0</code>.</li>
</ul>

<p>Nếu có nhiều đáp án hợp lệ, hãy trả về <strong>bất kỳ</strong> đáp án nào. Nếu không thể tránh lũ, hãy trả về <strong>một mảng rỗng</strong>.</p>

<p>Lưu ý rằng nếu bạn chọn làm khô một hồ đầy nước, hồ đó sẽ trở nên trống, nhưng nếu bạn chọn làm khô một hồ trống thì không có gì thay đổi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> rains = [1,2,3,4]
<strong>Đầu ra:</strong> [-1,-1,-1,-1]
<strong>Giải thích:</strong> Sau ngày đầu tiên, các hồ đầy nước là [1]
Sau ngày thứ hai, các hồ đầy nước là [1,2]
Sau ngày thứ ba, các hồ đầy nước là [1,2,3]
Sau ngày thứ tư, các hồ đầy nước là [1,2,3,4]
Không có ngày nào để làm khô hồ và không có hồ nào bị lũ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> rains = [1,2,0,0,2,1]
<strong>Đầu ra:</strong> [-1,-1,2,1,-1,-1]
<strong>Giải thích:</strong> Sau ngày đầu tiên, các hồ đầy nước là [1]
Sau ngày thứ hai, các hồ đầy nước là [1,2]
Sau ngày thứ ba, ta làm khô hồ 2. Các hồ đầy nước là [1]
Sau ngày thứ tư, ta làm khô hồ 1. Không còn hồ nào đầy nước.
Sau ngày thứ năm, các hồ đầy nước là [2].
Sau ngày thứ sáu, các hồ đầy nước là [1,2].
Dễ thấy kịch bản này không có lũ. [-1,-1,1,2,-1,-1] cũng là một kịch bản hợp lệ khác.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> rains = [1,2,0,1,2]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Sau ngày thứ hai, các hồ đầy nước là  [1,2]. Ta phải làm khô một hồ vào ngày thứ ba.
Sau đó, trời sẽ mưa trên các hồ [1,2]. Có thể dễ dàng chứng minh rằng dù bạn chọn làm khô hồ nào vào ngày thứ 3, hồ còn lại cũng sẽ bị lũ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= rains.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= rains[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Binary Search

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10^5$. Một hồ mà trời mưa lần thứ hai phải được làm khô sau lần mưa trước đó. Một ngày khô nên được dùng cho hồ sẽ có mưa lại sớm nhất trong số các hồ vẫn đang đầy.
>
> Lưu các ngày khô trong một danh sách đã sắp xếp và ngày mưa gần nhất của mỗi hồ trong một map. Khi trời mưa lại, dùng binary search để tìm ngày khô đầu tiên sau ngày mưa trước đó; nếu không tìm thấy thì thất bại. Các ngày khô chưa dùng được gán giá trị $1$.

<!-- thinking:end -->

Ta lưu tất cả các ngày nắng trong mảng $sunny$ hoặc một sorted set, đồng thời dùng hash table $rainy$ để ghi lại ngày mưa gần nhất của mỗi hồ. Ta khởi tạo mảng đáp án $ans$ với mọi phần tử đều bằng $-1$.

Tiếp theo, ta duyệt qua mảng $rains$. Với mỗi ngày mưa $i$, nếu $rainy[rains[i]]$ tồn tại, điều đó có nghĩa là hồ này đã từng có mưa, nên ta cần tìm ngày đầu tiên trong mảng $sunny$ lớn hơn $rainy[rains[i]]$, rồi thay ngày đó bằng ngày mưa. Nếu không tìm được, điều đó có nghĩa là không thể ngăn lũ và ta trả về một mảng rỗng. Với mỗi ngày không mưa $i$, ta lưu $i$ vào mảng $sunny$ và gán $ans[i]$ bằng $1$.

Sau khi duyệt xong, ta trả về mảng đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $rains$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def avoidFlood(self, rains: List[int]) -> List[int]:
        n = len(rains)
        ans = [-1] * n
        sunny = SortedList()
        rainy = {}
        for i, v in enumerate(rains):
            if v:
                if v in rainy:
                    idx = sunny.bisect_right(rainy[v])
                    if idx == len(sunny):
                        return []
                    ans[sunny[idx]] = v
                    sunny.discard(sunny[idx])
                rainy[v] = i
            else:
                sunny.add(i)
                ans[i] = 1
        return ans
```

#### Java

```java
class Solution {
    public int[] avoidFlood(int[] rains) {
        int n = rains.length;
        int[] ans = new int[n];
        Arrays.fill(ans, -1);
        TreeSet<Integer> sunny = new TreeSet<>();
        Map<Integer, Integer> rainy = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            int v = rains[i];
            if (v > 0) {
                if (rainy.containsKey(v)) {
                    Integer t = sunny.higher(rainy.get(v));
                    if (t == null) {
                        return new int[0];
                    }
                    ans[t] = v;
                    sunny.remove(t);
                }
                rainy.put(v, i);
            } else {
                sunny.add(i);
                ans[i] = 1;
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
    vector<int> avoidFlood(vector<int>& rains) {
        int n = rains.size();
        vector<int> ans(n, -1);
        set<int> sunny;
        unordered_map<int, int> rainy;
        for (int i = 0; i < n; ++i) {
            int v = rains[i];
            if (v) {
                if (rainy.count(v)) {
                    auto it = sunny.upper_bound(rainy[v]);
                    if (it == sunny.end()) {
                        return {};
                    }
                    ans[*it] = v;
                    sunny.erase(it);
                }
                rainy[v] = i;
            } else {
                sunny.insert(i);
                ans[i] = 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func avoidFlood(rains []int) []int {
	n := len(rains)
	ans := make([]int, n)
	for i := range ans {
		ans[i] = -1
	}

	sunny := redblacktree.New[int, struct{}]()
	rainy := map[int]int{}

	for i, v := range rains {
		if v > 0 {
			if last, ok := rainy[v]; ok {
				node, found := sunny.Ceiling(last + 1)
				if !found {
					return []int{}
				}
				t := node.Key
				ans[t] = v
				sunny.Remove(t)
			}
			rainy[v] = i
		} else {
			sunny.Put(i, struct{}{})
			ans[i] = 1
		}
	}
	return ans
}
```

#### TypeScript

```ts
import { AvlTree } from 'datastructures-js';

function avoidFlood(rains: number[]): number[] {
    const n = rains.length;
    const ans = Array(n).fill(-1);
    const sunny = new AvlTree<number>((a, b) => a - b);
    const rainy = new Map<number, number>();

    for (let i = 0; i < n; ++i) {
        const v = rains[i];
        if (v > 0) {
            if (rainy.has(v)) {
                const last = rainy.get(v)!;
                const node = sunny.ceil(last + 1);
                if (!node) return [];
                const t = node.getValue();
                ans[t] = v;
                sunny.remove(t);
            }
            rainy.set(v, i);
        } else {
            sunny.insert(i);
            ans[i] = 1;
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::{BTreeSet, HashMap};

impl Solution {
    pub fn avoid_flood(rains: Vec<i32>) -> Vec<i32> {
        let n = rains.len();
        let mut ans = vec![-1; n];
        let mut sunny = BTreeSet::new();
        let mut rainy = HashMap::new();

        for (i, &v) in rains.iter().enumerate() {
            if v > 0 {
                if let Some(&last) = rainy.get(&v) {
                    if let Some(&t) = sunny.range((last + 1) as usize..).next() {
                        ans[t] = v;
                        sunny.remove(&t);
                    } else {
                        return vec![];
                    }
                }
                rainy.insert(v, i as i32);
            } else {
                sunny.insert(i);
                ans[i] = 1;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
