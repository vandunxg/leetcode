---
comments: true
difficulty: Medium
tags:
    - Array
---

<!-- problem:start -->

# [665. Non-decreasing Array](https://leetcode.com/problems/non-decreasing-array)

[中文文档](/solution/0600-0699/0665.Non-decreasing%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> có <code>n</code> phần tử. Hãy kiểm tra xem có thể làm mảng trở thành không giảm bằng cách sửa đổi <strong>tối đa một phần tử</strong> hay không.</p>

<p>Một mảng được gọi là không giảm nếu <code>nums[i] &lt;= nums[i + 1]</code> đúng với mọi <code>i</code> (đánh chỉ số từ <strong>0</strong>) thỏa mãn <code>0 &lt;= i &lt;= n - 2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,3]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Bạn có thể sửa số 4 đầu tiên thành 1 để được mảng không giảm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,1]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể tạo mảng không giảm bằng cách sửa đổi tối đa một phần tử.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Thay đổi tối đa một giá trị để mảng không giảm. Nếu có hai vị trí giảm thì không thể; nếu chỉ có một vị trí giảm, ta có thể sửa một trong hai phần tử ở hai bên.
>
> Tại vị trí đầu tiên có $a>b$, thử gán $a$ thành $b$ và gán $b$ thành $a$, rồi kiểm tra mảng đã được sắp xếp chưa. Nếu không có vị trí giảm nào thì mảng đã thỏa mãn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkPossibility(self, nums: List[int]) -> bool:
        def is_sorted(nums: List[int]) -> bool:
            return all(a <= b for a, b in pairwise(nums))

        n = len(nums)
        for i in range(n - 1):
            a, b = nums[i], nums[i + 1]
            if a > b:
                nums[i] = b
                if is_sorted(nums):
                    return True
                nums[i] = nums[i + 1] = a
                return is_sorted(nums)
        return True
```

#### Java

```java
class Solution {
    public boolean checkPossibility(int[] nums) {
        for (int i = 0; i < nums.length - 1; ++i) {
            int a = nums[i], b = nums[i + 1];
            if (a > b) {
                nums[i] = b;
                if (isSorted(nums)) {
                    return true;
                }
                nums[i] = a;
                nums[i + 1] = a;
                return isSorted(nums);
            }
        }
        return true;
    }

    private boolean isSorted(int[] nums) {
        for (int i = 0; i < nums.length - 1; ++i) {
            if (nums[i] > nums[i + 1]) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n - 1; ++i) {
            int a = nums[i], b = nums[i + 1];
            if (a > b) {
                nums[i] = b;
                if (is_sorted(nums.begin(), nums.end())) {
                    return true;
                }
                nums[i] = a;
                nums[i + 1] = a;
                return is_sorted(nums.begin(), nums.end());
            }
        }
        return true;
    }
};
```

#### Go

```go
func checkPossibility(nums []int) bool {
	isSorted := func(nums []int) bool {
		for i, b := range nums[1:] {
			if nums[i] > b {
				return false
			}
		}
		return true
	}
	for i := 0; i < len(nums)-1; i++ {
		a, b := nums[i], nums[i+1]
		if a > b {
			nums[i] = b
			if isSorted(nums) {
				return true
			}
			nums[i] = a
			nums[i+1] = a
			return isSorted(nums)
		}
	}
	return true
}
```

#### TypeScript

```ts
function checkPossibility(nums: number[]): boolean {
    const isSorted = (nums: number[]) => {
        for (let i = 0; i < nums.length - 1; ++i) {
            if (nums[i] > nums[i + 1]) {
                return false;
            }
        }
        return true;
    };
    for (let i = 0; i < nums.length - 1; ++i) {
        const a = nums[i],
            b = nums[i + 1];
        if (a > b) {
            nums[i] = b;
            if (isSorted(nums)) {
                return true;
            }
            nums[i] = a;
            nums[i + 1] = a;
            return isSorted(nums);
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
