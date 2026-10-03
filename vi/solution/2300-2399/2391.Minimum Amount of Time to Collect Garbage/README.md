---
comments: true
difficulty: Medium
rating: 1455
source: Weekly Contest 308 Q3
tags:
    - Array
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [2391. Minimum Amount of Time to Collect Garbage](https://leetcode.com/problems/minimum-amount-of-time-to-collect-garbage)

[中文文档](/solution/2300-2399/2391.Minimum%20Amount%20of%20Time%20to%20Collect%20Garbage/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>garbage</code> <strong>được đánh chỉ số từ 0</strong>, trong đó <code>garbage[i]</code> biểu diễn các loại rác tại ngôi nhà thứ <code>i<sup>th</sup></code>. <code>garbage[i]</code> chỉ gồm các ký tự <code>&#39;M&#39;</code>, <code>&#39;P&#39;</code> và <code>&#39;G&#39;</code>, lần lượt biểu diễn một đơn vị rác kim loại, giấy và thủy tinh. Việc thu gom <strong>một</strong> đơn vị rác bất kỳ mất <code>1</code> phút.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>travel</code> <strong>được đánh chỉ số từ 0</strong>, trong đó <code>travel[i]</code> là số phút cần để đi từ ngôi nhà <code>i</code> đến ngôi nhà <code>i + 1</code>.</p>

<p>Trong thành phố có ba xe thu gom rác, mỗi xe phụ trách thu gom một loại rác. Mỗi xe bắt đầu tại ngôi nhà <code>0</code> và phải đi qua từng ngôi nhà <strong>theo thứ tự</strong>; tuy nhiên, chúng <strong>không</strong> cần đi qua mọi ngôi nhà.</p>

<p>Tại mỗi thời điểm chỉ được sử dụng <strong>một</strong> xe thu gom rác. Trong khi một xe đang di chuyển hoặc thu gom rác, hai xe còn lại <strong>không thể</strong> làm gì.</p>

<p>Hãy trả về <em>số phút <strong>ít nhất</strong> cần để thu gom toàn bộ rác.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> garbage = [&quot;G&quot;,&quot;P&quot;,&quot;GP&quot;,&quot;GG&quot;], travel = [2,4,3]
<strong>Đầu ra:</strong> 21
<strong>Giải thích:</strong>
Xe thu gom rác giấy:
1. Đi từ ngôi nhà 0 đến ngôi nhà 1
2. Thu gom rác giấy tại ngôi nhà 1
3. Đi từ ngôi nhà 1 đến ngôi nhà 2
4. Thu gom rác giấy tại ngôi nhà 2
Tổng cộng, xe mất 8 phút để thu gom toàn bộ rác giấy.
Xe thu gom rác thủy tinh:
1. Thu gom rác thủy tinh tại ngôi nhà 0
2. Đi từ ngôi nhà 0 đến ngôi nhà 1
3. Đi từ ngôi nhà 1 đến ngôi nhà 2
4. Thu gom rác thủy tinh tại ngôi nhà 2
5. Đi từ ngôi nhà 2 đến ngôi nhà 3
6. Thu gom rác thủy tinh tại ngôi nhà 3
Tổng cộng, xe mất 13 phút để thu gom toàn bộ rác thủy tinh.
Vì không có rác kim loại nên không cần xét xe thu gom rác kim loại.
Vậy tổng thời gian để thu gom toàn bộ rác là 8 + 13 = 21 phút.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> garbage = [&quot;MMM&quot;,&quot;PGM&quot;,&quot;GP&quot;], travel = [3,10]
<strong>Đầu ra:</strong> 37
<strong>Giải thích:</strong>
Xe thu gom rác kim loại mất 7 phút để thu gom toàn bộ rác kim loại.
Xe thu gom rác giấy mất 15 phút để thu gom toàn bộ rác giấy.
Xe thu gom rác thủy tinh mất 15 phút để thu gom toàn bộ rác thủy tinh.
Tổng thời gian để thu gom toàn bộ rác là 7 + 15 + 15 = 37 phút.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= garbage.length &lt;= 10<sup>5</sup></code></li>
	<li><code>garbage[i]</code> chỉ gồm các chữ cái <code>&#39;M&#39;</code>, <code>&#39;P&#39;</code> và <code>&#39;G&#39;</code>.</li>
	<li><code>1 &lt;= garbage[i].length &lt;= 10</code></li>
	<li><code>travel.length == garbage.length - 1</code></li>
	<li><code>1 &lt;= travel[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Ba xe thu gom lần lượt một loại rác và mỗi xe phải đi từ ngôi nhà $0$ đến ngôi nhà cuối cùng có loại rác đó. Thời gian thu gom là tổng số ký tự; thời gian di chuyển chỉ phụ thuộc vào chỉ số xa nhất. $n \le 10^5$.
>
> Một lần duyệt cộng độ dài các chuỗi và ghi lại các chỉ số cuối cùng. Cộng tổng tiền tố của $travel$ khi tiền tố kết thúc đúng tại ngôi nhà cuối cùng của một xe.

<!-- thinking:end -->

Theo mô tả bài toán, mỗi xe thu gom rác bắt đầu từ ngôi nhà $0$, thu gom một loại rác và di chuyển về phía trước theo thứ tự cho đến khi đến ngôi nhà có chỉ số mà loại rác này xuất hiện lần cuối.

Do đó, ta có thể dùng bảng băm $\textit{last}$ để ghi lại chỉ số ngôi nhà nơi mỗi loại rác xuất hiện lần cuối. Giả sử loại rác thứ $i$ xuất hiện lần cuối tại ngôi nhà thứ $j$, khi đó thời gian di chuyển cần thiết cho xe thứ $i$ là $\textit{travel}[0] + \textit{travel}[1] + \cdots + \textit{travel}[j-1]$. Lưu ý rằng nếu $j = 0$ thì không cần thời gian di chuyển. Cộng thời gian di chuyển của tất cả các xe, sau đó cộng tổng thời gian thu gom của từng loại rác, ta sẽ có đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(k)$, trong đó $n$ và $k$ lần lượt là số lượng và số loại rác. Trong bài toán này, $k = 3$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def garbageCollection(self, garbage: List[str], travel: List[int]) -> int:
        last = {}
        ans = 0
        for i, s in enumerate(garbage):
            ans += len(s)
            for c in s:
                last[c] = i
        ts = 0
        for i, t in enumerate(travel, 1):
            ts += t
            ans += sum(ts for j in last.values() if i == j)
        return ans
```

#### Java

```java
class Solution {
    public int garbageCollection(String[] garbage, int[] travel) {
        Map<Character, Integer> last = new HashMap<>(3);
        int ans = 0;
        for (int i = 0; i < garbage.length; ++i) {
            String s = garbage[i];
            ans += s.length();
            for (char c : s.toCharArray()) {
                last.put(c, i);
            }
        }
        int ts = 0;
        for (int i = 1; i <= travel.length; ++i) {
            ts += travel[i - 1];
            for (int j : last.values()) {
                if (i == j) {
                    ans += ts;
                }
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
    int garbageCollection(vector<string>& garbage, vector<int>& travel) {
        unordered_map<char, int> last;
        int ans = 0;
        for (int i = 0; i < garbage.size(); ++i) {
            auto& s = garbage[i];
            ans += s.size();
            for (char& c : s) {
                last[c] = i;
            }
        }
        int ts = 0;
        for (int i = 1; i <= travel.size(); ++i) {
            ts += travel[i - 1];
            for (auto& [_, j] : last) {
                if (i == j) {
                    ans += ts;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func garbageCollection(garbage []string, travel []int) (ans int) {
	last := map[byte]int{}
	for i, s := range garbage {
		ans += len(s)
		for j := range s {
			last[s[j]] = i
		}
	}
	ts := 0
	for i := 1; i <= len(travel); i++ {
		ts += travel[i-1]
		for _, j := range last {
			if i == j {
				ans += ts
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function garbageCollection(garbage: string[], travel: number[]): number {
    const last: Map<string, number> = new Map();
    let ans = 0;
    for (let i = 0; i < garbage.length; ++i) {
        const s = garbage[i];
        ans += s.length;
        for (const c of s) {
            last.set(c, i);
        }
    }
    let ts = 0;
    for (let i = 1; i <= travel.length; ++i) {
        ts += travel[i - 1];
        for (const [_, j] of last) {
            if (i === j) {
                ans += ts;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn garbage_collection(garbage: Vec<String>, travel: Vec<i32>) -> i32 {
        let mut last: HashMap<char, usize> = HashMap::new();
        let mut ans = 0;
        for (i, s) in garbage.iter().enumerate() {
            ans += s.len() as i32;
            for c in s.chars() {
                last.insert(c, i);
            }
        }
        let mut ts = 0;
        for (i, t) in travel.iter().enumerate() {
            ts += t;
            for &j in last.values() {
                if i + 1 == j {
                    ans += ts;
                }
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int GarbageCollection(string[] garbage, int[] travel) {
        Dictionary<char, int> last = new Dictionary<char, int>();
        int ans = 0;
        for (int i = 0; i < garbage.Length; ++i) {
            ans += garbage[i].Length;
            foreach (char c in garbage[i]) {
                last[c] = i;
            }
        }
        int ts = 0;
        for (int i = 1; i <= travel.Length; ++i) {
            ts += travel[i - 1];
            foreach (int j in last.Values) {
                if (i == j) {
                    ans += ts;
                }
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
