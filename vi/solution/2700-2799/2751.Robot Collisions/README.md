---
comments: true
difficulty: Hard
rating: 2091
source: Weekly Contest 351 Q4
tags:
    - Stack
    - Array
    - Sorting
    - Simulation
---

<!-- problem:start -->

# [2751. Robot Collisions](https://leetcode.com/problems/robot-collisions)

[中文文档](/solution/2700-2799/2751.Robot%20Collisions/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> robot được <strong>đánh số từ 1</strong>, mỗi robot có vị trí trên một đường thẳng, độ bền và hướng di chuyển.</p>

<p>Cho các mảng số nguyên được đánh chỉ số từ <strong>0</strong> là <code>positions</code>, <code>healths</code> và một chuỗi <code>directions</code> (<code>directions[i]</code> là <strong>&#39;L&#39;</strong> biểu thị <strong>trái</strong> hoặc <strong>&#39;R&#39;</strong> biểu thị <strong>phải</strong>). Tất cả các giá trị trong <code>positions</code> đều <strong>khác nhau</strong>.</p>

<p>Tất cả robot bắt đầu di chuyển trên đường thẳng <strong>đồng thời</strong> với <strong>cùng tốc độ </strong>theo hướng tương ứng. Nếu hai robot có cùng vị trí trong khi di chuyển, chúng sẽ <strong>va chạm</strong>.</p>

<p>Nếu hai robot va chạm, robot có <strong>độ bền thấp hơn</strong> sẽ bị <strong>loại khỏi</strong> đường thẳng, còn độ bền của robot kia <strong>giảm</strong> <strong>đi một</strong>. Robot sống sót tiếp tục di chuyển theo <strong>cùng</strong> hướng trước đó. Nếu hai robot có <strong>cùng</strong> độ bền, cả hai đều bị<strong> </strong>loại khỏi đường thẳng.</p>

<p>Nhiệm vụ của bạn là xác định <strong>độ bền</strong> của các robot sống sót sau các va chạm, theo <strong>thứ tự </strong>các robot được cho, <strong> </strong>tức là độ bền cuối cùng của robot 1 (nếu còn sống), độ bền cuối cùng của robot 2 (nếu còn sống), v.v. Nếu không còn robot nào sống sót, trả về một mảng rỗng.</p>

<p>Trả về <em>một mảng chứa độ bền của các robot còn lại (theo thứ tự chúng được cho trong đầu vào), sau khi không thể xảy ra thêm va chạm nào.</em></p>

<p><strong>Lưu ý:</strong> Các vị trí có thể không được sắp xếp.</p>

<div class="notranslate" style="all: initial;">&nbsp;</div>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img height="169" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2751.Robot%20Collisions/images/image-20230516011718-12.png" width="808" /></p>

<pre>
<strong>Đầu vào:</strong> positions = [5,4,3,2,1], healths = [2,17,9,15,10], directions = &quot;RRRRR&quot;
<strong>Đầu ra:</strong> [2,17,9,15,10]
<strong>Giải thích:</strong> Không xảy ra va chạm trong ví dụ này vì tất cả robot đều di chuyển cùng hướng. Do đó, độ bền của các robot theo thứ tự ban đầu được trả về là [2, 17, 9, 15, 10].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img height="176" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2751.Robot%20Collisions/images/image-20230516004433-7.png" width="717" /></p>

<pre>
<strong>Đầu vào:</strong> positions = [3,5,2,6], healths = [10,10,15,12], directions = &quot;RLRL&quot;
<strong>Đầu ra:</strong> [14]
<strong>Giải thích:</strong> Có 2 va chạm trong ví dụ này. Trước hết, robot 1 và robot 2 sẽ va chạm, vì cả hai có cùng độ bền nên đều bị loại khỏi đường thẳng. Tiếp theo, robot 3 và robot 4 sẽ va chạm; vì độ bền của robot 4 nhỏ hơn nên robot 4 bị loại, còn độ bền của robot 3 trở thành 15 - 1 = 14. Chỉ robot 3 còn sống, nên ta trả về [14].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img height="172" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2751.Robot%20Collisions/images/image-20230516005114-9.png" width="732" /></p>

<pre>
<strong>Đầu vào:</strong> positions = [1,2,5,6], healths = [10,10,11,11], directions = &quot;RLRL&quot;
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Robot 1 và robot 2 sẽ va chạm; vì cả hai có cùng độ bền nên đều bị loại. Robot 3 và robot 4 cũng sẽ va chạm; vì cả hai có cùng độ bền nên đều bị loại. Do đó, ta trả về một mảng rỗng, [].</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= positions.length == healths.length == directions.length == n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= positions[i], healths[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>directions[i] == &#39;L&#39;</code> hoặc <code>directions[i] == &#39;R&#39;</code></li>
	<li>Tất cả giá trị trong <code>positions</code> đều khác nhau</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng bằng stack

<!-- thinking:start -->

> **Tư duy**
>
> Các robot trên một đường thẳng va chạm trực diện: robot yếu hơn bị loại, robot mạnh hơn mất một đơn vị độ bền, còn hai robot có cùng độ bền đều bị loại. Ta cần trả về độ bền còn lại theo thứ tự ban đầu. Mô phỏng theo thời gian sẽ cần xử lý từng lần gặp nhau, trong khi các vị trí có thể rất lớn.
>
> Sắp xếp robot theo vị trí. Đưa các robot di chuyển sang phải vào một stack; robot di chuyển sang trái sẽ lần lượt đối đầu với phần tử trên cùng cho đến khi nó bị loại hoặc stack không còn robot di chuyển sang phải. Các chỉ số ban đầu của những robot có độ bền dương là các robot sống sót.

<!-- thinking:end -->

Trước tiên, ta sắp xếp các robot theo thứ tự tăng dần của vị trí và lưu các chỉ số robot sau khi sắp xếp vào mảng $\textit{idx}$. Sau đó, ta dùng một stack để mô phỏng quá trình va chạm:

1. Duyệt các chỉ số robot $i$ trong $\textit{idx}$ từ trái sang phải. Nếu $directions[i]$ di chuyển sang phải, đưa $i$ vào stack.
2. Nếu $directions[i]$ di chuyển sang trái, nó va chạm với robot di chuyển sang phải ở đỉnh stack cho đến khi stack rỗng hoặc robot hiện tại bị loại.
    - Nếu độ bền của robot ở đỉnh lớn hơn robot hiện tại, robot hiện tại bị loại và độ bền của robot ở đỉnh giảm đi 1.
    - Nếu độ bền của robot ở đỉnh nhỏ hơn robot hiện tại, robot ở đỉnh bị loại, độ bền của robot hiện tại giảm đi 1, rồi robot hiện tại tiếp tục va chạm với robot mới ở đỉnh stack.
    - Nếu hai robot có cùng độ bền, cả hai đều bị loại.

Cuối cùng, ta trả về độ bền của tất cả robot có độ bền lớn hơn 0.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số robot.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def survivedRobotsHealths(self, positions, healths, directions):
        idx = sorted(range(len(positions)), key=lambda i: positions[i])
        stk = []

        for i in idx:
            if directions[i] == "R":
                stk.append(i)
                continue

            while stk and healths[i]:
                j = stk[-1]
                if healths[j] > healths[i]:
                    healths[j] -= 1
                    healths[i] = 0
                elif healths[j] < healths[i]:
                    healths[i] -= 1
                    healths[j] = 0
                    stk.pop()
                else:
                    healths[i] = healths[j] = 0
                    stk.pop()
                    break

        return [h for h in healths if h > 0]
```

#### Java

```java
class Solution {
    public List<Integer> survivedRobotsHealths(int[] positions, int[] healths, String directions) {
        int n = positions.length;
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, (a, b) -> positions[a] - positions[b]);

        Deque<Integer> stk = new ArrayDeque<>();

        for (int i : idx) {
            if (directions.charAt(i) == 'R') {
                stk.push(i);
                continue;
            }

            while (!stk.isEmpty() && healths[i] > 0) {
                int j = stk.peek();

                if (healths[j] > healths[i]) {
                    healths[j]--;
                    healths[i] = 0;
                } else if (healths[j] < healths[i]) {
                    healths[i]--;
                    healths[j] = 0;
                    stk.pop();
                } else {
                    healths[i] = healths[j] = 0;
                    stk.pop();
                    break;
                }
            }
        }

        List<Integer> ans = new ArrayList<>();
        for (int h : healths) {
            if (h > 0) {
                ans.add(h);
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
    vector<int> survivedRobotsHealths(vector<int>& positions, vector<int>& healths, string directions) {
        int n = positions.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);

        sort(idx.begin(), idx.end(), [&](int a, int b) {
            return positions[a] < positions[b];
        });

        vector<int> stk;

        for (int i : idx) {
            if (directions[i] == 'R') {
                stk.push_back(i);
                continue;
            }

            while (!stk.empty() && healths[i] > 0) {
                int j = stk.back();

                if (healths[j] > healths[i]) {
                    healths[j]--;
                    healths[i] = 0;
                } else if (healths[j] < healths[i]) {
                    healths[i]--;
                    healths[j] = 0;
                    stk.pop_back();
                } else {
                    healths[i] = healths[j] = 0;
                    stk.pop_back();
                    break;
                }
            }
        }

        vector<int> ans;
        for (int h : healths) {
            if (h > 0) {
                ans.push_back(h);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func survivedRobotsHealths(positions []int, healths []int, directions string) []int {
	n := len(positions)
	idx := make([]int, n)
	for i := range idx {
		idx[i] = i
	}

	sort.Slice(idx, func(i, j int) bool {
		return positions[idx[i]] < positions[idx[j]]
	})

	stk := []int{}

	for _, i := range idx {
		if directions[i] == 'R' {
			stk = append(stk, i)
			continue
		}

		for len(stk) > 0 && healths[i] > 0 {
			j := stk[len(stk)-1]

			if healths[j] > healths[i] {
				healths[j]--
				healths[i] = 0
			} else if healths[j] < healths[i] {
				healths[i]--
				healths[j] = 0
				stk = stk[:len(stk)-1]
			} else {
				healths[i], healths[j] = 0, 0
				stk = stk[:len(stk)-1]
				break
			}
		}
	}

	ans := []int{}
	for _, h := range healths {
		if h > 0 {
			ans = append(ans, h)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function survivedRobotsHealths(
    positions: number[],
    healths: number[],
    directions: string,
): number[] {
    const n = positions.length;
    const idx = Array.from({ length: n }, (_, i) => i).sort((a, b) => positions[a] - positions[b]);

    const stk: number[] = [];

    for (const i of idx) {
        if (directions[i] === 'R') {
            stk.push(i);
            continue;
        }

        while (stk.length && healths[i] > 0) {
            const j = stk[stk.length - 1];

            if (healths[j] > healths[i]) {
                healths[j]--;
                healths[i] = 0;
            } else if (healths[j] < healths[i]) {
                healths[i]--;
                healths[j] = 0;
                stk.pop();
            } else {
                healths[i] = healths[j] = 0;
                stk.pop();
                break;
            }
        }
    }

    return healths.filter(h => h > 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn survived_robots_healths(
        positions: Vec<i32>,
        mut healths: Vec<i32>,
        directions: String
    ) -> Vec<i32> {
        let n = positions.len();
        let mut idx: Vec<usize> = (0..n).collect();

        idx.sort_by_key(|&i| positions[i]);

        let dirs = directions.as_bytes();
        let mut stk: Vec<usize> = Vec::new();

        for &i in &idx {
            if dirs[i] == b'R' {
                stk.push(i);
                continue;
            }

            while let Some(&j) = stk.last() {
                if healths[i] == 0 {
                    break;
                }

                if healths[j] > healths[i] {
                    healths[j] -= 1;
                    healths[i] = 0;
                    break;
                } else if healths[j] < healths[i] {
                    healths[i] -= 1;
                    healths[j] = 0;
                    stk.pop();
                } else {
                    healths[i] = 0;
                    healths[j] = 0;
                    stk.pop();
                    break;
                }
            }
        }

        healths.into_iter().filter(|&h| h > 0).collect()
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} positions
 * @param {number[]} healths
 * @param {string} directions
 * @return {number[]}
 */
var survivedRobotsHealths = function (positions, healths, directions) {
    const n = positions.length;
    const idx = Array.from({ length: n }, (_, i) => i).sort((a, b) => positions[a] - positions[b]);

    const stk = [];

    for (const i of idx) {
        if (directions[i] === 'R') {
            stk.push(i);
            continue;
        }

        while (stk.length && healths[i] > 0) {
            const j = stk[stk.length - 1];

            if (healths[j] > healths[i]) {
                healths[j]--;
                healths[i] = 0;
            } else if (healths[j] < healths[i]) {
                healths[i]--;
                healths[j] = 0;
                stk.pop();
            } else {
                healths[i] = healths[j] = 0;
                stk.pop();
                break;
            }
        }
    }

    return healths.filter(h => h > 0);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
