---
comments: true
difficulty: Hard
rating: 2530
source: Weekly Contest 230 Q4
tags:
    - Stack
    - Array
    - Math
    - Monotonic Stack
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1776. Car Fleet II](https://leetcode.com/problems/car-fleet-ii)

[中文文档](/solution/1700-1799/1776.Car%20Fleet%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> chiếc xe chạy với tốc độ khác nhau theo cùng một hướng trên đường một làn. Cho mảng <code>cars</code> độ dài <code>n</code>, trong đó <code>cars[i] = [position<sub>i</sub>, speed<sub>i</sub>]</code> biểu diễn:</p>

<ul>
<li><code>position<sub>i</sub></code> là khoảng cách tính bằng mét giữa chiếc xe thứ <code>i<sup>th</sup></code> và đầu đường. Đảm bảo <code>position<sub>i</sub> &lt; position<sub>i+1</sub></code>.</li>
<li><code>speed<sub>i</sub></code> là tốc độ ban đầu của chiếc xe thứ <code>i<sup>th</sup></code>, tính bằng mét trên giây.</li>
</ul>

<p>Để đơn giản, có thể coi các xe là những điểm chuyển động trên trục số. Hai xe va chạm khi ở cùng vị trí. Khi một xe va chạm với xe khác, chúng hợp lại thành một đoàn xe. Các xe trong đoàn mới có cùng vị trí và tốc độ, bằng tốc độ ban đầu của xe <strong>chậm nhất</strong> trong đoàn.</p>

<p>Trả về mảng <code>answer</code>, trong đó <code>answer[i]</code> là thời điểm tính bằng giây khi chiếc xe thứ <code>i<sup>th</sup></code> va chạm với xe kế tiếp, hoặc <code>-1</code> nếu xe không va chạm với xe kế tiếp. Chấp nhận đáp án sai khác đáp án thực tế không quá <code>10<sup>-5</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> cars = [[1,2],[2,1],[4,3],[7,2]]
<strong>Output:</strong> [1.00000,-1.00000,3.00000,-1.00000]
<strong>Explanation:</strong> After exactly one second, the first car will collide with the second car, and form a car fleet with speed 1 m/s. After exactly 3 seconds, the third car will collide with the fourth car, and form a car fleet with speed 2 m/s.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> cars = [[3,4],[5,4],[6,3],[9,1]]
<strong>Output:</strong> [2.00000,1.00000,1.50000,-1.00000]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= cars.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= position<sub>i</sub>, speed<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
	<li><code>position<sub>i</sub> &lt; position<sub>i+1</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các xe chạy sang phải và nhập vào đoàn chậm hơn khi va chạm. Xe nào bị một xe đâm phải chỉ phụ thuộc vào các xe bên phải. $n$ lớn nên cần một cấu trúc tuyến tính.
>
> Duyệt từ phải sang trái bằng stack chứa các ứng viên chưa bị một xe còn chậm hơn hấp thụ. Xe ở đỉnh chỉ bị bắt kịp khi nó chậm hơn; nếu thời điểm gặp nhau sau va chạm của chính xe ở đỉnh, xe đó biến mất trước và bị loại khỏi stack.
>
> Stack rỗng nghĩa là không có va chạm; nếu không, ghi thời điểm va chạm với phần tử mới ở đỉnh rồi đẩy xe hiện tại vào stack.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getCollisionTimes(self, cars: List[List[int]]) -> List[float]:
        stk = []
        n = len(cars)
        ans = [-1] * n
        for i in range(n - 1, -1, -1):
            while stk:
                j = stk[-1]
                if cars[i][1] > cars[j][1]:
                    t = (cars[j][0] - cars[i][0]) / (cars[i][1] - cars[j][1])
                    if ans[j] == -1 or t <= ans[j]:
                        ans[i] = t
                        break
                stk.pop()
            stk.append(i)
        return ans
```

#### Java

```java
class Solution {
    public double[] getCollisionTimes(int[][] cars) {
        int n = cars.length;
        double[] ans = new double[n];
        Arrays.fill(ans, -1.0);
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty()) {
                int j = stk.peek();
                if (cars[i][1] > cars[j][1]) {
                    double t = (cars[j][0] - cars[i][0]) * 1.0 / (cars[i][1] - cars[j][1]);
                    if (ans[j] < 0 || t <= ans[j]) {
                        ans[i] = t;
                        break;
                    }
                }
                stk.pop();
            }
            stk.push(i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<double> getCollisionTimes(vector<vector<int>>& cars) {
        int n = cars.size();
        vector<double> ans(n, -1.0);
        stack<int> stk;
        for (int i = n - 1; ~i; --i) {
            while (stk.size()) {
                int j = stk.top();
                if (cars[i][1] > cars[j][1]) {
                    double t = (cars[j][0] - cars[i][0]) * 1.0 / (cars[i][1] - cars[j][1]);
                    if (ans[j] < 0 || t <= ans[j]) {
                        ans[i] = t;
                        break;
                    }
                }
                stk.pop();
            }
            stk.push(i);
        }
        return ans;
    }
};
```

#### Go

```go
func getCollisionTimes(cars [][]int) []float64 {
	n := len(cars)
	ans := make([]float64, n)
	for i := range ans {
		ans[i] = -1.0
	}
	stk := []int{}
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 {
			j := stk[len(stk)-1]
			if cars[i][1] > cars[j][1] {
				t := float64(cars[j][0]-cars[i][0]) / float64(cars[i][1]-cars[j][1])
				if ans[j] < 0 || t <= ans[j] {
					ans[i] = t
					break
				}
			}
			stk = stk[:len(stk)-1]
		}
		stk = append(stk, i)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
