---
comments: true
difficulty: Hard
rating: 2453
source: Weekly Contest 318 Q4
tags:
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [2463. Minimum Total Distance Traveled](https://leetcode.com/problems/minimum-total-distance-traveled)

[中文文档](/solution/2400-2499/2463.Minimum%20Total%20Distance%20Traveled/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số robot và nhà máy trên trục X. Cho một mảng số nguyên <code>robot</code>, trong đó <code>robot[i]</code> là vị trí của robot <code>i<sup>th</sup></code>. Ngoài ra, cho một mảng số nguyên 2 chiều <code>factory</code>, trong đó <code>factory[j] = [position<sub>j</sub>, limit<sub>j</sub>]</code> cho biết <code>position<sub>j</sub></code> là vị trí của nhà máy <code>j<sup>th</sup></code> và nhà máy <code>j<sup>th</sup></code> có thể sửa chữa nhiều nhất <code>limit<sub>j</sub></code> robot.</p>

<p>Vị trí của mỗi robot là <strong>duy nhất</strong>. Vị trí của mỗi nhà máy cũng <strong>duy nhất</strong>. Lưu ý rằng ban đầu một robot có thể <strong>ở cùng vị trí</strong> với một nhà máy.</p>

<p>Ban đầu tất cả robot đều bị hỏng; chúng liên tục di chuyển theo một hướng. Hướng đó có thể là hướng âm hoặc hướng dương của trục X. Khi một robot đến một nhà máy chưa đạt giới hạn, nhà máy sẽ sửa robot và robot dừng lại.</p>

<p><strong>Vào bất kỳ thời điểm nào</strong>, bạn có thể đặt hướng di chuyển ban đầu cho <strong>một số</strong> robot. Mục tiêu là tối thiểu hóa tổng quãng đường mà tất cả robot đã di chuyển.</p>

<p>Trả về <em>tổng quãng đường di chuyển nhỏ nhất của tất cả robot</em>. Dữ liệu kiểm thử được tạo sao cho tất cả robot đều có thể được sửa chữa.</p>

<p><strong>Lưu ý rằng</strong></p>

<ul>
	<li>Tất cả robot di chuyển với cùng tốc độ.</li>
	<li>Nếu hai robot di chuyển cùng hướng, chúng sẽ không bao giờ va chạm.</li>
	<li>Nếu hai robot di chuyển ngược hướng và gặp nhau tại một điểm, chúng không va chạm mà đi xuyên qua nhau.</li>
	<li>Nếu một robot đi qua một nhà máy đã đạt giới hạn, nó sẽ đi xuyên qua nhà máy đó như thể nhà máy không tồn tại.</li>
	<li>Nếu robot di chuyển từ vị trí <code>x</code> đến vị trí <code>y</code>, quãng đường nó đã di chuyển là <code>|y - x|</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2463.Minimum%20Total%20Distance%20Traveled/images/example1.jpg" style="width: 500px; height: 320px;" />
<pre>
<strong>Đầu vào:</strong> robot = [0,4,6], factory = [[2,2],[6,2]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Như trong hình:
- Robot đầu tiên ở vị trí 0 di chuyển theo hướng dương. Nó sẽ được sửa chữa tại nhà máy đầu tiên.
- Robot thứ hai ở vị trí 4 di chuyển theo hướng âm. Nó sẽ được sửa chữa tại nhà máy đầu tiên.
- Robot thứ ba ở vị trí 6 sẽ được sửa chữa tại nhà máy thứ hai. Nó không cần di chuyển.
Giới hạn của nhà máy đầu tiên là 2 và nhà máy này đã sửa 2 robot.
Giới hạn của nhà máy thứ hai là 2 và nhà máy này đã sửa 1 robot.
Tổng quãng đường là |2 - 0| + |2 - 4| + |6 - 6| = 4. Có thể chứng minh rằng không thể đạt được tổng quãng đường nhỏ hơn 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2463.Minimum%20Total%20Distance%20Traveled/images/example-2.jpg" style="width: 500px; height: 329px;" />
<pre>
<strong>Đầu vào:</strong> robot = [1,-1], factory = [[-2,1],[2,1]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Như trong hình:
- Robot đầu tiên ở vị trí 1 di chuyển theo hướng dương. Nó sẽ được sửa chữa tại nhà máy thứ hai.
- Robot thứ hai ở vị trí -1 di chuyển theo hướng âm. Nó sẽ được sửa chữa tại nhà máy đầu tiên.
Giới hạn của nhà máy đầu tiên là 1 và nhà máy này đã sửa 1 robot.
Giới hạn của nhà máy thứ hai là 1 và nhà máy này đã sửa 1 robot.
Tổng quãng đường là |2 - 1| + |(-2) - (-1)| = 2. Có thể chứng minh rằng không thể đạt được tổng quãng đường nhỏ hơn 2.
</pre>

<p>&nbsp;</p>
<p><strong>Các ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= robot.length, factory.length &lt;= 100</code></li>
	<li><code>factory[j].length == 2</code></li>
	<li><code>-10<sup>9</sup> &lt;= robot[i], position<sub>j</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= limit<sub>j</sub> &lt;= robot.length</code></li>
	<li>Đầu vào luôn được tạo sao cho có thể sửa chữa tất cả robot.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều nhất $100$ robot và nhà máy, mỗi nhà máy có một giới hạn. Sau khi sắp xếp, một phép ghép tối ưu không giao nhau: các robot liền kề được đưa đến cùng một nhà máy hoặc các nhà máy liền kề nhau.
>
> $dfs(i,j)$ là khoảng cách nhỏ nhất tính từ robot $i$ và nhà máy $j$: bỏ qua nhà máy này, hoặc sửa $0..limit$ robot tiếp theo tại đó. Các trạng thái có ghi nhớ là $O(mn)$ nhân với giới hạn.

<!-- thinking:end -->

Trước tiên, chúng ta sắp xếp robot và nhà máy theo thứ tự tăng dần. Sau đó, định nghĩa hàm $dfs(i, j)$ biểu diễn tổng quãng đường di chuyển nhỏ nhất bắt đầu từ robot thứ $i$ và nhà máy thứ $j$.

Với $dfs(i, j)$, nếu nhà máy thứ $j$ không sửa robot, thì $dfs(i, j) = dfs(i, j+1)$. Nếu nhà máy thứ $j$ sửa robot, chúng ta có thể duyệt qua số robot được nhà máy thứ $j$ sửa và tìm tổng quãng đường di chuyển nhỏ nhất. Cụ thể, $dfs(i, j) = \min(dfs(i + k + 1, j + 1) + \sum_{t = 0}^{k} |robot[i + t] - factory[j][0]|)$.

Độ phức tạp thời gian là $O(m^2 \times n)$ và độ phức tạp không gian là $O(m \times n)$. Ở đây, $m$ và $n$ lần lượt là số robot và số nhà máy.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTotalDistance(self, robot: List[int], factory: List[List[int]]) -> int:
        @cache
        def dfs(i, j):
            if i == len(robot):
                return 0
            if j == len(factory):
                return inf
            ans = dfs(i, j + 1)
            t = 0
            for k in range(factory[j][1]):
                if i + k == len(robot):
                    break
                t += abs(robot[i + k] - factory[j][0])
                ans = min(ans, t + dfs(i + k + 1, j + 1))
            return ans

        robot.sort()
        factory.sort()
        ans = dfs(0, 0)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private Long[][] f;
    private List<Integer> robot;
    private int[][] factory;

    public long minimumTotalDistance(List<Integer> robot, int[][] factory) {
        Collections.sort(robot);
        Arrays.sort(factory, (a, b) -> a[0] - b[0]);
        this.robot = robot;
        this.factory = factory;
        f = new Long[robot.size()][factory.length];
        return dfs(0, 0);
    }

    private long dfs(int i, int j) {
        if (i == robot.size()) {
            return 0;
        }
        if (j == factory.length) {
            return Long.MAX_VALUE / 1000;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        long ans = dfs(i, j + 1);
        long t = 0;
        for (int k = 0; k < factory[j][1]; ++k) {
            if (i + k == robot.size()) {
                break;
            }
            t += Math.abs(robot.get(i + k) - factory[j][0]);
            ans = Math.min(ans, t + dfs(i + k + 1, j + 1));
        }
        f[i][j] = ans;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumTotalDistance(vector<int>& robot, vector<vector<int>>& factory) {
        ranges::sort(robot);
        ranges::sort(factory);
        using ll = long long;
        vector<vector<ll>> f(robot.size(), vector<ll>(factory.size(), -1));

        auto dfs = [&](this auto&& dfs, int i, int j) -> ll {
            if (i == robot.size()) {
                return 0;
            }
            if (j == factory.size()) {
                return 1e15;
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            ll ans = dfs(i, j + 1);
            ll t = 0;
            for (int k = 0; k < factory[j][1]; ++k) {
                if (i + k >= robot.size()) {
                    break;
                }
                t += abs(robot[i + k] - factory[j][0]);
                ans = min(ans, t + dfs(i + k + 1, j + 1));
            }
            f[i][j] = ans;
            return ans;
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func minimumTotalDistance(robot []int, factory [][]int) int64 {
	sort.Ints(robot)
	sort.Slice(factory, func(i, j int) bool { return factory[i][0] < factory[j][0] })
	f := make([][]int, len(robot))
	for i := range f {
		f[i] = make([]int, len(factory))
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i == len(robot) {
			return 0
		}
		if j == len(factory) {
			return 1e15
		}
		if f[i][j] != -1 {
			return f[i][j]
		}
		ans := dfs(i, j+1)
		t := 0
		for k := 0; k < factory[j][1]; k++ {
			if i+k >= len(robot) {
				break
			}
			t += abs(robot[i+k] - factory[j][0])
			ans = min(ans, t+dfs(i+k+1, j+1))
		}
		f[i][j] = ans
		return ans
	}
	return int64(dfs(0, 0))
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minimumTotalDistance(robot: number[], factory: number[][]): number {
    robot.sort((a, b) => a - b);
    factory.sort((a, b) => a[0] - b[0]);

    const n = robot.length;
    const m = factory.length;
    const f: number[][] = Array.from({ length: n }, () => Array(m).fill(-1));

    const dfs = (i: number, j: number): number => {
        if (i === n) return 0;
        if (j === m) return 1e15;
        if (f[i][j] !== -1) return f[i][j];

        let ans = dfs(i, j + 1);
        let totalDist = 0;
        const [pos, capacity] = factory[j];

        for (let k = 0; k < capacity; k++) {
            if (i + k >= n) break;
            totalDist += Math.abs(robot[i + k] - pos);
            ans = Math.min(ans, totalDist + dfs(i + k + 1, j + 1));
        }

        return (f[i][j] = ans);
    };

    return dfs(0, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
