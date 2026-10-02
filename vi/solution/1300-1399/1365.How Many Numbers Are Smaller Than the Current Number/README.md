---
comments: true
difficulty: Easy
rating: 1152
source: Weekly Contest 178 Q1
tags:
    - Array
    - Hash Table
    - Counting Sort
    - Sorting
---

<!-- problem:start -->

# [1365. How Many Numbers Are Smaller Than the Current Number](https://leetcode.com/problems/how-many-numbers-are-smaller-than-the-current-number)

[中文文档](/solution/1300-1399/1365.How%20Many%20Numbers%20Are%20Smaller%20Than%20the%20Current%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code>. Với mỗi <code>nums[i]</code>, hãy tìm có bao nhiêu số trong mảng nhỏ hơn nó. Nói cách khác, với mỗi <code>nums[i]</code>, hãy đếm số chỉ số <code>j</code>&nbsp;thỏa mãn&nbsp;<code>j != i</code> <strong>và</strong> <code>nums[j] &lt; nums[i]</code>.</p>

<p>Trả về kết quả dưới dạng một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [8,1,2,2,3]
<strong>Đầu ra:</strong> [4,0,1,1,3]
<strong>Giải thích:</strong> 
Với nums[0]=8, có bốn số nhỏ hơn nó (1, 2, 2 và 3). 
Với nums[1]=1, không có số nào nhỏ hơn nó.
Với nums[2]=2, có một số nhỏ hơn nó (1). 
Với nums[3]=2, có một số nhỏ hơn nó (1). 
Với nums[4]=3, có ba số nhỏ hơn nó (1, 2 và 2).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,5,4,8]
<strong>Đầu ra:</strong> [2,1,0,3]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,7,7,7]
<strong>Đầu ra:</strong> [0,0,0,0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 500</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số giá trị nhỏ hơn nghiêm ngặt từng $x$. Với $n \le 500$, có thể dùng hai vòng lặp; nhưng sau khi sắp xếp, số lượng đó chính là chỉ số chèn của $x$. Sao chép và sắp xếp mảng, rồi dùng $\mathrm{bisect\_left}$ cho mỗi $x$ để tìm số phần tử nhỏ hơn.

<!-- thinking:end -->

Ta sao chép mảng $nums$ thành $arr$, sau đó sắp xếp $arr$ theo thứ tự tăng dần.

Tiếp theo, với mỗi phần tử $x$ trong $nums$, ta dùng tìm kiếm nhị phân để tìm chỉ số $j$ của phần tử đầu tiên lớn hơn hoặc bằng $x$. Khi đó, $j$ cũng chính là số phần tử nhỏ hơn $x$. Ta lưu $j$ vào mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallerNumbersThanCurrent(self, nums: List[int]) -> List[int]:
        arr = sorted(nums)
        return [bisect_left(arr, x) for x in nums]
```

#### Java

```java
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums) {
        int[] arr = nums.clone();
        Arrays.sort(arr);
        for (int i = 0; i < nums.length; ++i) {
            nums[i] = search(arr, nums[i]);
        }
        return nums;
    }

    private int search(int[] nums, int x) {
        int l = 0, r = nums.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        vector<int> arr = nums;
        sort(arr.begin(), arr.end());
        for (int i = 0; i < nums.size(); ++i) {
            nums[i] = lower_bound(arr.begin(), arr.end(), nums[i]) - arr.begin();
        }
        return nums;
    }
};
```

#### Go

```go
func smallerNumbersThanCurrent(nums []int) (ans []int) {
	arr := make([]int, len(nums))
	copy(arr, nums)
	sort.Ints(arr)
	for i, x := range nums {
		nums[i] = sort.SearchInts(arr, x)
	}
	return nums
}
```

#### TypeScript

```ts
function smallerNumbersThanCurrent(nums: number[]): number[] {
    const search = (nums: number[], x: number) => {
        let l = 0,
            r = nums.length;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    const arr = nums.slice().sort((a, b) => a - b);
    for (let i = 0; i < nums.length; ++i) {
        nums[i] = search(arr, nums[i]);
    }
    return nums;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Counting sort + prefix sum

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị nằm trong $[0,100]$, nên không cần comparison sort. Lấy prefix sum của mảng tần suất sẽ cho $s[x]$ là số giá trị nhỏ hơn $x$; chỉ cần duyệt mảng ban đầu một lượt là có thể trả lời cho mọi phần tử.

<!-- thinking:end -->

Ta nhận thấy các phần tử trong mảng $nums$ nằm trong khoảng $[0, 100]$. Vì vậy, trước tiên ta dùng counting sort để đếm số lần xuất hiện của mỗi giá trị trong $nums$. Sau đó, tính prefix sum của mảng đếm. Cuối cùng, duyệt mảng $nums$; với mỗi phần tử $x$, lấy trực tiếp giá trị tại chỉ số $x$ trong mảng đếm đã tích lũy và thêm vào mảng kết quả.

Độ phức tạp thời gian là $O(n + M)$ và độ phức tạp không gian là $O(M)$, trong đó $n$ là độ dài và $M$ là giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallerNumbersThanCurrent(self, nums: List[int]) -> List[int]:
        cnt = [0] * 102
        for x in nums:
            cnt[x + 1] += 1
        s = list(accumulate(cnt))
        return [s[x] for x in nums]
```

#### Java

```java
class Solution {
    public int[] smallerNumbersThanCurrent(int[] nums) {
        int[] cnt = new int[102];
        for (int x : nums) {
            ++cnt[x + 1];
        }
        for (int i = 1; i < cnt.length; ++i) {
            cnt[i] += cnt[i - 1];
        }
        int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = cnt[nums[i]];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        int cnt[102]{};
        for (int& x : nums) {
            ++cnt[x + 1];
        }
        for (int i = 1; i < 102; ++i) {
            cnt[i] += cnt[i - 1];
        }
        vector<int> ans;
        for (int& x : nums) {
            ans.push_back(cnt[x]);
        }
        return ans;
    }
};
```

#### Go

```go
func smallerNumbersThanCurrent(nums []int) (ans []int) {
	cnt := [102]int{}
	for _, x := range nums {
		cnt[x+1]++
	}
	for i := 1; i < len(cnt); i++ {
		cnt[i] += cnt[i-1]
	}
	for _, x := range nums {
		ans = append(ans, cnt[x])
	}
	return
}
```

#### TypeScript

```ts
function smallerNumbersThanCurrent(nums: number[]): number[] {
    const cnt: number[] = new Array(102).fill(0);
    for (const x of nums) {
        ++cnt[x + 1];
    }
    for (let i = 1; i < cnt.length; ++i) {
        cnt[i] += cnt[i - 1];
    }
    const n = nums.length;
    const ans: number[] = new Array(n);
    for (let i = 0; i < n; ++i) {
        ans[i] = cnt[nums[i]];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
