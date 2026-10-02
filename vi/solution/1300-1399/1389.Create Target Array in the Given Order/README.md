---
comments: true
difficulty: Easy
rating: 1208
source: Weekly Contest 181 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [1389. Create Target Array in the Given Order](https://leetcode.com/problems/create-target-array-in-the-given-order)

[中文文档](/solution/1300-1399/1389.Create%20Target%20Array%20in%20the%20Given%20Order/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums</code> và <code>index</code>. Nhiệm vụ của bạn là tạo mảng <em>target</em> theo các quy tắc sau:</p>

<ul>
	<li>Ban đầu, mảng <em>target</em> rỗng.</li>
	<li>Đọc <code>nums[i]</code> và <code>index[i]</code> từ trái sang phải, rồi chèn giá trị <code>nums[i]</code> vào mảng <em>target</em> tại chỉ số <code>index[i]</code>.</li>
	<li>Lặp lại bước trên cho đến khi không còn phần tử nào để đọc trong <code>nums</code> và <code>index.</code></li>
</ul>

<p>Trả về mảng <em>target</em>.</p>

<p>Đảm bảo rằng các thao tác chèn đều hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,2,3,4], index = [0,1,2,2,1]
<strong>Đầu ra:</strong> [0,4,1,3,2]
<strong>Giải thích:</strong>
nums       index     target
0            0        [0]
1            1        [0,1]
2            2        [0,1,2]
3            2        [0,1,3,2]
4            1        [0,4,1,3,2]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,0], index = [0,1,2,3,0]
<strong>Đầu ra:</strong> [0,1,2,3,4]
<strong>Giải thích:</strong>
nums       index     target
1            0        [1]
2            1        [1,2]
3            2        [1,2,3]
4            3        [1,2,3,4]
0            0        [0,1,2,3,4]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1], index = [0]
<strong>Đầu ra:</strong> [1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length, index.length &lt;= 100</code></li>
	<li><code>nums.length == index.length</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>0 &lt;= index[i] &lt;= i</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chèn $\textit{nums}[i]$ tại $\textit{index}[i]$; chỉ số luôn hợp lệ. Vì $n \le 100$, ta có thể dùng trực tiếp $\textit{insert}$, thao tác này sẽ dịch chuyển các phần tử phía sau mỗi lần chèn.

<!-- thinking:end -->

Ta tạo một danh sách $target$ để lưu mảng kết quả. Vì đề bài đảm bảo vị trí chèn luôn hợp lệ, ta có thể lần lượt chèn các phần tử vào đúng vị trí theo thứ tự đã cho.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def createTargetArray(self, nums: List[int], index: List[int]) -> List[int]:
        target = []
        for x, i in zip(nums, index):
            target.insert(i, x)
        return target
```

#### Java

```java
class Solution {
    public int[] createTargetArray(int[] nums, int[] index) {
        int n = nums.length;
        List<Integer> target = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            target.add(index[i], nums[i]);
        }
        // return target.stream().mapToInt(i -> i).toArray();
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = target.get(i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> createTargetArray(vector<int>& nums, vector<int>& index) {
        vector<int> target;
        for (int i = 0; i < nums.size(); ++i) {
            target.insert(target.begin() + index[i], nums[i]);
        }
        return target;
    }
};
```

#### Go

```go
func createTargetArray(nums []int, index []int) []int {
	target := make([]int, len(nums))
	for i, x := range nums {
		copy(target[index[i]+1:], target[index[i]:])
		target[index[i]] = x
	}
	return target
}
```

#### TypeScript

```ts
function createTargetArray(nums: number[], index: number[]): number[] {
    const ans: number[] = [];
    for (let i = 0; i < nums.length; i++) {
        ans.splice(index[i], 0, nums[i]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
