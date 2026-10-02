---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - String
    - Sorting
---

<!-- problem:start -->

# [539. Minimum Time Difference](https://leetcode.com/problems/minimum-time-difference)

[中文文档](/solution/0500-0599/0539.Minimum%20Time%20Difference/README.md)

## Mô tả

<!-- description:start -->

Cho danh sách các mốc thời gian theo định dạng 24 giờ <strong>&quot;HH:MM&quot;</strong>, hãy trả về <em>độ chênh lệch nhỏ nhất tính bằng <b>phút</b> giữa hai mốc bất kỳ trong danh sách</em>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> timePoints = ["23:59","00:00"]
<strong>Đầu ra:</strong> 1
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> timePoints = ["00:00","23:59","00:00"]
<strong>Đầu ra:</strong> 0
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= timePoints.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>timePoints[i]</code> is in the format <strong>&quot;HH:MM&quot;</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi sắp xếp, khoảng cách nhỏ nhất nằm giữa hai thời điểm liền kề, hoặc giữa thời điểm cuối và thời điểm đầu khi đi qua nửa đêm. Chỉ có $1440$ phút phân biệt trong một ngày, nên nếu có hơn $1440$ mốc thì chắc chắn có mốc trùng nhau và đáp án là $0$.
>
> Đổi các mốc thành phút, sắp xếp, thêm giá trị đầu tiên cộng $1440$ vào cuối danh sách rồi lấy hiệu nhỏ nhất giữa hai giá trị liền kề.

<!-- thinking:end -->

Một ngày có tối đa $24 \times 60 = 1440$ mốc thời gian khác nhau. Vì vậy, nếu danh sách $timePoints$ dài hơn $1440$, chắc chắn có mốc bị trùng và ta có thể trả về $0$ ngay.

Tiếp theo, duyệt danh sách mốc thời gian và đổi chúng thành số phút, lưu vào danh sách $nums$. Ví dụ, mốc `13:14` được đổi thành $13 \times 60 + 14$.

Sau đó, sắp xếp danh sách số phút theo thứ tự tăng dần và thêm $nums[0] + 1440$ vào cuối. Bước này xử lý trường hợp đặc biệt tính chênh lệch giữa giá trị lớn nhất và nhỏ nhất qua nửa đêm.

Cuối cùng, duyệt danh sách số phút để tìm độ chênh lệch nhỏ nhất giữa hai thời điểm liền kề.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số mốc thời gian.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMinDifference(self, timePoints: List[str]) -> int:
        if len(timePoints) > 1440:
            return 0
        nums = sorted(int(x[:2]) * 60 + int(x[3:]) for x in timePoints)
        nums.append(nums[0] + 1440)
        return min(b - a for a, b in pairwise(nums))
```

#### Java

```java
class Solution {
    public int findMinDifference(List<String> timePoints) {
        if (timePoints.size() > 1440) {
            return 0;
        }
        int n = timePoints.size();
        int[] nums = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            String[] t = timePoints.get(i).split(":");
            nums[i] = Integer.parseInt(t[0]) * 60 + Integer.parseInt(t[1]);
        }
        Arrays.sort(nums, 0, n);
        nums[n] = nums[0] + 1440;
        int ans = 1 << 30;
        for (int i = 1; i <= n; ++i) {
            ans = Math.min(ans, nums[i] - nums[i - 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMinDifference(vector<string>& timePoints) {
        if (timePoints.size() > 1440) {
            return 0;
        }
        int n = timePoints.size();
        vector<int> nums(n + 1);
        for (int i = 0; i < n; ++i) {
            int hours = stoi(timePoints[i].substr(0, 2));
            int minutes = stoi(timePoints[i].substr(3, 2));
            nums[i] = hours * 60 + minutes;
        }
        sort(nums.begin(), nums.begin() + n);
        nums[n] = nums[0] + 1440;
        int ans = INT_MAX;
        for (int i = 1; i <= n; ++i) {
            ans = min(ans, nums[i] - nums[i - 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func findMinDifference(timePoints []string) int {
	if len(timePoints) > 1440 {
		return 0
	}

	n := len(timePoints)
	nums := make([]int, n+1)
	for i, time := range timePoints {
		parts := strings.Split(time, ":")
		hours, _ := strconv.Atoi(parts[0])
		minutes, _ := strconv.Atoi(parts[1])
		nums[i] = hours*60 + minutes
	}

	sort.Ints(nums[:n])
	nums[n] = nums[0] + 1440

	ans := 1 << 30
	for i := 1; i <= n; i++ {
		ans = min(ans, nums[i]-nums[i-1])
	}

	return ans
}
```

#### TypeScript

```ts
function findMinDifference(timePoints: string[]): number {
    if (timePoints.length > 1440) {
        return 0;
    }
    const n = timePoints.length;
    const nums: number[] = Array(n + 1);
    for (let i = 0; i < n; ++i) {
        const [hours, minutes] = timePoints[i].split(':').map(Number);
        nums[i] = hours * 60 + minutes;
    }
    nums.sort((a, b) => a - b);
    nums[n] = nums[0] + 1440;
    let ans = 1 << 30;
    for (let i = 1; i <= n; ++i) {
        ans = Math.min(ans, nums[i] - nums[i - 1]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_min_difference(time_points: Vec<String>) -> i32 {
        if time_points.len() > 1440 {
            return 0;
        }

        let n = time_points.len();
        let mut nums: Vec<i32> = Vec::with_capacity(n + 1);

        for time in time_points.iter() {
            let parts: Vec<i32> = time.split(':').map(|s| s.parse().unwrap()).collect();
            let minutes = parts[0] * 60 + parts[1];
            nums.push(minutes);
        }

        nums.sort();
        nums.push(nums[0] + 1440);

        let mut ans = i32::MAX;
        for i in 1..=n {
            ans = ans.min(nums[i] - nums[i - 1]);
        }

        ans
    }
}
```

#### Swift

```swift
class Solution {
    func findMinDifference(_ timePoints: [String]) -> Int {
        if timePoints.count > 1440 {
            return 0
        }

        var nums = [Int]()

        for t in timePoints {
            let time = t.split(separator: ":").map { Int($0)! }
            nums.append(time[0] * 60 + time[1])
        }

        nums.sort()
        nums.append(nums[0] + 1440)

        var ans = Int.max
        for i in 1..<nums.count {
            ans = min(ans, nums[i] - nums[i - 1])
        }

        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
