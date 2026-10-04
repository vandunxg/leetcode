---
comments: true
difficulty: Easy
rating: 1169
source: Weekly Contest 375 Q1
tags:
    - Array
    - Counting
    - Simulation
---

<!-- problem:start -->

# [2960. Count Tested Devices After Test Operations](https://leetcode.com/problems/count-tested-devices-after-test-operations)

[中文文档](/solution/2900-2999/2960.Count%20Tested%20Devices%20After%20Test%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>batteryPercentages</code> có độ dài <code>n</code>, biểu thị phần trăm pin của <code>n</code> thiết bị <strong>được đánh chỉ số từ 0</strong>.</p>

<p>Nhiệm vụ của bạn là kiểm tra từng thiết bị <code>i</code> <strong>theo thứ tự</strong> từ <code>0</code> đến <code>n - 1</code>, bằng cách thực hiện các thao tác kiểm tra sau:</p>

<ul>
	<li>Nếu <code>batteryPercentages[i]</code> <strong>lớn hơn</strong> <code>0</code>:

    <ul>
    	<li><strong>Tăng</strong> số lượng thiết bị đã kiểm tra.</li>
    	<li><strong>Giảm</strong> phần trăm pin của tất cả thiết bị có chỉ số <code>j</code> trong đoạn <code>[i + 1, n - 1]</code> đi <code>1</code>, đảm bảo phần trăm pin của chúng <strong>không bao giờ nhỏ hơn</strong> <code>0</code>, tức là <code>batteryPercentages[j] = max(0, batteryPercentages[j] - 1)</code>.</li>
    	<li>Chuyển sang thiết bị tiếp theo.</li>
    </ul>
    </li>
    <li>Nếu không, chuyển sang thiết bị tiếp theo mà không thực hiện kiểm tra.</li>

</ul>

<p>Trả về <em>một số nguyên biểu thị số lượng thiết bị sẽ được kiểm tra sau khi thực hiện các thao tác kiểm tra theo thứ tự.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> batteryPercentages = [1,1,2,1,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Thực hiện các thao tác kiểm tra theo thứ tự, bắt đầu từ thiết bị 0:
Tại thiết bị 0, batteryPercentages[0] &gt; 0, nên hiện có 1 thiết bị đã được kiểm tra, và batteryPercentages trở thành [1,0,1,0,2].
Tại thiết bị 1, batteryPercentages[1] == 0, nên chuyển sang thiết bị tiếp theo mà không kiểm tra.
Tại thiết bị 2, batteryPercentages[2] &gt; 0, nên hiện có 2 thiết bị đã được kiểm tra, và batteryPercentages trở thành [1,0,1,0,1].
Tại thiết bị 3, batteryPercentages[3] == 0, nên chuyển sang thiết bị tiếp theo mà không kiểm tra.
Tại thiết bị 4, batteryPercentages[4] &gt; 0, nên hiện có 3 thiết bị đã được kiểm tra, và batteryPercentages không thay đổi.
Vì vậy, đáp án là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> batteryPercentages = [0,1,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Thực hiện các thao tác kiểm tra theo thứ tự, bắt đầu từ thiết bị 0:
Tại thiết bị 0, batteryPercentages[0] == 0, nên chuyển sang thiết bị tiếp theo mà không kiểm tra.
Tại thiết bị 1, batteryPercentages[1] &gt; 0, nên hiện có 1 thiết bị đã được kiểm tra, và batteryPercentages trở thành [0,1,1].
Tại thiết bị 2, batteryPercentages[2] &gt; 0, nên hiện có 2 thiết bị đã được kiểm tra, và batteryPercentages không thay đổi.
Vì vậy, đáp án là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == batteryPercentages.length &lt;= 100 </code></li>
	<li><code>0 &lt;= batteryPercentages[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Khi kiểm tra một thiết bị vẫn còn pin, pin của mọi thiết bị phía sau sẽ giảm đi. Số thiết bị đã kiểm tra, $ans$, chính là tổng số lần giảm trước thiết bị hiện tại, vì vậy thiết bị có thể được kiểm tra khi và chỉ khi $x>ans$.
>
> $n \le 100$; chỉ cần cộng dồn điều kiện này mà không cần cập nhật lại mảng.

<!-- thinking:end -->

Giả sử số thiết bị hiện tại đã kiểm tra là $ans$. Khi kiểm tra thiết bị mới $i$, lượng pin còn lại của thiết bị đó là $\max(0, batteryPercentages[i] - ans)$. Nếu lượng pin còn lại lớn hơn $0$, điều đó có nghĩa là thiết bị này có thể được kiểm tra, và ta cần tăng $ans$ thêm $1$.

Cuối cùng, trả về $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countTestedDevices(self, batteryPercentages: List[int]) -> int:
        ans = 0
        for x in batteryPercentages:
            ans += x > ans
        return ans
```

#### Java

```java
class Solution {
    public int countTestedDevices(int[] batteryPercentages) {
        int ans = 0;
        for (int x : batteryPercentages) {
            ans += x > ans ? 1 : 0;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countTestedDevices(vector<int>& batteryPercentages) {
        int ans = 0;
        for (int x : batteryPercentages) {
            ans += x > ans;
        }
        return ans;
    }
};
```

#### Go

```go
func countTestedDevices(batteryPercentages []int) (ans int) {
	for _, x := range batteryPercentages {
		if x > ans {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countTestedDevices(batteryPercentages: number[]): number {
    let ans = 0;
    for (const x of batteryPercentages) {
        ans += x > ans ? 1 : 0;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_tested_devices(battery_percentages: Vec<i32>) -> i32 {
        let mut ans = 0;
        for x in battery_percentages {
            ans += if x > ans { 1 } else { 0 };
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
