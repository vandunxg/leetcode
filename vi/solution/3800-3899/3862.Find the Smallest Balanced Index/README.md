---
comments: true
difficulty: Medium
rating: 1697
source: Weekly Contest 492 Q2
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3862. Find the Smallest Balanced Index](https://leetcode.com/problems/find-the-smallest-balanced-index)

[中文文档](/solution/3800-3899/3862.Find%20the%20Smallest%20Balanced%20Index/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Một chỉ số <code>i</code> là <strong>balanced</strong> nếu tổng các phần tử nằm <strong>nghiêm ngặt</strong> bên trái <code>i</code> bằng tích các phần tử nằm <strong>nghiêm ngặt</strong> bên phải <code>i</code>.</p>

<p>Nếu không có phần tử nào bên trái, tổng được xem là 0. Tương tự, nếu không có phần tử nào bên phải, tích được xem là 1.</p>

<p>Hãy trả về một số nguyên biểu thị chỉ số balanced <strong>nhỏ nhất</strong>. Nếu không tồn tại chỉ số balanced nào, hãy trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với chỉ số <code>i = 1</code>:</p>

<ul>
	<li>Tổng bên trái = <code>nums[0] = 2</code></li>
	<li>Tích bên phải = <code>nums[2] = 2</code></li>
	<li>Vì tổng bên trái bằng tích bên phải, chỉ số 1 là balanced.</li>
</ul>

<p>Không có chỉ số nhỏ hơn nào thỏa mãn điều kiện, nên đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,8,2,2,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với chỉ số <code>i = 2</code>:</p>

<ul>
	<li>Tổng bên trái = <code>2 + 8 = 10</code></li>
	<li>Tích bên phải = <code>2 * 5 = 10</code></li>
	<li>Vì tổng bên trái bằng tích bên phải, chỉ số 2 là balanced.</li>
</ul>

<p>Không có chỉ số nhỏ hơn nào thỏa mãn điều kiện, nên đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>
Với chỉ số <code>i = 0</code>:

<ul>
	<li>Phía bên trái rỗng, nên tổng bên trái là 0.</li>
	<li>Phía bên phải rỗng, nên tích bên phải là 1.</li>
	<li>Vì tổng bên trái không bằng tích bên phải, chỉ số 0 không balanced.</li>
</ul>
Do đó, không tồn tại chỉ số balanced nào và đáp án là -1.</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Một chỉ số balanced có tổng bên trái bằng tích bên phải. Vì $n \le 10^5$ và các phần tử đều dương, tích bên phải tăng khi chỉ số di chuyển về bên trái.
>
> Khi di chuyển sang trái, tổng bên trái giảm còn tích bên phải tăng, nên đẳng thức nhiều nhất chỉ có thể đúng một lần. Nếu tồn tại, chỉ số đó là duy nhất.
>
> Duyệt từ phải sang trái với tổng bên trái còn lại và tích bên phải; trả về ngay khi hai giá trị bằng nhau. Khi tích đã lớn hơn hoặc bằng tổng còn lại, các chỉ số tiếp theo không thể thỏa mãn.
>
> Một lượt duyệt từ phải sang trái là đủ để quyết định đáp án.

<!-- thinking:end -->

Trước tiên, ta tính tổng $s$ của tất cả phần tử trong mảng. Sau đó, ta duyệt từng chỉ số $i$ từ phải sang trái, đồng thời duy trì biến $p$ để ghi nhận tích của tất cả phần tử nằm bên phải chỉ số $i$. Khi đến chỉ số $i$, trước tiên ta trừ $nums[i]$ khỏi $s$, sau đó kiểm tra xem $s$ có bằng $p$ hay không; nếu bằng, ta trả về chỉ số $i$. Tiếp theo, ta nhân $p$ với $nums[i]$. Nếu $p$ lớn hơn hoặc bằng $s$, tích sẽ tiếp tục tăng và không thể tìm thấy chỉ số balanced nào về sau, nên ta có thể kết thúc việc duyệt sớm.

Nếu không tìm thấy chỉ số balanced nào sau khi duyệt, ta trả về -1.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestBalancedIndex(self, nums: list[int]) -> int:
        s = sum(nums)
        p = 1
        for i in range(len(nums) - 1, -1, -1):
            s -= nums[i]
            if s == p:
                return i
            p *= nums[i]
            if p >= s:
                break
        return -1
```

#### Java

```java
class Solution {
    public int smallestBalancedIndex(int[] nums) {
        long s = 0, p = 1;
        for (int x : nums) {
            s += x;
        }
        for (int i = nums.length - 1; i >= 0; --i) {
            s -= nums[i];
            if (s == p) {
                return i;
            }
            p *= nums[i];
            if (p >= s) {
                break;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestBalancedIndex(vector<int>& nums) {
        long long s = 0, p = 1;
        for (int x : nums) {
            s += x;
        }
        for (int i = nums.size() - 1; i >= 0; --i) {
            s -= nums[i];
            if (s == p) {
                return i;
            }
            p *= nums[i];
            if (p >= s) {
                break;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func smallestBalancedIndex(nums []int) int {
	s, p := 0, 1
	for _, x := range nums {
		s += x
	}
	for i := len(nums) - 1; i >= 0; i-- {
		s -= nums[i]
		if s == p {
			return i
		}
		p *= nums[i]
		if p >= s {
			break
		}
	}
	return -1
}
```

#### TypeScript

```ts
function smallestBalancedIndex(nums: number[]): number {
    let s = 0;
    for (const x of nums) {
        s += x;
    }
    for (let i = nums.length - 1, p = 1; i >= 0; --i) {
        s -= nums[i];
        if (s === p) {
            return i;
        }
        p *= nums[i];
        if (p >= s) {
            break;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
