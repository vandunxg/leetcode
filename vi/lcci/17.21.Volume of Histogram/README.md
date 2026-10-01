---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [17.21. Volume of Histogram](https://leetcode.cn/problems/volume-of-histogram-lcci)

[中文文档](/lcci/17.21.Volume%20of%20Histogram/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy hình dung một histogram (biểu đồ cột). Hãy thiết kế một thuật toán để tính thể tích nước mà histogram có thể chứa nếu đổ nước lên trên. Có thể giả sử rằng mỗi cột của histogram có chiều rộng bằng 1.</p>

![](https://fastly.jsdelivr.net/gh/doocs/leetcode@main/lcci/17.21.Volume%20of%20Histogram/images/rainwatertrap.png)

<p><small>Bản đồ độ cao ở trên được biểu diễn bằng mảng [0,1,0,2,1,0,1,3,2,1,2,1]. Trong trường hợp này, có thể giữ lại 6 đơn vị nước (phần màu xanh). Cảm ơn <strong>Marcos</strong> đã đóng góp hình ảnh này!</small></p>

<p><strong>Ví dụ:</strong></p>

<pre>

<strong>Đầu vào:</strong> [0,1,0,2,1,0,1,3,2,1,2,1]

<strong>Đầu ra:</strong> 6</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bài toán là giữ nước mưa trên một histogram. Nếu quét các biên cho từng cột, độ phức tạp có thể là bậc hai.
>
> Lượng nước tại $i$ bằng giá trị nhỏ hơn giữa cột cao nhất ở mỗi phía, trừ đi $height[i]$; các giá trị cực đại đó có thể được tính trước.
>
> $left[i]$ và $right[i]$ lần lượt là các giá trị cực đại prefix/suffix bao gồm cả vị trí hiện tại; cộng $\min(l,r)-h$. Nếu độ dài nhỏ hơn $3$ thì không chứa được nước.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        n = len(height)
        if n < 3:
            return 0
        left = [height[0]] * n
        right = [height[-1]] * n
        for i in range(1, n):
            left[i] = max(left[i - 1], height[i])
            right[n - i - 1] = max(right[n - i], height[n - i - 1])
        return sum(min(l, r) - h for l, r, h in zip(left, right, height))
```

#### Java

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        if (n < 3) {
            return 0;
        }
        int[] left = new int[n];
        int[] right = new int[n];
        left[0] = height[0];
        right[n - 1] = height[n - 1];
        for (int i = 1; i < n; ++i) {
            left[i] = Math.max(left[i - 1], height[i]);
            right[n - i - 1] = Math.max(right[n - i], height[n - i - 1]);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += Math.min(left[i], right[i]) - height[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        if (n < 3) {
            return 0;
        }
        int left[n], right[n];
        left[0] = height[0];
        right[n - 1] = height[n - 1];
        for (int i = 1; i < n; ++i) {
            left[i] = max(left[i - 1], height[i]);
            right[n - i - 1] = max(right[n - i], height[n - i - 1]);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += min(left[i], right[i]) - height[i];
        }
        return ans;
    }
};
```

#### Go

```go
func trap(height []int) (ans int) {
	n := len(height)
	if n < 3 {
		return 0
	}
	left := make([]int, n)
	right := make([]int, n)
	left[0], right[n-1] = height[0], height[n-1]
	for i := 1; i < n; i++ {
		left[i] = max(left[i-1], height[i])
		right[n-i-1] = max(right[n-i], height[n-i-1])
	}
	for i, h := range height {
		ans += min(left[i], right[i]) - h
	}
	return
}
```

#### TypeScript

```ts
function trap(height: number[]): number {
    const n = height.length;
    if (n < 3) {
        return 0;
    }
    const left: number[] = new Array(n).fill(height[0]);
    const right: number[] = new Array(n).fill(height[n - 1]);
    for (let i = 1; i < n; ++i) {
        left[i] = Math.max(left[i - 1], height[i]);
        right[n - i - 1] = Math.max(right[n - i], height[n - i - 1]);
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        ans += Math.min(left[i], right[i]) - height[i];
    }
    return ans;
}
```

#### C#

```cs
public class Solution {
    public int Trap(int[] height) {
        int n = height.Length;
        if (n < 3) {
            return 0;
        }
        int[] left = new int[n];
        int[] right = new int[n];
        left[0] = height[0];
        right[n - 1] = height[n - 1];
        for (int i = 1; i < n; ++i) {
            left[i] = Math.Max(left[i - 1], height[i]);
            right[n - i - 1] = Math.Max(right[n - i], height[n - i - 1]);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += Math.Min(left[i], right[i]) - height[i];
        }
        return ans;
    }
}
```

#### Swift

```swift
class Solution {
    func trap(_ height: [Int]) -> Int {
        let n = height.count
        if n < 3 {
            return 0
        }

        var left = [Int](repeating: 0, count: n)
        var right = [Int](repeating: 0, count: n)

        left[0] = height[0]
        right[n - 1] = height[n - 1]

        for i in 1..<n {
            left[i] = max(left[i - 1], height[i])
        }

        for i in stride(from: n - 2, through: 0, by: -1) {
            right[i] = max(right[i + 1], height[i])
        }

        var ans = 0
        for i in 0..<n {
            ans += min(left[i], right[i]) - height[i]
        }

        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
