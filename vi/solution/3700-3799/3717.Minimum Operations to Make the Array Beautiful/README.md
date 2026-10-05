---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3717. Minimum Operations to Make the Array Beautiful 🔒](https://leetcode.com/problems/minimum-operations-to-make-the-array-beautiful)

[中文文档](/solution/3700-3799/3717.Minimum%20Operations%20to%20Make%20the%20Array%20Beautiful/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Một mảng được gọi là <strong>đẹp</strong> nếu với mọi chỉ số <code>i &gt; 0</code>, giá trị tại <code>nums[i]</code> <strong>chia hết cho</strong> <code>nums[i - 1]</code>.</p>

<p>Trong một thao tác, bạn có thể <strong>tăng</strong> một phần tử bất kỳ <code>nums[i]</code> (với <code>i &gt; 0</code>) thêm <code>1</code>.</p>

<p>Trả về <strong>số thao tác ít nhất</strong> cần thực hiện để biến mảng thành mảng đẹp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,7,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thực hiện thao tác hai lần trên <code>nums[1]</code> sẽ biến mảng thành mảng đẹp: <code>[3,9,9]</code></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng đã cho vốn đã đẹp.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng chỉ có một phần tử nên đã đẹp.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50​​​</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các phần tử ở phía sau chỉ có thể tăng và phải trở thành bội số của phần tử đứng trước. Với $n\le 100$ và $nums[i]\le 50$, các giá trị sau khi tăng vẫn nằm trong một khoảng nhỏ. DP lưu giá trị được ghi tại chỉ số trước đó cùng với chi phí tương ứng; từ mỗi giá trị như vậy, ta thử các bội số của nó không nhỏ hơn $x$ và không vượt quá một cận không đổi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        f = {nums[0]: 0}
        for x in nums[1:]:
            g = {}
            for pre, s in f.items():
                cur = (x + pre - 1) // pre * pre
                while cur <= 100:
                    if cur not in g or g[cur] > s + cur - x:
                        g[cur] = s + cur - x
                    cur += pre
            f = g
        return min(f.values())
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        Map<Integer, Integer> f = new HashMap<>();
        f.put(nums[0], 0);

        for (int i = 1; i < nums.length; i++) {
            int x = nums[i];
            Map<Integer, Integer> g = new HashMap<>();

            for (var entry : f.entrySet()) {
                int pre = entry.getKey();
                int s = entry.getValue();

                int cur = (x + pre - 1) / pre * pre;
                while (cur <= 100) {
                    int val = s + (cur - x);
                    if (!g.containsKey(cur) || g.get(cur) > val) {
                        g.put(cur, val);
                    }
                    cur += pre;
                }
            }
            f = g;
        }

        int ans = Integer.MAX_VALUE;
        for (int v : f.values()) {
            ans = Math.min(ans, v);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums) {
        unordered_map<int, int> f;
        f[nums[0]] = 0;

        for (int i = 1; i < nums.size(); i++) {
            int x = nums[i];
            unordered_map<int, int> g;
            for (auto [pre, s] : f) {
                int cur = (x + pre - 1) / pre * pre;
                while (cur <= 100) {
                    int val = s + (cur - x);
                    auto jt = g.find(cur);
                    if (jt == g.end() || jt->second > val) {
                        g[cur] = val;
                    }
                    cur += pre;
                }
            }
            f = move(g);
        }

        int ans = INT_MAX;
        for (auto& it : f) {
            ans = min(ans, it.second);
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) int {
	f := map[int]int{nums[0]: 0}

	for i := 1; i < len(nums); i++ {
		x := nums[i]
		g := make(map[int]int)
		for pre, s := range f {
			cur := (x + pre - 1) / pre * pre
			for cur <= 100 {
				val := s + (cur - x)
				if old, ok := g[cur]; !ok || old > val {
					g[cur] = val
				}
				cur += pre
			}
		}
		f = g
	}

	ans := math.MaxInt32
	for _, v := range f {
		ans = min(ans, v)
	}
	return ans
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    let f = new Map<number, number>();
    f.set(nums[0], 0);

    for (let i = 1; i < nums.length; i++) {
        const x = nums[i];
        const g = new Map<number, number>();

        for (const [pre, s] of f.entries()) {
            let cur = Math.floor((x + pre - 1) / pre) * pre;
            while (cur <= 100) {
                const val = s + (cur - x);
                const old = g.get(cur);
                if (old === undefined || old > val) {
                    g.set(cur, val);
                }
                cur += pre;
            }
        }
        f = g;
    }

    return Math.min(...f.values());
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn min_operations(nums: Vec<i32>) -> i32 {
        let mut f: HashMap<i32, i32> = HashMap::new();
        f.insert(nums[0], 0);

        for i in 1..nums.len() {
            let x = nums[i];
            let mut g: HashMap<i32, i32> = HashMap::new();

            for (&pre, &s) in f.iter() {
                let mut cur = ((x + pre - 1) / pre) * pre;
                while cur <= 100 {
                    let val = s + (cur - x);
                    match g.get(&cur) {
                        None => {
                            g.insert(cur, val);
                        }
                        Some(&old) => {
                            if val < old {
                                g.insert(cur, val);
                            }
                        }
                    }
                    cur += pre;
                }
            }
            f = g;
        }

        *f.values().min().unwrap()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
