---
comments: true
difficulty: Medium
rating: 1284
source: Weekly Contest 193 Q2
tags:
    - Greedy
    - Array
    - Hash Table
    - Counting
    - Sorting
---

<!-- problem:start -->

# [1481. Least Number of Unique Integers after K Removals](https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals)

[中文文档](/solution/1400-1499/1481.Least%20Number%20of%20Unique%20Integers%20after%20K%20Removals/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên&nbsp;<code>arr</code>&nbsp;và một số nguyên <code>k</code>.&nbsp;Hãy tìm <em>số lượng số nguyên phân biệt ít nhất</em>&nbsp;sau khi xóa <strong>chính xác</strong> <code>k</code> phần tử<b>.</b></p>

<ol>
</ol>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input: </strong>arr = [5,5,4], k = 1
<strong>Output: </strong>1
<strong>Explanation</strong>: Xóa số 4 duy nhất, chỉ còn lại 5.
</pre>

<strong class="example">Ví dụ 2:</strong>

<pre>
<strong>Input: </strong>arr = [4,3,1,1,3,3,2], k = 3
<strong>Output: </strong>2
<strong>Explanation</strong>: Xóa 4, 2 và một trong hai số 1 hoặc ba số 3. 1 và 3 sẽ còn lại.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length&nbsp;&lt;= 10^5</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10^9</code></li>
	<li><code>0 &lt;= k&nbsp;&lt;= arr.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Sau $k$ lần xóa, ta muốn số lượng giá trị phân biệt ít nhất, vì vậy hãy xóa các giá trị xuất hiện ít nhất trước. Sắp xếp tần suất xuất hiện rồi dùng $k$ để xóa chúng; khi $k$ không còn đủ, số lượng giá trị phân biệt còn lại chính là đáp án.

<!-- thinking:end -->

Ta sử dụng hash table $cnt$ để đếm số lần xuất hiện của mỗi số nguyên trong mảng $arr$, sau đó sắp xếp các giá trị trong $cnt$ theo thứ tự tăng dần và lưu chúng vào mảng $nums$.

Tiếp theo, ta duyệt mảng $nums$. Với giá trị hiện tại là $nums[i]$, ta trừ $nums[i]$ khỏi $k$. Nếu $k \lt 0$, điều đó có nghĩa là ta đã xóa quá $k$ phần tử, và số lượng số nguyên phân biệt ít nhất trong mảng là độ dài của $nums$ trừ đi chỉ số $i$ đang duyệt. Khi đó trả về kết quả ngay.

Nếu duyệt đến cuối mảng, điều đó có nghĩa là ta đã xóa tất cả các phần tử, và số lượng số nguyên phân biệt ít nhất trong mảng là $0$.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLeastNumOfUniqueInts(self, arr: List[int], k: int) -> int:
        cnt = Counter(arr)
        for i, v in enumerate(sorted(cnt.values())):
            k -= v
            if k < 0:
                return len(cnt) - i
        return 0
```

#### Java

```java
class Solution {
    public int findLeastNumOfUniqueInts(int[] arr, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : arr) {
            cnt.merge(x, 1, Integer::sum);
        }
        List<Integer> nums = new ArrayList<>(cnt.values());
        Collections.sort(nums);
        for (int i = 0, m = nums.size(); i < m; ++i) {
            k -= nums.get(i);
            if (k < 0) {
                return m - i;
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findLeastNumOfUniqueInts(vector<int>& arr, int k) {
        unordered_map<int, int> cnt;
        for (int& x : arr) {
            ++cnt[x];
        }
        vector<int> nums;
        for (auto& [_, c] : cnt) {
            nums.push_back(c);
        }
        sort(nums.begin(), nums.end());
        for (int i = 0, m = nums.size(); i < m; ++i) {
            k -= nums[i];
            if (k < 0) {
                return m - i;
            }
        }
        return 0;
    }
};
```

#### Go

```go
func findLeastNumOfUniqueInts(arr []int, k int) int {
	cnt := map[int]int{}
	for _, x := range arr {
		cnt[x]++
	}
	nums := make([]int, 0, len(cnt))
	for _, v := range cnt {
		nums = append(nums, v)
	}
	sort.Ints(nums)
	for i, v := range nums {
		k -= v
		if k < 0 {
			return len(nums) - i
		}
	}
	return 0
}
```

#### TypeScript

```ts
function findLeastNumOfUniqueInts(arr: number[], k: number): number {
    const cnt: Map<number, number> = new Map();
    for (const x of arr) {
        cnt.set(x, (cnt.get(x) ?? 0) + 1);
    }

    const nums = [...cnt.values()].sort((a, b) => a - b);

    for (let i = 0; i < nums.length; ++i) {
        k -= nums[i];
        if (k < 0) {
            return nums.length - i;
        }
    }
    return 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
