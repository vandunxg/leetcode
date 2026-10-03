---
comments: true
difficulty: Easy
rating: 1252
source: Weekly Contest 279 Q1
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [2164. Sort Even and Odd Indices Independently](https://leetcode.com/problems/sort-even-and-odd-indices-independently)

[中文文档](/solution/2100-2199/2164.Sort%20Even%20and%20Odd%20Indices%20Independently/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> <strong>được đánh chỉ số từ 0</strong>. Hãy sắp xếp lại các giá trị của <code>nums</code> theo các quy tắc sau:</p>

<ol>
	<li>Sắp xếp các giá trị tại <strong>chỉ số lẻ</strong> của <code>nums</code> theo <strong>thứ tự không tăng</strong>.

    <ul>
    <li>Ví dụ, nếu <code>nums = [4,<strong><u>1</u></strong>,2,<u><strong>3</strong></u>]</code> trước bước này, thì sau đó mảng trở thành <code>[4,<u><strong>3</strong></u>,2,<strong><u>1</u></strong>]</code>. Các giá trị tại chỉ số lẻ <code>1</code> và <code>3</code> được sắp xếp theo thứ tự không tăng.</li>
    </ul>
    </li>
    <li>Sắp xếp các giá trị tại <strong>chỉ số chẵn</strong> của <code>nums</code> theo <strong>thứ tự không giảm</strong>.
    <ul>
    <li>Ví dụ, nếu <code>nums = [<u><strong>4</strong></u>,1,<u><strong>2</strong></u>,3]</code> trước bước này, thì sau đó mảng trở thành <code>[<u><strong>2</strong></u>,1,<u><strong>4</strong></u>,3]</code>. Các giá trị tại chỉ số chẵn <code>0</code> và <code>2</code> được sắp xếp theo thứ tự không giảm.</li>
    </ul>
    </li>

</ol>

<p>Trả về <em>mảng được tạo thành sau khi sắp xếp lại các giá trị của</em> <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,1,2,3]
<strong>Đầu ra:</strong> [2,3,4,1]
<strong>Giải thích:</strong>
Đầu tiên, ta sắp xếp các giá trị tại chỉ số lẻ (1 và 3) theo thứ tự không tăng.
Vì vậy, nums thay đổi từ [4,<strong><u>1</u></strong>,2,<strong><u>3</u></strong>] thành [4,<u><strong>3</strong></u>,2,<strong><u>1</u></strong>].
Tiếp theo, ta sắp xếp các giá trị tại chỉ số chẵn (0 và 2) theo thứ tự không giảm.
Vì vậy, nums thay đổi từ [<u><strong>4</strong></u>,1,<strong><u>2</u></strong>,3] thành [<u><strong>2</strong></u>,3,<u><strong>4</strong></u>,1].
Do đó, mảng được tạo thành sau khi sắp xếp lại là [2,3,4,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1]
<strong>Đầu ra:</strong> [2,1]
<strong>Giải thích:</strong>
Vì chỉ có đúng một chỉ số lẻ và một chỉ số chẵn nên không có sự sắp xếp lại nào diễn ra.
Mảng kết quả là [2,1], giống với mảng ban đầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các chỉ số chẵn được sắp xếp tăng dần còn các chỉ số lẻ được sắp xếp giảm dần; hai nhóm không ảnh hưởng lẫn nhau. Trích xuất, sắp xếp rồi ghi lại.
>
> Gán $\textit{nums}[::2]$ đã sắp xếp và $\textit{nums}[1::2]$ đã sắp xếp ngược.
>
> Thời gian chạy chủ yếu nằm ở bước sắp xếp.

<!-- thinking:end -->

Ta có thể tách riêng các phần tử tại chỉ số lẻ và chẵn, sau đó sắp xếp mảng các chỉ số lẻ theo thứ tự không tăng và mảng các chỉ số chẵn theo thứ tự không giảm. Cuối cùng, ta gộp hai mảng trở lại.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortEvenOdd(self, nums: List[int]) -> List[int]:
        a = sorted(nums[::2])
        b = sorted(nums[1::2], reverse=True)
        nums[::2] = a
        nums[1::2] = b
        return nums
```

#### Java

```java
class Solution {
    public int[] sortEvenOdd(int[] nums) {
        int n = nums.length;
        int[] a = new int[(n + 1) >> 1];
        int[] b = new int[n >> 1];
        for (int i = 0, j = 0; j < n >> 1; i += 2, ++j) {
            a[j] = nums[i];
            b[j] = nums[i + 1];
        }
        if (n % 2 == 1) {
            a[a.length - 1] = nums[n - 1];
        }
        Arrays.sort(a);
        Arrays.sort(b);
        int[] ans = new int[n];
        for (int i = 0, j = 0; j < a.length; i += 2, ++j) {
            ans[i] = a[j];
        }
        for (int i = 1, j = b.length - 1; j >= 0; i += 2, --j) {
            ans[i] = b[j];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sortEvenOdd(vector<int>& nums) {
        int n = nums.size();
        vector<int> a;
        vector<int> b;
        for (int i = 0; i < n; ++i) {
            if (i % 2 == 0) {
                a.push_back(nums[i]);
            } else {
                b.push_back(nums[i]);
            }
        }
        sort(a.begin(), a.end());
        sort(b.rbegin(), b.rend());
        vector<int> ans(n);
        for (int i = 0, j = 0; j < a.size(); i += 2, ++j) {
            ans[i] = a[j];
        }
        for (int i = 1, j = 0; j < b.size(); i += 2, ++j) {
            ans[i] = b[j];
        }
        return ans;
    }
};
```

#### Go

```go
func sortEvenOdd(nums []int) []int {
	n := len(nums)
	var a []int
	var b []int
	for i, v := range nums {
		if i%2 == 0 {
			a = append(a, v)
		} else {
			b = append(b, v)
		}
	}
	ans := make([]int, n)
	sort.Ints(a)
	sort.Slice(b, func(i, j int) bool {
		return b[i] > b[j]
	})
	for i, j := 0, 0; j < len(a); i, j = i+2, j+1 {
		ans[i] = a[j]
	}
	for i, j := 1, 0; j < len(b); i, j = i+2, j+1 {
		ans[i] = b[j]
	}
	return ans
}
```

#### TypeScript

```ts
function sortEvenOdd(nums: number[]): number[] {
    const n = nums.length;
    const a: number[] = [];
    const b: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (i % 2 === 0) {
            a.push(nums[i]);
        } else {
            b.push(nums[i]);
        }
    }
    a.sort((x, y) => x - y);
    b.sort((x, y) => y - x);
    const ans: number[] = [];
    for (let i = 0, j = 0; j < a.length; i += 2, ++j) {
        ans[i] = a[j];
    }
    for (let i = 1, j = 0; j < b.length; i += 2, ++j) {
        ans[i] = b[j];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
