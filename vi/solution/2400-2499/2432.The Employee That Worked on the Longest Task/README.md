---
comments: true
difficulty: Easy
rating: 1266
source: Weekly Contest 314 Q1
tags:
    - Array
---

<!-- problem:start -->

# [2432. The Employee That Worked on the Longest Task](https://leetcode.com/problems/the-employee-that-worked-on-the-longest-task)

[中文文档](/solution/2400-2499/2432.The%20Employee%20That%20Worked%20on%20the%20Longest%20Task/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> nhân viên, mỗi người có một mã duy nhất từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Bạn được cung cấp một mảng số nguyên 2 chiều <code>logs</code>, trong đó <code>logs[i] = [id<sub>i</sub>, leaveTime<sub>i</sub>]</code>, với:</p>

<ul>
	<li><code>id<sub>i</sub></code> là mã của nhân viên đã thực hiện task thứ <code>i<sup>th</sup></code>, và</li>
	<li><code>leaveTime<sub>i</sub></code> là thời điểm nhân viên hoàn thành task thứ <code>i<sup>th</sup></code>. Tất cả các giá trị <code>leaveTime<sub>i</sub></code> đều <strong>khác nhau</strong>.</li>
</ul>

<p>Lưu ý rằng task thứ <code>i<sup>th</sup></code> bắt đầu ngay sau khi task thứ <code>(i - 1)<sup>th</sup></code> kết thúc, và task thứ <code>0<sup>th</sup></code> bắt đầu tại thời điểm <code>0</code>.</p>

<p>Trả về <em>mã của nhân viên đã thực hiện task trong thời gian lâu nhất</em>. Nếu có hai hoặc nhiều nhân viên hòa nhau, hãy trả về <em>mã <strong>nhỏ nhất</strong> trong số đó</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10, logs = [[0,3],[2,5],[0,9],[1,15]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Task 0 bắt đầu tại 0 và kết thúc tại 3, kéo dài 3 đơn vị thời gian.
Task 1 bắt đầu tại 3 và kết thúc tại 5, kéo dài 2 đơn vị thời gian.
Task 2 bắt đầu tại 5 và kết thúc tại 9, kéo dài 4 đơn vị thời gian.
Task 3 bắt đầu tại 9 và kết thúc tại 15, kéo dài 6 đơn vị thời gian.
Task có thời gian lâu nhất là task 3 và nhân viên có mã 1 là người đã thực hiện task đó, nên ta trả về 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 26, logs = [[1,1],[3,7],[2,12],[7,17]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Task 0 bắt đầu tại 0 và kết thúc tại 1, kéo dài 1 đơn vị thời gian.
Task 1 bắt đầu tại 1 và kết thúc tại 7, kéo dài 6 đơn vị thời gian.
Task 2 bắt đầu tại 7 và kết thúc tại 12, kéo dài 5 đơn vị thời gian.
Task 3 bắt đầu tại 12 và kết thúc tại 17, kéo dài 5 đơn vị thời gian.
Task có thời gian lâu nhất là task 1. Nhân viên đã thực hiện task đó là nhân viên 3, nên ta trả về 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, logs = [[0,10],[1,20]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Task 0 bắt đầu tại 0 và kết thúc tại 10, kéo dài 10 đơn vị thời gian.
Task 1 bắt đầu tại 10 và kết thúc tại 20, kéo dài 10 đơn vị thời gian.
Hai task 0 và 1 có thời gian lâu nhất. Nhân viên đã thực hiện chúng lần lượt là nhân viên 0 và 1, nên ta trả về mã nhỏ nhất là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 500</code></li>
	<li><code>1 &lt;= logs.length &lt;= 500</code></li>
	<li><code>logs[i].length == 2</code></li>
	<li><code>0 &lt;= id<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>1 &lt;= leaveTime<sub>i</sub> &lt;= 500</code></li>
	<li><code>id<sub>i</sub> != id<sub>i+1</sub></code></li>
	<li><code>leaveTime<sub>i</sub></code> được sắp xếp theo thứ tự tăng nghiêm ngặt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Vì $\textit{logs}$ tăng nghiêm ngặt theo thời gian rời đi, task thứ $i$ kéo dài bằng $leaveTime_i$ trừ đi thời điểm rời đi trước đó. Có nhiều nhất $500$ phần tử, nên chỉ cần một lần duyệt để theo dõi thời lượng dài nhất và mã nhỏ nhất.

<!-- thinking:end -->

Ta dùng biến $last$ để ghi nhận thời điểm kết thúc của task trước đó, biến $mx$ để ghi nhận thời gian làm việc dài nhất, và biến $ans$ để ghi nhận nhân viên có thời gian làm việc dài nhất và có $id$ nhỏ nhất. Ban đầu, cả ba biến đều bằng $0$.

Tiếp theo, ta duyệt mảng $logs$. Với mỗi nhân viên, ta lấy thời điểm nhân viên hoàn thành task trừ đi thời điểm kết thúc của task trước đó để tính thời gian làm việc $t$ của nhân viên này. Nếu $mx$ nhỏ hơn $t$, hoặc $mx$ bằng $t$ và $id$ của nhân viên này nhỏ hơn $ans$, ta cập nhật $mx$ và $ans$. Sau đó, ta cập nhật $last$ thành thời điểm kết thúc của task trước đó cộng với $t$. Tiếp tục duyệt cho đến khi duyệt hết mảng.

Cuối cùng, trả về đáp án $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $logs$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hardestWorker(self, n: int, logs: List[List[int]]) -> int:
        last = mx = ans = 0
        for uid, t in logs:
            t -= last
            if mx < t or (mx == t and ans > uid):
                ans, mx = uid, t
            last += t
        return ans
```

#### Java

```java
class Solution {
    public int hardestWorker(int n, int[][] logs) {
        int ans = 0;
        int last = 0, mx = 0;
        for (int[] log : logs) {
            int uid = log[0], t = log[1];
            t -= last;
            if (mx < t || (mx == t && ans > uid)) {
                ans = uid;
                mx = t;
            }
            last += t;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int hardestWorker(int n, vector<vector<int>>& logs) {
        int ans = 0, mx = 0, last = 0;
        for (auto& log : logs) {
            int uid = log[0], t = log[1];
            t -= last;
            if (mx < t || (mx == t && ans > uid)) {
                mx = t;
                ans = uid;
            }
            last += t;
        }
        return ans;
    }
};
```

#### Go

```go
func hardestWorker(n int, logs [][]int) (ans int) {
	var mx, last int
	for _, log := range logs {
		uid, t := log[0], log[1]
		t -= last
		if mx < t || (mx == t && uid < ans) {
			mx = t
			ans = uid
		}
		last += t
	}
	return
}
```

#### TypeScript

```ts
function hardestWorker(n: number, logs: number[][]): number {
    let [ans, mx, last] = [0, 0, 0];
    for (let [uid, t] of logs) {
        t -= last;
        if (mx < t || (mx == t && ans > uid)) {
            ans = uid;
            mx = t;
        }
        last += t;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn hardest_worker(n: i32, logs: Vec<Vec<i32>>) -> i32 {
        let mut res = 0;
        let mut max = 0;
        let mut pre = 0;
        for log in logs.iter() {
            let t = log[1] - pre;
            if t > max || (t == max && res > log[0]) {
                res = log[0];
                max = t;
            }
            pre = log[1];
        }
        res
    }
}
```

#### C

```c
#define min(a, b) (((a) < (b)) ? (a) : (b))

int hardestWorker(int n, int** logs, int logsSize, int* logsColSize) {
    int res = 0;
    int max = 0;
    int pre = 0;
    for (int i = 0; i < logsSize; i++) {
        int t = logs[i][1] - pre;
        if (t > max || (t == max && res > logs[i][0])) {
            res = logs[i][0];
            max = t;
        }
        pre = logs[i][1];
    }
    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 tính lại thời điểm kết thúc bằng cách cộng dồn. Lưu lại $leaveTime$ trước đó rồi lấy hiệu cũng cho cùng thời lượng, đồng thời giảm bớt một phép tính số học.

<!-- thinking:end -->

<!-- tabs:start -->

#### Rust

```rust
impl Solution {
    pub fn hardest_worker(n: i32, logs: Vec<Vec<i32>>) -> i32 {
        let mut ans = 0;
        let mut mx = 0;
        let mut last = 0;

        for log in logs {
            let uid = log[0];
            let t = log[1];

            let diff = t - last;
            last = t;

            if diff > mx || (diff == mx && uid < ans) {
                ans = uid;
                mx = diff;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
