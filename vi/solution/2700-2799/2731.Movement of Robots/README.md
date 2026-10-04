---
comments: true
difficulty: Medium
rating: 1922
source: Biweekly Contest 106 Q3
tags:
    - Brainteaser
    - Array
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2731. Movement of Robots](https://leetcode.com/problems/movement-of-robots)

[中文文档](/solution/2700-2799/2731.Movement%20of%20Robots/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số robot đang đứng trên một trục số vô hạn, với tọa độ ban đầu được cho bởi một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Các robot sẽ bắt đầu di chuyển khi nhận được lệnh. Mỗi giây, robot di chuyển một đơn vị khoảng cách.</p>

<p>Bạn được cho một chuỗi <code>s</code> biểu thị hướng di chuyển của các robot khi nhận lệnh. <code>&#39;L&#39;</code> nghĩa là robot sẽ di chuyển về phía trái hoặc phía âm của trục số, còn <code>&#39;R&#39;</code> nghĩa là robot sẽ di chuyển về phía phải hoặc phía dương của trục số.</p>

<p>Nếu hai robot va chạm, chúng sẽ bắt đầu di chuyển theo hai hướng ngược nhau.</p>

<p>Hãy trả về <em>tổng khoảng cách giữa mọi cặp robot</em> <code>d</code> <em>giây sau khi nhận lệnh.</em> Vì tổng có thể rất lớn, hãy trả về kết quả theo modulo <code>10<sup>9</sup> + 7</code>.</p>

<p><b>Lưu ý: </b></p>

<ul>
	<li>Với hai robot ở chỉ số <code>i</code> và <code>j</code>, cặp <code>(i,j)</code> và cặp <code>(j,i)</code> được xem là cùng một cặp.</li>
	<li>Khi các robot va chạm, chúng <strong>lập tức đổi hướng</strong> mà không mất thời gian.</li>
	<li>Va chạm xảy ra&nbsp;khi hai robot ở cùng một vị trí tại cùng một&nbsp;thời điểm.
	<ul>
		<li>Ví dụ, nếu một robot ở vị trí 0 đang đi sang phải và một robot khác ở vị trí 2 đang đi sang trái, thì vào giây tiếp theo cả hai sẽ ở vị trí 1 và đổi hướng. Vào giây sau đó, robot thứ nhất sẽ ở vị trí 0, đi sang trái, còn robot kia sẽ ở vị trí 2, đi sang phải.</li>
		<li>Ví dụ,&nbsp;nếu một robot ở vị trí 0 đang đi sang phải và một robot khác ở vị trí 1&nbsp;đang đi sang trái, thì vào giây tiếp theo robot thứ nhất sẽ ở vị trí 0, đi sang trái, còn robot kia sẽ ở vị trí 1, đi sang phải.</li>
	</ul>
	</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-2,0,2], s = &quot;RLL&quot;, d = 3
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong>
Sau 1 giây, các vị trí là [-1,-1,1]. Bây giờ, robot ở chỉ số 0 sẽ đi sang trái, còn robot ở chỉ số 1 sẽ đi sang phải.
Sau 2 giây, các vị trí là [-2,0,0]. Bây giờ, robot ở chỉ số 1 sẽ đi sang trái, còn robot ở chỉ số 2 sẽ đi sang phải.
Sau 3 giây, các vị trí là [-3,-1,1].
Khoảng cách giữa robot ở chỉ số 0 và 1 là abs(-3 - (-1)) = 2.
Khoảng cách giữa robot ở chỉ số 0 và 2 là abs(-3 - 1) = 4.
Khoảng cách giữa robot ở chỉ số 1 và 2 là abs(-1 - 1) = 2.
Tổng khoảng cách của tất cả các cặp = 2 + 4 + 2 = 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,0], s = &quot;RL&quot;, d = 2
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Sau 1 giây, các vị trí là [2,-1].
Sau 2 giây, các vị trí là [3,-2].
Khoảng cách giữa hai robot là abs(-2 - 3) = 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-2 * 10<sup>9</sup>&nbsp;&lt;= nums[i] &lt;= 2 * 10<sup>9</sup></code></li>
	<li><code>0 &lt;= d &lt;= 10<sup>9</sup></code></li>
	<li><code>nums.length == s.length&nbsp;</code></li>
	<li><code>s</code> chỉ gồm &#39;L&#39; và &#39;R&#39;</li>
	<li><code>nums[i]</code>&nbsp;đôi một khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Suy luận nhanh + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Robot di chuyển trên một đường thẳng và đổi hướng khi va chạm; ta cần tính tổng khoảng cách giữa mọi cặp sau $d$ giây. Việc mô phỏng va chạm theo từng giây là không thể khi miền tọa độ lớn.
>
> Đổi hướng tương đương với việc đi xuyên qua nhau, vì vậy mỗi robot chỉ cần đi $d$ đơn vị theo hướng ban đầu. Sau khi sắp xếp, vị trí thứ $i$ đóng góp $i\cdot x_i$ trừ đi tổng tiền tố của các vị trí trước đó. Một lượt duyệt là đủ để cộng dồn tổng theo modulo đã cho.

<!-- thinking:end -->

Sau khi hai robot va chạm, chúng lập tức đổi hướng, tương đương với việc hai robot tiếp tục di chuyển theo hướng ban đầu. Vì vậy, ta duyệt mảng $nums$, dựa vào chỉ dẫn trong chuỗi $s$ để cộng hoặc trừ $d$ vào vị trí của mỗi robot, sau đó sắp xếp mảng $nums$.

Tiếp theo, ta duyệt vị trí của từng robot từ nhỏ đến lớn và tính tổng khoảng cách giữa robot hiện tại với tất cả robot đứng trước nó; đó chính là đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng robot.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumDistance(self, nums: List[int], s: str, d: int) -> int:
        mod = 10**9 + 7
        for i, c in enumerate(s):
            nums[i] += d if c == "R" else -d
        nums.sort()
        ans = s = 0
        for i, x in enumerate(nums):
            ans += i * x - s
            s += x
        return ans % mod
```

#### Java

```java
class Solution {
    public int sumDistance(int[] nums, String s, int d) {
        int n = nums.length;
        long[] arr = new long[n];
        for (int i = 0; i < n; ++i) {
            arr[i] = (long) nums[i] + (s.charAt(i) == 'L' ? -d : d);
        }
        Arrays.sort(arr);
        long ans = 0, sum = 0;
        final int mod = (int) 1e9 + 7;
        for (int i = 0; i < n; ++i) {
            ans = (ans + i * arr[i] - sum) % mod;
            sum += arr[i];
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumDistance(vector<int>& nums, string s, int d) {
        int n = nums.size();
        vector<long long> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = 1LL * nums[i] + (s[i] == 'L' ? -d : d);
        }
        sort(arr.begin(), arr.end());
        long long ans = 0;
        long long sum = 0;
        const int mod = 1e9 + 7;
        for (int i = 0; i < n; ++i) {
            ans = (ans + i * arr[i] - sum) % mod;
            sum += arr[i];
        }
        return ans;
    }
};
```

#### Go

```go
func sumDistance(nums []int, s string, d int) (ans int) {
	for i, c := range s {
		if c == 'R' {
			nums[i] += d
		} else {
			nums[i] -= d
		}
	}
	sort.Ints(nums)
	sum := 0
	const mod int = 1e9 + 7
	for i, x := range nums {
		ans = (ans + i*x - sum) % mod
		sum += x
	}
	return
}
```

#### TypeScript

```ts
function sumDistance(nums: number[], s: string, d: number): number {
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        nums[i] += s[i] === 'L' ? -d : d;
    }
    nums.sort((a, b) => a - b);
    let ans = 0;
    let sum = 0;
    const mod = 1e9 + 7;
    for (let i = 0; i < n; ++i) {
        ans = (ans + i * nums[i] - sum) % mod;
        sum += nums[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
