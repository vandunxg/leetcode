---
comments: true
difficulty: Hard
tags:
    - Array
    - Hash Table
    - String
    - Counting
    - Prefix Sum
    - Simulation
---

<!-- problem:start -->

# [3279. Maximum Total Area Occupied by Pistons 🔒](https://leetcode.com/problems/maximum-total-area-occupied-by-pistons)

[中文文档](/solution/3200-3299/3279.Maximum%20Total%20Area%20Occupied%20by%20Pistons/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số piston trong động cơ của một chiếc xe cũ, và chúng ta muốn tính diện tích <strong>lớn nhất</strong> có thể <strong>bên dưới</strong> các piston.</p>

<p>Bạn được cho:</p>

<ul>
	<li>Một số nguyên <code>height</code>, biểu thị chiều cao <strong>lớn nhất</strong> mà một piston có thể đạt tới.</li>
	<li>Một mảng số nguyên <code>positions</code>, trong đó <code>positions[i]</code> là vị trí hiện tại của piston <code>i</code>, cũng chính là diện tích hiện tại <strong>bên dưới</strong> nó.</li>
	<li>Một chuỗi <code>directions</code>, trong đó <code>directions[i]</code> là hướng chuyển động hiện tại của piston <code>i</code>, <code>&#39;U&#39;</code> là đi lên và <code>&#39;D&#39;</code> là đi xuống.</li>
</ul>

<p>Mỗi giây:</p>

<ul>
	<li>Mỗi piston di chuyển 1 đơn vị theo hướng hiện tại. Ví dụ, nếu hướng là đi lên, <code>positions[i]</code> tăng thêm 1.</li>
	<li>Nếu một piston đã chạm một trong hai đầu, tức là <code>positions[i] == 0</code> hoặc <code>positions[i] == height</code>, hướng của nó sẽ thay đổi.</li>
</ul>

<p>Trả về <em>diện tích lớn nhất có thể</em> bên dưới tất cả các piston.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">height = 5, positions = [2,5], directions = &quot;UD&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vị trí hiện tại của các piston tạo ra diện tích lớn nhất có thể bên dưới chúng.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">height = 6, positions = [0,0,6,3], directions = &quot;UUDU&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau 3 giây, các piston sẽ ở các vị trí <code>[3, 3, 3, 6]</code>, tạo ra diện tích lớn nhất có thể bên dưới chúng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= height &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= positions.length == directions.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= positions[i] &lt;= height</code></li>
	<li><code>directions[i]</code> là một trong hai giá trị <code>&#39;U&#39;</code> hoặc <code>&#39;D&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các piston dao động trong $[0,height]$ với tốc độ đơn vị; diện tích là tổng các vị trí, tạo thành một đường gấp khúc theo thời gian. $n\le 10^5$ và $height\le 10^6$ khiến việc lặp qua từng giây là không khả thi. Vận tốc chỉ đổi dấu tại một đầu mút, nên số lượng sự kiện là nhỏ.
>
> Diện tích ban đầu là tổng các vị trí; vận tốc tổng là số piston đi lên trừ số piston đi xuống. Tại mỗi lần đổi hướng, cập nhật difference map bằng $\pm 2$. Duyệt các sự kiện theo thời gian, cập nhật diện tích trên mỗi đoạn có vận tốc không đổi và giữ lại giá trị lớn nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxArea(self, height: int, positions: List[int], directions: str) -> int:
        delta = defaultdict(int)
        diff = res = 0
        for pos, dir in zip(positions, directions):
            res += pos
            if dir == "U":
                diff += 1
                delta[height - pos] -= 2
                delta[height * 2 - pos] += 2
            else:
                diff -= 1
                delta[pos] += 2
                delta[height + pos] -= 2
        ans = res
        pre = 0
        for cur, d in sorted(delta.items()):
            res += (cur - pre) * diff
            pre = cur
            diff += d
            ans = max(ans, res)
        return ans
```

#### Java

```java
class Solution {
    public long maxArea(int height, int[] positions, String directions) {
        Map<Integer, Integer> delta = new TreeMap<>();
        int diff = 0;
        long res = 0;
        for (int i = 0; i < positions.length; ++i) {
            int pos = positions[i];
            char dir = directions.charAt(i);
            res += pos;
            if (dir == 'U') {
                ++diff;
                delta.merge(height - pos, -2, Integer::sum);
                delta.merge(height * 2 - pos, 2, Integer::sum);
            } else {
                --diff;
                delta.merge(pos, 2, Integer::sum);
                delta.merge(height + pos, -2, Integer::sum);
            }
        }
        long ans = res;
        int pre = 0;
        for (var e : delta.entrySet()) {
            int cur = e.getKey();
            int d = e.getValue();
            res += (long) (cur - pre) * diff;
            pre = cur;
            diff += d;
            ans = Math.max(ans, res);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxArea(int height, vector<int>& positions, string directions) {
        map<int, int> delta;
        int diff = 0;
        long long res = 0;

        for (int i = 0; i < positions.size(); ++i) {
            int pos = positions[i];
            char dir = directions[i];
            res += pos;

            if (dir == 'U') {
                ++diff;
                delta[height - pos] -= 2;
                delta[height * 2 - pos] += 2;
            } else {
                --diff;
                delta[pos] += 2;
                delta[height + pos] -= 2;
            }
        }

        long long ans = res;
        int pre = 0;

        for (const auto& [cur, d] : delta) {
            res += static_cast<long long>(cur - pre) * diff;
            pre = cur;
            diff += d;
            ans = max(ans, res);
        }

        return ans;
    }
};
```

#### Go

```go
func maxArea(height int, positions []int, directions string) int64 {
	delta := make(map[int]int)
	diff := 0
	var res int64 = 0
	for i, pos := range positions {
		dir := directions[i]
		res += int64(pos)

		if dir == 'U' {
			diff++
			delta[height-pos] -= 2
			delta[height*2-pos] += 2
		} else {
			diff--
			delta[pos] += 2
			delta[height+pos] -= 2
		}
	}
	ans := res
	pre := 0
	keys := make([]int, 0, len(delta))
	for key := range delta {
		keys = append(keys, key)
	}
	sort.Ints(keys)
	for _, cur := range keys {
		d := delta[cur]
		res += int64(cur-pre) * int64(diff)
		pre = cur
		diff += d
		ans = max(ans, res)
	}
	return ans
}
```

#### TypeScript

```ts
function maxArea(height: number, positions: number[], directions: string): number {
    const delta = new Map<number, number>();
    let diff = 0;
    let res = 0;
    for (let i = 0; i < positions.length; i++) {
        const pos = positions[i];
        const dir = directions[i];
        res += pos;
        if (dir === 'U') {
            diff++;
            delta.set(height - pos, (delta.get(height - pos) ?? 0) - 2);
            delta.set(height * 2 - pos, (delta.get(height * 2 - pos) ?? 0) + 2);
        } else {
            diff--;
            delta.set(pos, (delta.get(pos) ?? 0) + 2);
            delta.set(height + pos, (delta.get(height + pos) ?? 0) - 2);
        }
    }
    let ans = res;
    let pre = 0;
    const keys = [...delta.keys()].sort((a, b) => a - b);
    for (const cur of keys) {
        res += (cur - pre) * diff;
        pre = cur;
        diff += delta.get(cur)!;
        ans = Math.max(ans, res);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
