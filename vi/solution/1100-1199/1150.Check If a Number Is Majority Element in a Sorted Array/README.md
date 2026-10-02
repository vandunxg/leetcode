---
comments: true
difficulty: Easy
rating: 1249
source: Biweekly Contest 6 Q1
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1150. Check If a Number Is Majority Element in a Sorted Array 🔒](https://leetcode.com/problems/check-if-a-number-is-majority-element-in-a-sorted-array)

[中文文档](/solution/1100-1199/1150.Check%20If%20a%20Number%20Is%20Majority%20Element%20in%20a%20Sorted%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> được sắp xếp theo thứ tự không giảm và số nguyên <code>target</code>. Trả về <code>true</code> <em>nếu</em> <code>target</code> <em>là phần tử <strong>chiếm đa số</strong>, nếu không thì trả về </em><code>false</code>.</p>

<p>Phần tử <strong>chiếm đa số</strong> trong mảng <code>nums</code> là phần tử xuất hiện nhiều hơn <code>nums.length / 2</code> lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,5,5,5,5,5,6,6], target = 5
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Giá trị 5 xuất hiện 5 lần và độ dài mảng là 9.
Do đó, 5 là phần tử chiếm đa số vì 5 &gt; 9/2 là đúng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,100,101,101], target = 101
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Giá trị 101 xuất hiện 2 lần và độ dài mảng là 4.
Do đó, 101 không phải phần tử chiếm đa số vì 2 &gt; 4/2 là sai.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i], target &lt;= 10<sup>9</sup></code></li>
	<li><code>nums</code> được sắp xếp theo thứ tự không giảm.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Trong mảng đã sắp xếp, số lần xuất hiện của $target$ bằng khoảng cách giữa cận trái và cận phải của nó. Hai lần tìm kiếm nhị phân tìm vị trí đầu tiên có giá trị $\ge target$ và vị trí đầu tiên có giá trị $>target$; khoảng cách lớn hơn $n/2$ khi và chỉ khi $target$ chiếm đa số. Với $n$ lớn, không cần đếm tuyến tính.

<!-- thinking:end -->

Ta nhận thấy các phần tử trong mảng $nums$ được sắp xếp không giảm. Vì vậy, có thể dùng tìm kiếm nhị phân để tìm chỉ số $left$ của phần tử đầu tiên trong $nums$ lớn hơn hoặc bằng $target$, và chỉ số $right$ của phần tử đầu tiên lớn hơn $target$. Nếu $right - left > \frac{n}{2}$, số lần xuất hiện của $target$ trong $nums$ vượt quá một nửa độ dài mảng, nên trả về $true$; ngược lại trả về $false$.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isMajorityElement(self, nums: List[int], target: int) -> bool:
        left = bisect_left(nums, target)
        right = bisect_right(nums, target)
        return right - left > len(nums) // 2
```

#### Java

```java
class Solution {
    public boolean isMajorityElement(int[] nums, int target) {
        int left = search(nums, target);
        int right = search(nums, target + 1);
        return right - left > nums.length / 2;
    }

    private int search(int[] nums, int x) {
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (nums[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isMajorityElement(vector<int>& nums, int target) {
        auto left = lower_bound(nums.begin(), nums.end(), target);
        auto right = upper_bound(nums.begin(), nums.end(), target);
        return right - left > nums.size() / 2;
    }
};
```

#### Go

```go
func isMajorityElement(nums []int, target int) bool {
	left := sort.SearchInts(nums, target)
	right := sort.SearchInts(nums, target+1)
	return right-left > len(nums)/2
}
```

#### TypeScript

```ts
function isMajorityElement(nums: number[], target: number): boolean {
    const search = (x: number) => {
        let left = 0;
        let right = nums.length;
        while (left < right) {
            const mid = (left + right) >> 1;
            if (nums[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    };
    const left = search(target);
    const right = search(target + 1);
    return right - left > nums.length >> 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tìm kiếm nhị phân (Tối ưu)

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 thực hiện hai lần tìm kiếm nhị phân. Nếu $target$ xuất hiện quá nửa số phần tử, vị trí $left+\lfloor n/2\rfloor$ vẫn chứa $target$. Dùng một lần `bisect_left` rồi kiểm tra vị trí đó sẽ loại bỏ được lần tìm kiếm thứ hai.

<!-- thinking:end -->

Ở lời giải 1, ta dùng tìm kiếm nhị phân hai lần để tìm chỉ số $left$ của phần tử đầu tiên trong mảng $nums$ lớn hơn hoặc bằng $target$, và chỉ số $right$ của phần tử đầu tiên lớn hơn $target$. Tuy nhiên, ta chỉ cần tìm kiếm nhị phân một lần để tìm $left$, rồi kiểm tra xem $nums[left + \frac{n}{2}]$ có bằng $target$ hay không. Nếu bằng, $target$ xuất hiện nhiều hơn một nửa độ dài mảng, nên trả về $true$; nếu không, trả về $false$.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isMajorityElement(self, nums: List[int], target: int) -> bool:
        left = bisect_left(nums, target)
        right = left + len(nums) // 2
        return right < len(nums) and nums[right] == target
```

#### Java

```java
class Solution {
    public boolean isMajorityElement(int[] nums, int target) {
        int n = nums.length;
        int left = search(nums, target);
        int right = left + n / 2;
        return right < n && nums[right] == target;
    }

    private int search(int[] nums, int x) {
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (nums[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isMajorityElement(vector<int>& nums, int target) {
        int n = nums.size();
        int left = lower_bound(nums.begin(), nums.end(), target) - nums.begin();
        int right = left + n / 2;
        return right < n && nums[right] == target;
    }
};
```

#### Go

```go
func isMajorityElement(nums []int, target int) bool {
	n := len(nums)
	left := sort.SearchInts(nums, target)
	right := left + n/2
	return right < n && nums[right] == target
}
```

#### TypeScript

```ts
function isMajorityElement(nums: number[], target: number): boolean {
    const search = (x: number) => {
        let left = 0;
        let right = n;
        while (left < right) {
            const mid = (left + right) >> 1;
            if (nums[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    };
    const n = nums.length;
    const left = search(target);
    const right = left + (n >> 1);
    return right < n && nums[right] === target;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
