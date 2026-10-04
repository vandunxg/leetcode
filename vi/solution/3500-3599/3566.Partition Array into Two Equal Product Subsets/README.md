---
comments: true
difficulty: Medium
rating: 1459
source: Weekly Contest 452 Q1
tags:
    - Bit Manipulation
    - Recursion
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3566. Partition Array into Two Equal Product Subsets](https://leetcode.com/problems/partition-array-into-two-equal-product-subsets)

[中文文档](/solution/3500-3599/3566.Partition%20Array%20into%20Two%20Equal%20Product%20Subsets/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> chứa các số nguyên dương <strong>khác nhau</strong> và một số nguyên <code>target</code>.</p>

<p>Hãy xác định xem có thể chia <code>nums</code> thành hai <strong>tập con</strong> <strong>không rỗng</strong> <strong>không giao nhau</strong> hay không, sao cho mỗi phần tử thuộc về <strong>chính xác một</strong> tập con và tích các phần tử trong mỗi tập con đều bằng <code>target</code>.</p>

<p>Trả về <code>true</code> nếu tồn tại cách chia như vậy và <code>false</code> nếu không.</p>
Một <strong>tập con</strong> của một mảng là một lựa chọn các phần tử của mảng.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,6,8,4], target = 24</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong> Hai tập con <code>[3, 8]</code> và <code>[1, 6, 4]</code> đều có tích bằng 24. Do đó, kết quả là true.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,5,3,7], target = 15</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong> Không có cách nào chia <code>nums</code> thành hai tập con không rỗng, không giao nhau sao cho tích của cả hai tập con đều bằng 15. Do đó, kết quả là false.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>3 &lt;= nums.length &lt;= 12</code></li>
    <li><code>1 &lt;= target &lt;= 10<sup>15</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 100</code></li>
    <li>Tất cả các phần tử của <code>nums</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> $n$ rất nhỏ. Ta chia thành hai tập con có tích đều bằng $\textit{target}$, vì vậy chỉ cần xét $2^n$ cách phân chia.
>
> Với mỗi mask, ta nhân các phần tử của hai phía; nếu cả hai đều bằng $\textit{target}$ thì trả về kết quả thành công. Tập rỗng và tập đầy đủ sẽ thất bại, trừ khi tích của chúng tình cờ bằng nhau; phép kiểm tra tương tự vẫn bao quát trường hợp này.

<!-- thinking:end -->

Ta có thể dùng liệt kê nhị phân để kiểm tra tất cả các cách chia tập con. Với mỗi cách chia, ta tính tích của hai tập con rồi kiểm tra xem cả hai có bằng giá trị target hay không.

Cụ thể, ta dùng một số nguyên $i$ để biểu diễn trạng thái của cách chia tập con, trong đó các bit nhị phân của $i$ cho biết mỗi phần tử thuộc tập con thứ nhất hay không. Với mỗi giá trị $i$ có thể có, ta tính tích của hai tập con và kiểm tra xem cả hai có bằng target hay không.

Độ phức tạp thời gian là $O(2^n \times n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkEqualPartitions(self, nums: List[int], target: int) -> bool:
        n = len(nums)
        for i in range(1 << n):
            x = y = 1
            for j in range(n):
                if i >> j & 1:
                    x *= nums[j]
                else:
                    y *= nums[j]
            if x == target and y == target:
                return True
        return False
```

#### Java

```java
class Solution {
    public boolean checkEqualPartitions(int[] nums, long target) {
        int n = nums.length;
        for (int i = 0; i < 1 << n; ++i) {
            long x = 1, y = 1;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    x *= nums[j];
                } else {
                    y *= nums[j];
                }
            }
            if (x == target && y == target) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkEqualPartitions(vector<int>& nums, long long target) {
        int n = nums.size();
        for (int i = 0; i < 1 << n; ++i) {
            long long x = 1, y = 1;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    x *= nums[j];
                } else {
                    y *= nums[j];
                }
                if (x > target || y > target) {
                    break;
                }
            }
            if (x == target && y == target) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func checkEqualPartitions(nums []int, target int64) bool {
    n := len(nums)
    for i := 0; i < 1<<n; i++ {
        x, y := int64(1), int64(1)
        for j, v := range nums {
            if i>>j&1 == 1 {
                x *= int64(v)
            } else {
                y *= int64(v)
            }
            if x > target || y > target {
                break
            }
        }
        if x == target && y == target {
            return true
        }
    }
    return false
}
```

#### TypeScript

```ts
function checkEqualPartitions(nums: number[], target: number): boolean {
    const n = nums.length;
    for (let i = 0; i < 1 << n; ++i) {
        let [x, y] = [1, 1];
        for (let j = 0; j < n; ++j) {
            if (((i >> j) & 1) === 1) {
                x *= nums[j];
            } else {
                y *= nums[j];
            }
            if (x > target || y > target) {
                break;
            }
        }
        if (x === target && y === target) {
            return true;
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
