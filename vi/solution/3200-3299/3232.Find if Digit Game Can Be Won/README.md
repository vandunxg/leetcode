---
comments: true
difficulty: Easy
rating: 1163
source: Weekly Contest 408 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3232. Find if Digit Game Can Be Won](https://leetcode.com/problems/find-if-digit-game-can-be-won)

[中文文档](/solution/3200-3299/3232.Find%20if%20Digit%20Game%20Can%20Be%20Won/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng các số nguyên <strong>dương</strong> <code>nums</code>.</p>

<p>Alice và Bob đang chơi một trò chơi. Trong trò chơi, Alice có thể chọn <strong>hoặc</strong> toàn bộ các số có một chữ số hoặc toàn bộ các số có hai chữ số từ <code>nums</code>, còn các số khác sẽ được đưa cho Bob. Alice thắng nếu tổng các số của cô ấy <strong>lớn hơn hẳn</strong> tổng các số của Bob.</p>

<p>Trả về <code>true</code> nếu Alice có thể thắng trò chơi này, nếu không thì trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Alice không thể thắng khi chọn các số có một chữ số hoặc các số có hai chữ số.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5,14]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Alice có thể thắng bằng cách chọn các số có một chữ số, với tổng bằng 15.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,5,5,25]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Alice có thể thắng bằng cách chọn các số có hai chữ số, với tổng bằng 25.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 99</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tính tổng

<!-- thinking:start -->

> **Tư duy**
>
> Alice chọn toàn bộ các số có một chữ số hoặc toàn bộ các số có hai chữ số, và thắng nếu tổng được chọn lớn hơn hẳn tổng còn lại. Với $n\le 100$, không cần tìm kiếm: hai lựa chọn này bổ sung cho nhau, nên Alice thắng khi và chỉ khi hai tổng khác nhau.
>
> Tính tổng các giá trị $<10$ và các giá trị $\ge 10$, rồi so sánh. Chỉ cần duyệt mảng một lần.

<!-- thinking:end -->

Theo mô tả bài toán, chỉ cần tổng các số có một chữ số khác tổng các số có hai chữ số thì Alice luôn có thể chọn nhóm có tổng lớn hơn để giành chiến thắng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canAliceWin(self, nums: List[int]) -> bool:
        a = sum(x for x in nums if x < 10)
        b = sum(x for x in nums if x > 9)
        return a != b
```

#### Java

```java
class Solution {
    public boolean canAliceWin(int[] nums) {
        int a = 0, b = 0;
        for (int x : nums) {
            if (x < 10) {
                a += x;
            } else {
                b += x;
            }
        }
        return a != b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canAliceWin(vector<int>& nums) {
        int a = 0, b = 0;
        for (int x : nums) {
            if (x < 10) {
                a += x;
            } else {
                b += x;
            }
        }
        return a != b;
    }
};
```

#### Go

```go
func canAliceWin(nums []int) bool {
	a, b := 0, 0
	for _, x := range nums {
		if x < 10 {
			a += x
		} else {
			b += x
		}
	}
	return a != b
}
```

#### TypeScript

```ts
function canAliceWin(nums: number[]): boolean {
    let [a, b] = [0, 0];
    for (const x of nums) {
        if (x < 10) {
            a += x;
        } else {
            b += x;
        }
    }
    return a !== b;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
