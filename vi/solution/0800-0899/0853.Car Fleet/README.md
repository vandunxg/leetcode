---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [853. Car Fleet](https://leetcode.com/problems/car-fleet)

[中文文档](/solution/0800-0899/0853.Car%20Fleet/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> chiếc xe ở các vị trí cách mốc xuất phát 0 một số dặm nhất định, đang di chuyển đến mốc <code>target</code>.</p>

<p>Cho hai mảng số nguyên&nbsp;<code>position</code> và <code>speed</code>, đều có độ dài <code>n</code>. Trong đó, <code>position[i]</code> là vị trí xuất phát tính bằng dặm của chiếc xe thứ <code>i<sup>th</sup></code>, còn <code>speed[i]</code> là tốc độ của chiếc xe thứ <code>i<sup>th</sup></code> tính bằng dặm/giờ.</p>

<p>Xe không thể vượt xe khác, nhưng có thể đuổi kịp rồi chạy sát bên với tốc độ của xe chậm hơn.</p>

<p><strong>Đoàn xe</strong> gồm một chiếc xe hoặc một nhóm xe chạy sát bên nhau. Tốc độ của đoàn xe bằng tốc độ <strong>thấp nhất</strong> trong nhóm.</p>

<p>Nếu một chiếc xe đuổi kịp đoàn xe tại mốc <code>target</code>, xe đó vẫn được tính là một phần của đoàn xe.</p>

<p>Hãy trả về số đoàn xe sẽ đến đích.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">target = 12, position = [10,8,0,5,3], speed = [2,4,1,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Hai xe xuất phát tại 10 (tốc độ 2) và 8 (tốc độ 4) nhập thành một đoàn xe khi gặp nhau tại 12. Đoàn xe hình thành ở <code>target</code>.</li>
	<li>Xe xuất phát tại 0 (tốc độ 1) không đuổi kịp xe nào khác nên tạo thành một đoàn riêng.</li>
	<li>Hai xe xuất phát tại 5 (tốc độ 1) và 3 (tốc độ 3) nhập thành một đoàn khi gặp nhau tại 6. Đoàn xe di chuyển với tốc độ 1 cho đến khi đến <code>target</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">target = 10, position = [3], speed = [3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>
Chỉ có một chiếc xe nên cũng chỉ có một đoàn xe.</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">target = 100, position = [0,2,4], speed = [4,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Hai xe xuất phát tại 0 (tốc độ 4) và 2 (tốc độ 2) nhập thành một đoàn khi gặp nhau tại 4. Xe xuất phát tại 4 (tốc độ 1) tiếp tục đi đến 5.</li>
	<li>Sau đó, đoàn xe ở vị trí 4 (tốc độ 2) và xe ở vị trí 5 (tốc độ 1) nhập thành một đoàn khi gặp nhau tại 6. Đoàn xe di chuyển với tốc độ 1 cho đến khi đến <code>target</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == position.length == speed.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt; target &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= position[i] &lt; target</code></li>
	<li>Tất cả giá trị trong <code>position</code> đều <strong>khác nhau</strong>.</li>
	<li><code>0 &lt; speed[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xe ở gần đích hơn không thể bị vượt; xe phía sau chạy nhanh hơn sẽ đuổi kịp và nhập vào cùng đoàn. Vì $n\le 10^5$, mô phỏng các lần vượt/đuổi kịp sẽ chậm.
>
> Sắp xếp vị trí theo thứ tự từ gần đích về phía sau rồi xét thời gian đến nơi. Xe có thời gian đến lâu hơn đoàn phía trước sẽ tạo đoàn mới; nếu không, xe nhập vào đoàn đó. Số xe dẫn đầu các đoàn chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        idx = sorted(range(len(position)), key=lambda i: position[i])
        ans = pre = 0
        for i in idx[::-1]:
            t = (target - position[i]) / speed[i]
            if t > pre:
                ans += 1
                pre = t
        return ans
```

#### Java

```java
class Solution {
    public int carFleet(int target, int[] position, int[] speed) {
        int n = position.length;
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, (i, j) -> position[j] - position[i]);
        int ans = 0;
        double pre = 0;
        for (int i : idx) {
            double t = 1.0 * (target - position[i]) / speed[i];
            if (t > pre) {
                ++ans;
                pre = t;
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
    int carFleet(int target, vector<int>& position, vector<int>& speed) {
        int n = position.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) {
            return position[i] > position[j];
        });
        int ans = 0;
        double pre = 0;
        for (int i : idx) {
            double t = 1.0 * (target - position[i]) / speed[i];
            if (t > pre) {
                ++ans;
                pre = t;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func carFleet(target int, position []int, speed []int) (ans int) {
	n := len(position)
	idx := make([]int, n)
	for i := range idx {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool { return position[idx[j]] < position[idx[i]] })
	var pre float64
	for _, i := range idx {
		t := float64(target-position[i]) / float64(speed[i])
		if t > pre {
			ans++
			pre = t
		}
	}
	return
}
```

#### TypeScript

```ts
function carFleet(target: number, position: number[], speed: number[]): number {
    const n = position.length;
    const idx = Array(n)
        .fill(0)
        .map((_, i) => i)
        .sort((i, j) => position[j] - position[i]);
    let ans = 0;
    let pre = 0;
    for (const i of idx) {
        const t = (target - position[i]) / speed[i];
        if (t > pre) {
            ++ans;
            pre = t;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
