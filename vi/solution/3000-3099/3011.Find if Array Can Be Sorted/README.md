---
comments: true
difficulty: Medium
rating: 1496
source: Biweekly Contest 122 Q2
tags:
    - Bit Manipulation
    - Array
    - Sorting
---

<!-- problem:start -->

# [3011. Find if Array Can Be Sorted](https://leetcode.com/problems/find-if-array-can-be-sorted)

[中文文档](/solution/3000-3099/3011.Find%20if%20Array%20Can%20Be%20Sorted/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>dương</strong> <code>nums</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Trong một <strong>phép toán</strong>, bạn có thể hoán đổi hai phần tử <strong>kề nhau</strong> bất kỳ nếu chúng có <strong>cùng</strong> số <span data-keyword="set-bit">bit 1</span>. Bạn có thể thực hiện phép toán này <strong>bất kỳ</strong> số lần nào (<strong>kể cả không lần nào</strong>).</p>

<p>Trả về <code>true</code> <em>nếu có thể sắp xếp mảng theo thứ tự tăng dần, nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [8,4,2,30,15]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hãy xét biểu diễn nhị phân của từng phần tử. Các số 2, 4 và 8 đều có một bit 1 trong biểu diễn nhị phân lần lượt là &quot;10&quot;, &quot;100&quot; và &quot;1000&quot;. Các số 15 và 30 đều có bốn bit 1 trong biểu diễn nhị phân lần lượt là &quot;1111&quot; và &quot;11110&quot;.
Ta có thể sắp xếp mảng bằng 4 phép toán:
- Hoán đổi nums[0] với nums[1]. Phép toán này hợp lệ vì 8 và 4 đều có một bit 1. Mảng trở thành [4,8,2,30,15].
- Hoán đổi nums[1] với nums[2]. Phép toán này hợp lệ vì 8 và 2 đều có một bit 1. Mảng trở thành [4,2,8,30,15].
- Hoán đổi nums[0] với nums[1]. Phép toán này hợp lệ vì 4 và 2 đều có một bit 1. Mảng trở thành [2,4,8,30,15].
- Hoán đổi nums[3] với nums[4]. Phép toán này hợp lệ vì 30 và 15 đều có bốn bit 1. Mảng trở thành [2,4,8,15,30].
Mảng đã được sắp xếp, vì vậy ta trả về true.
Lưu ý rằng có thể có những chuỗi phép toán khác cũng sắp xếp được mảng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Mảng đã được sắp xếp, vì vậy ta trả về true.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,16,8,4,2]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Có thể chứng minh rằng không thể sắp xếp mảng đầu vào bằng bất kỳ số phép toán nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 2<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 100$. Hai giá trị có thể được hoán đổi khi và chỉ khi chúng có cùng số bit 1, vì vậy mỗi đoạn liên tiếp có cùng số bit 1 có thể được sắp xếp lại tùy ý.
>
> Mảng có thể sắp xếp được khi các đoạn sau khi sắp xếp được ghép lại, tức là giá trị nhỏ nhất của đoạn hiện tại không nhỏ hơn giá trị lớn nhất của đoạn trước đó.
>
> Hai con trỏ chia mảng theo số bit 1, theo dõi giá trị lớn nhất và nhỏ nhất của mỗi đoạn, rồi so sánh với giá trị lớn nhất trước đó.

<!-- thinking:end -->

Ta có thể dùng hai con trỏ để chia mảng $\textit{nums}$ thành một số mảng con, trong đó mỗi mảng con chứa các phần tử có cùng số bit $1$ trong biểu diễn nhị phân. Với mỗi mảng con, ta chỉ cần quan tâm đến giá trị lớn nhất và nhỏ nhất của nó. Nếu giá trị nhỏ nhất nhỏ hơn giá trị lớn nhất của mảng con trước đó, thì không thể sắp xếp mảng bằng cách hoán đổi.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canSortArray(self, nums: List[int]) -> bool:
        pre_mx = 0
        i, n = 0, len(nums)
        while i < n:
            cnt = nums[i].bit_count()
            j = i + 1
            mi = mx = nums[i]
            while j < n and nums[j].bit_count() == cnt:
                mi = min(mi, nums[j])
                mx = max(mx, nums[j])
                j += 1
            if pre_mx > mi:
                return False
            pre_mx = mx
            i = j
        return True
```

#### Java

```java
class Solution {
    public boolean canSortArray(int[] nums) {
        int preMx = 0;
        int i = 0, n = nums.length;
        while (i < n) {
            int cnt = Integer.bitCount(nums[i]);
            int j = i + 1;
            int mi = nums[i], mx = nums[i];
            while (j < n && Integer.bitCount(nums[j]) == cnt) {
                mi = Math.min(mi, nums[j]);
                mx = Math.max(mx, nums[j]);
                j++;
            }
            if (preMx > mi) {
                return false;
            }
            preMx = mx;
            i = j;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canSortArray(vector<int>& nums) {
        int preMx = 0;
        int i = 0, n = nums.size();
        while (i < n) {
            int cnt = __builtin_popcount(nums[i]);
            int j = i + 1;
            int mi = nums[i], mx = nums[i];
            while (j < n && __builtin_popcount(nums[j]) == cnt) {
                mi = min(mi, nums[j]);
                mx = max(mx, nums[j]);
                j++;
            }
            if (preMx > mi) {
                return false;
            }
            preMx = mx;
            i = j;
        }
        return true;
    }
};
```

#### Go

```go
func canSortArray(nums []int) bool {
	preMx := 0
	i, n := 0, len(nums)
	for i < n {
		cnt := bits.OnesCount(uint(nums[i]))
		j := i + 1
		mi, mx := nums[i], nums[i]
		for j < n && bits.OnesCount(uint(nums[j])) == cnt {
			mi = min(mi, nums[j])
			mx = max(mx, nums[j])
			j++
		}
		if preMx > mi {
			return false
		}
		preMx = mx
		i = j
	}
	return true
}
```

#### TypeScript

```ts
function canSortArray(nums: number[]): boolean {
    let preMx = 0;
    const n = nums.length;
    for (let i = 0; i < n;) {
        const cnt = bitCount(nums[i]);
        let j = i + 1;
        let [mi, mx] = [nums[i], nums[i]];
        while (j < n && bitCount(nums[j]) === cnt) {
            mi = Math.min(mi, nums[j]);
            mx = Math.max(mx, nums[j]);
            j++;
        }
        if (preMx > mi) {
            return false;
        }
        preMx = mx;
        i = j;
    }
    return true;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
