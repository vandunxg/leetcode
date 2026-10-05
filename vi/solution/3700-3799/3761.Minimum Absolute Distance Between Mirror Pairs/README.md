---
comments: true
difficulty: Medium
rating: 1668
source: Weekly Contest 478 Q3
tags:
    - Array
    - Hash Table
    - Math
---

<!-- problem:start -->

# [3761. Minimum Absolute Distance Between Mirror Pairs](https://leetcode.com/problems/minimum-absolute-distance-between-mirror-pairs)

[中文文档](/solution/3700-3799/3761.Minimum%20Absolute%20Distance%20Between%20Mirror%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <strong>cặp đối xứng</strong> là một cặp chỉ số <code>(i, j)</code> thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; nums.length</code>, và</li>
	<li><code>reverse(nums[i]) == nums[j]</code>, trong đó <code>reverse(x)</code> biểu thị số nguyên tạo thành bằng cách đảo ngược các chữ số của <code>x</code>. Các số 0 ở đầu sẽ được bỏ qua sau khi đảo ngược, ví dụ <code>reverse(120) = 21</code>.</li>
</ul>

<p>Hãy trả về <strong>khoảng cách tuyệt đối nhỏ nhất</strong> giữa các chỉ số của bất kỳ cặp đối xứng nào. Khoảng cách tuyệt đối giữa các chỉ số <code>i</code> và <code>j</code> là <code>abs(i - j)</code>.</p>

<p>Nếu không tồn tại cặp đối xứng, hãy trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [12,21,45,33,54]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cặp đối xứng là:</p>

<ul>
	<li>(0, 1) vì <code>reverse(nums[0]) = reverse(12) = 21 = nums[1]</code>, cho khoảng cách tuyệt đối <code>abs(0 - 1) = 1</code>.</li>
	<li>(2, 4) vì <code>reverse(nums[2]) = reverse(45) = 54 = nums[4]</code>, cho khoảng cách tuyệt đối <code>abs(2 - 4) = 2</code>.</li>
</ul>

<p>Khoảng cách tuyệt đối nhỏ nhất trong tất cả các cặp là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [120,21]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có một cặp đối xứng (0, 1) vì <code>reverse(nums[0]) = reverse(120) = 21 = nums[1]</code>.</p>

<p>Khoảng cách tuyệt đối nhỏ nhất là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [21,120]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cặp đối xứng nào trong mảng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp đối xứng thỏa mãn $\mathrm{reverse}(nums[i])=nums[j]$. Giá trị $j$ gần nhất là chỉ số trước đó lớn nhất của giá trị đã đảo ngược này, nên chỉ cần duyệt từ trái sang phải với một map ánh xạ từ $\mathrm{reverse}(x)$ đến chỉ số cuối cùng của nó.

<!-- thinking:end -->

Ta có thể dùng một hash table $\textit{pos}$ để ghi lại vị trí xuất hiện cuối cùng của mỗi số đã đảo ngược.

Trước tiên, ta khởi tạo đáp án $\textit{ans} = n + 1$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

Tiếp theo, ta duyệt qua mảng $\textit{nums}$. Với mỗi chỉ số $i$ và số tương ứng $x = \textit{nums}[i]$, nếu khóa $x$ tồn tại trong $\textit{pos}$, điều đó có nghĩa là tồn tại một chỉ số $j$ sao cho số thu được khi đảo ngược $\textit{nums}[j]$ bằng $x$. Khi đó, ta cập nhật đáp án thành $\min(\textit{ans}, i - \textit{pos}[x])$. Sau đó, ta cập nhật $\textit{pos}[\text{reverse}(x)]$ thành $i$. Ta tiếp tục quá trình này cho đến khi duyệt hết mảng.

Cuối cùng, nếu đáp án $\textit{ans}$ vẫn bằng $n + 1$, điều đó có nghĩa là không tồn tại cặp đối xứng nào, và ta trả về $-1$; ngược lại, ta trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là độ dài của mảng $\textit{nums}$, còn $M$ là giá trị lớn nhất trong mảng. Độ phức tạp không gian là $O(n)$, dùng để lưu hash table $\textit{pos}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMirrorPairDistance(self, nums: List[int]) -> int:
        def reverse(x: int) -> int:
            y = 0
            while x:
                v = x % 10
                y = y * 10 + v
                x //= 10
            return y

        pos = {}
        ans = inf
        for i, x in enumerate(nums):
            if x in pos:
                ans = min(ans, i - pos[x])
            pos[reverse(x)] = i
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    public int minMirrorPairDistance(int[] nums) {
        int n = nums.length;
        Map<Integer, Integer> pos = new HashMap<>(n);
        int ans = n + 1;
        for (int i = 0; i < n; ++i) {
            if (pos.containsKey(nums[i])) {
                ans = Math.min(ans, i - pos.get(nums[i]));
            }
            pos.put(reverse(nums[i]), i);
        }
        return ans > n ? -1 : ans;
    }

    private int reverse(int x) {
        int y = 0;
        for (; x > 0; x /= 10) {
            y = y * 10 + x % 10;
        }
        return y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMirrorPairDistance(vector<int>& nums) {
        int n = nums.size();
        int ans = n + 1;
        unordered_map<int, int> pos;
        auto reverse = [](int x) {
            int y = 0;
            for (; x > 0; x /= 10) {
                y = y * 10 + x % 10;
            }
            return y;
        };
        for (int i = 0; i < n; ++i) {
            if (pos.contains(nums[i])) {
                ans = min(ans, i - pos[nums[i]]);
            }
            pos[reverse(nums[i])] = i;
        }
        return ans > n ? -1 : ans;
    }
};
```

#### Go

```go
func minMirrorPairDistance(nums []int) int {
	n := len(nums)
	pos := map[int]int{}
	ans := n + 1
	reverse := func(x int) int {
		y := 0
		for ; x > 0; x /= 10 {
			y = y*10 + x%10
		}
		return y
	}
	for i, x := range nums {
		if j, ok := pos[x]; ok {
			ans = min(ans, i-j)
		}
		pos[reverse(x)] = i
	}
	if ans > n {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minMirrorPairDistance(nums: number[]): number {
    const n = nums.length;
    const pos = new Map<number, number>();
    let ans = n + 1;
    const reverse = (x: number) => {
        let y = 0;
        for (; x > 0; x = Math.floor(x / 10)) {
            y = y * 10 + (x % 10);
        }
        return y;
    };
    for (let i = 0; i < n; ++i) {
        if (pos.has(nums[i])) {
            const j = pos.get(nums[i])!;
            ans = Math.min(ans, i - j);
        }
        pos.set(reverse(nums[i]), i);
    }
    return ans > n ? -1 : ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn min_mirror_pair_distance(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut ans = n as i32 + 1;
        let mut pos: HashMap<i32, i32> = HashMap::new();

        fn reverse(mut x: i32) -> i32 {
            let mut y = 0;
            while x > 0 {
                y = y * 10 + x % 10;
                x /= 10;
            }
            y
        }

        for (i, &v) in nums.iter().enumerate() {
            if let Some(&j) = pos.get(&v) {
                ans = ans.min(i as i32 - j);
            }
            pos.insert(reverse(v), i as i32);
        }

        if ans > n as i32 { -1 } else { ans }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
