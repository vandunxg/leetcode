---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [17.10. Find Majority Element](https://leetcode.cn/problems/find-majority-element-lcci)

[中文文档](/lcci/17.10.Find%20Majority%20Element/README.md)

## Mô tả

<!-- description:start -->

<p>Phần tử chiếm đa số là phần tử xuất hiện nhiều hơn một nửa số phần tử trong một mảng. Cho một mảng số nguyên dương, hãy tìm phần tử chiếm đa số. Nếu không có phần tử chiếm đa số, trả về -1. Thực hiện trong thời gian O(N) và không gian O(1).</p>

<p><strong>Ví dụ 1: </strong></p>

<pre>

<strong>Đầu vào: </strong>[1,2,5,9,5,9,5,5,5]

<strong>Đầu ra: </strong>5</pre>

<p>&nbsp;</p>

<p><strong>Ví dụ 2: </strong></p>

<pre>

<strong>Đầu vào: </strong>[3,2]

<strong>Đầu ra: </strong>-1</pre>

<p>&nbsp;</p>

<p><strong>Ví dụ 3: </strong></p>

<pre>

<strong>Đầu vào: </strong>[2,2,1,1,1,2,2]

<strong>Đầu ra: </strong>2

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Phần tử chiếm đa số xuất hiện nhiều hơn một nửa số lần, nếu không thì đáp án là $-1$. Frequency map là cách đúng, nhưng dùng không gian tuyến tính.
>
> Boyer–Moore ghép từng cặp các giá trị khác nhau; nếu phần tử chiếm đa số tồn tại, nó sẽ là ứng viên cuối cùng.
>
> Lượt duyệt đầu tiên tạo ra $m$; `count` kiểm tra ngưỡng. Bộ đếm sẽ đặt lại ứng viên khi bằng 0, như thường lệ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        cnt = m = 0
        for v in nums:
            if cnt == 0:
                m, cnt = v, 1
            else:
                cnt += 1 if m == v else -1
        return m if nums.count(m) > len(nums) // 2 else -1
```

#### Java

```java
class Solution {
    public int majorityElement(int[] nums) {
        int cnt = 0, m = 0;
        for (int v : nums) {
            if (cnt == 0) {
                m = v;
                cnt = 1;
            } else {
                cnt += (m == v ? 1 : -1);
            }
        }
        cnt = 0;
        for (int v : nums) {
            if (m == v) {
                ++cnt;
            }
        }
        return cnt > nums.length / 2 ? m : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int cnt = 0, m = 0;
        for (int& v : nums) {
            if (cnt == 0) {
                m = v;
                cnt = 1;
            } else
                cnt += (m == v ? 1 : -1);
        }
        cnt = count(nums.begin(), nums.end(), m);
        return cnt > nums.size() / 2 ? m : -1;
    }
};
```

#### Go

```go
func majorityElement(nums []int) int {
	cnt, m := 0, 0
	for _, v := range nums {
		if cnt == 0 {
			m, cnt = v, 1
		} else {
			if m == v {
				cnt++
			} else {
				cnt--
			}
		}
	}
	cnt = 0
	for _, v := range nums {
		if m == v {
			cnt++
		}
	}
	if cnt > len(nums)/2 {
		return m
	}
	return -1
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function (nums) {
    let cnt = 0,
        m = 0;
    for (const v of nums) {
        if (cnt == 0) {
            m = v;
            cnt = 1;
        } else {
            cnt += m == v ? 1 : -1;
        }
    }
    cnt = 0;
    for (const v of nums) {
        if (m == v) {
            ++cnt;
        }
    }
    return cnt > nums.length / 2 ? m : -1;
};
```

#### C#

```cs
public class Solution {
    public int MajorityElement(int[] nums) {
        int cnt = 0, m = 0;
        foreach (int v in nums) {
            if (cnt == 0) {
                m = v;
                cnt = 1;
            }
            else {
                cnt += m == v ? 1 : -1;
            }
        }
        cnt = 0;
        foreach (int v in nums) {
            if (m == v) {
                ++cnt;
            }
        }
        return cnt > nums.Length / 2 ? m : -1;
    }
}
```

#### Swift

```swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        var count = 0
        var candidate: Int?

        for num in nums {
            if count == 0 {
                candidate = num
                count = 1
            } else if let candidate = candidate, candidate == num {
                count += 1
            } else {
                count -= 1
            }
        }

        count = 0
        if let candidate = candidate {
            for num in nums {
                if num == candidate {
                    count += 1
                }
            }
            if count > nums.count / 2 {
                return candidate
            }
        }

        return -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
