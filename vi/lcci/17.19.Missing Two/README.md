---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [17.19. Missing Two](https://leetcode.cn/problems/missing-two-lcci)

[中文文档](/lcci/17.19.Missing%20Two/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chứa tất cả các số từ 1 đến N, mỗi số xuất hiện đúng một lần, ngoại trừ hai số bị thiếu. Làm thế nào để tìm các số bị thiếu trong thời gian O(N) và không gian 0(1)?</p>

<p>Bạn có thể trả về các số bị thiếu theo bất kỳ thứ tự nào.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Đầu vào:</strong> [1]

<strong>Đầu ra: </strong>[2,3]</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong>Đầu vào:</strong> [2,3]

<strong>Đầu ra: </strong>[1,4]</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li><code>nums.length &lt;=&nbsp;30000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có hai số bị thiếu trong $1\ldots n$. Có thể dùng tổng và tổng các bình phương, nhưng bình phương rất dễ gây overflow.
>
> XOR của tất cả các giá trị là $a\oplus b$. `lowbit` chia chúng thành hai nhóm khác nhau (bit đó bằng $1$ ở một số và bằng $0$ ở số còn lại); XOR trong mỗi nhóm sẽ tách riêng từng số.
>
> Tính $xor$, sau đó XOR các giá trị mà bit tương ứng với $diff=xor\&(-xor)$ được bật để tìm $a$, rồi tính $b=xor\oplus a$. Thời gian tuyến tính, không gian hằng số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingTwo(self, nums: List[int]) -> List[int]:
        n = len(nums) + 2
        xor = 0
        for v in nums:
            xor ^= v
        for i in range(1, n + 1):
            xor ^= i

        diff = xor & (-xor)
        a = 0
        for v in nums:
            if v & diff:
                a ^= v
        for i in range(1, n + 1):
            if i & diff:
                a ^= i
        b = xor ^ a
        return [a, b]
```

#### Java

```java
class Solution {
    public int[] missingTwo(int[] nums) {
        int n = nums.length + 2;
        int xor = 0;
        for (int v : nums) {
            xor ^= v;
        }
        for (int i = 1; i <= n; ++i) {
            xor ^= i;
        }
        int diff = xor & (-xor);
        int a = 0;
        for (int v : nums) {
            if ((v & diff) != 0) {
                a ^= v;
            }
        }
        for (int i = 1; i <= n; ++i) {
            if ((i & diff) != 0) {
                a ^= i;
            }
        }
        int b = xor ^ a;
        return new int[] {a, b};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> missingTwo(vector<int>& nums) {
        int n = nums.size() + 2;
        int eor = 0;
        for (int v : nums) eor ^= v;
        for (int i = 1; i <= n; ++i) eor ^= i;

        int diff = eor & -eor;
        int a = 0;
        for (int v : nums)
            if (v & diff) a ^= v;
        for (int i = 1; i <= n; ++i)
            if (i & diff) a ^= i;
        int b = eor ^ a;
        return {a, b};
    }
};
```

#### Go

```go
func missingTwo(nums []int) []int {
	n := len(nums) + 2
	xor := 0
	for _, v := range nums {
		xor ^= v
	}
	for i := 1; i <= n; i++ {
		xor ^= i
	}
	diff := xor & -xor
	a := 0
	for _, v := range nums {
		if (v & diff) != 0 {
			a ^= v
		}
	}
	for i := 1; i <= n; i++ {
		if (i & diff) != 0 {
			a ^= i
		}
	}
	b := xor ^ a
	return []int{a, b}
}
```

#### Swift

```swift
class Solution {
    func missingTwo(_ nums: [Int]) -> [Int] {
        let n = nums.count + 2
        var xor = 0

        for num in nums {
            xor ^= num
        }

        for i in 1...n {
            xor ^= i
        }

        let diff = xor & (-xor)

        var a = 0

        for num in nums {
            if (num & diff) != 0 {
                a ^= num
            }
        }

        for i in 1...n {
            if (i & diff) != 0 {
                a ^= i
            }
        }

        let b = xor ^ a
        return [a, b]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
