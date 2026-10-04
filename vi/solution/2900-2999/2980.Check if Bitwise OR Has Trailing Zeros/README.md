---
comments: true
difficulty: Easy
rating: 1233
source: Weekly Contest 378 Q1
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [2980. Check if Bitwise OR Has Trailing Zeros](https://leetcode.com/problems/check-if-bitwise-or-has-trailing-zeros)

[Tài liệu tiếng Trung](/solution/2900-2999/2980.Check%20if%20Bitwise%20OR%20Has%20Trailing%20Zeros/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng các số nguyên <strong>dương</strong> <code>nums</code>.</p>

<p>Hãy kiểm tra xem có thể chọn <strong>hai hoặc nhiều hơn</strong> phần tử trong mảng sao cho phép <code>OR</code> bitwise của các phần tử được chọn có <strong>ít nhất </strong>một số 0 ở cuối trong biểu diễn nhị phân hay không.</p>

<p>Ví dụ, biểu diễn nhị phân của <code>5</code> là <code>&quot;101&quot;</code>, không có số 0 nào ở cuối, trong khi biểu diễn nhị phân của <code>4</code> là <code>&quot;100&quot;</code>, có hai số 0 ở cuối.</p>

<p>Trả về <code>true</code> <em>nếu có thể chọn hai hoặc nhiều phần tử sao cho phép</em> <code>OR</code> <em>bitwise của chúng có các số 0 ở cuối, trả về</em> <code>false</code> <em>nếu không</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Nếu chọn các phần tử 2 và 4, phép OR bitwise của chúng là 6, có biểu diễn nhị phân là &quot;110&quot; với một số 0 ở cuối.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,8,16]
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Nếu chọn các phần tử 2 và 4, phép OR bitwise của chúng là 6, có biểu diễn nhị phân là &quot;110&quot; với một số 0 ở cuối.
Các cách chọn phần tử khác để biểu diễn nhị phân của phép OR bitwise có các số 0 ở cuối là: (2, 8), (2, 16), (4, 8), (4, 16), (8, 16), (2, 4, 8), (2, 4, 16), (2, 8, 16), (4, 8, 16) và (2, 4, 8, 16).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,5,7,9]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có cách nào để chọn hai hoặc nhiều phần tử sao cho biểu diễn nhị phân của phép OR bitwise của chúng có các số 0 ở cuối.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm số chẵn

<!-- thinking:start -->

> **Tư duy**
>
> Phép OR bitwise có số 0 ở cuối khi và chỉ khi mọi số được chọn đều là số chẵn. Vì phải chọn ít nhất hai số nên chỉ cần có hai số chẵn. $n \le 100$; hãy đếm số lượng đó.

<!-- thinking:end -->

Theo đề bài, nếu có từ hai phần tử trở lên trong mảng mà phép OR bitwise của chúng có các số 0 ở cuối, thì trong mảng phải có ít nhất hai số chẵn. Vì vậy, ta có thể đếm số lượng số chẵn trong mảng. Nếu số lượng số chẵn lớn hơn hoặc bằng $2$, trả về `true`, ngược lại trả về `false`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasTrailingZeros(self, nums: List[int]) -> bool:
        return sum(x & 1 ^ 1 for x in nums) >= 2
```

#### Java

```java
class Solution {
    public boolean hasTrailingZeros(int[] nums) {
        int cnt = 0;
        for (int x : nums) {
            cnt += (x & 1 ^ 1);
        }
        return cnt >= 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool hasTrailingZeros(vector<int>& nums) {
        int cnt = 0;
        for (int x : nums) {
            cnt += (x & 1 ^ 1);
        }
        return cnt >= 2;
    }
};
```

#### Go

```go
func hasTrailingZeros(nums []int) bool {
	cnt := 0
	for _, x := range nums {
		cnt += (x&1 ^ 1)
	}
	return cnt >= 2
}
```

#### TypeScript

```ts
function hasTrailingZeros(nums: number[]): boolean {
    let cnt = 0;
    for (const x of nums) {
        cnt += (x & 1) ^ 1;
    }
    return cnt >= 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
