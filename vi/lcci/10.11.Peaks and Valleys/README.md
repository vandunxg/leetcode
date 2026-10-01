---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [10.11. Peaks and Valleys](https://leetcode.cn/problems/peaks-and-valleys-lcci)

[中文文档](/lcci/10.11.Peaks%20and%20Valleys/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một mảng số nguyên, một &quot;đỉnh&quot; là phần tử lớn hơn hoặc bằng các phần tử kề nó, còn một &quot;đáy&quot; là phần tử nhỏ hơn hoặc bằng các phần tử kề nó. Ví dụ, trong mảng {5, 8, 6, 2, 3, 4, 6}, {8, 6} là các đỉnh và {5, 2} là các đáy. Cho một mảng số nguyên, hãy sắp xếp mảng thành một dãy xen kẽ giữa các đỉnh và đáy.</p>
<p><strong>Ví dụ:</strong></p>
<pre>

<strong>Đầu vào: </strong>[5, 3, 1, 2, 3]

<strong>Đầu ra:</strong>&nbsp;[5, 1, 3, 2, 3]

</pre>
<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>nums.length &lt;= 10000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mảng phải xen kẽ giữa các đỉnh và đáy. Việc sửa các nghịch thế kề nhau một cách cục bộ có thể làm hỏng phía còn lại.
>
> Sau khi sắp xếp toàn bộ, đổi chỗ mỗi chỉ số chẵn với phần tử kế tiếp sẽ đưa giá trị lớn hơn vào các chỉ số lẻ: nhỏ, lớn, nhỏ, lớn.
>
> Thực hiện `nums.sort()` rồi `nums[i:i+2]=reversed(...)` với mọi $i$ chẵn. Các cặp đã sắp xếp trở thành các đỉnh không nhỏ hơn các phần tử lân cận.

<!-- thinking:end -->

Trước tiên, chúng ta sắp xếp mảng, sau đó duyệt mảng và đổi chỗ các phần tử ở chỉ số chẵn với phần tử kế tiếp.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def wiggleSort(self, nums: List[int]) -> None:
        nums.sort()
        for i in range(0, len(nums), 2):
            nums[i : i + 2] = nums[i : i + 2][::-1]
```

#### Java

```java
class Solution {
    public void wiggleSort(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        for (int i = 0; i < n - 1; i += 2) {
            int t = nums[i];
            nums[i] = nums[i + 1];
            nums[i + 1] = t;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        for (int i = 0; i < n - 1; i += 2) {
            swap(nums[i], nums[i + 1]);
        }
    }
};
```

#### Go

```go
func wiggleSort(nums []int) {
	sort.Ints(nums)
	for i := 0; i < len(nums)-1; i += 2 {
		nums[i], nums[i+1] = nums[i+1], nums[i]
	}
}
```

#### TypeScript

```ts
/**
 Do not return anything, modify nums in-place instead.
 */
function wiggleSort(nums: number[]): void {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    for (let i = 0; i < n - 1; i += 2) {
        [nums[i], nums[i + 1]] = [nums[i + 1], nums[i]];
    }
}
```

#### Swift

```swift
class Solution {
    func wiggleSort(_ nums: inout [Int]) {
        nums.sort()

        let n = nums.count

        for i in stride(from: 0, to: n - 1, by: 2) {
            let temp = nums[i]
            nums[i] = nums[i + 1]
            nums[i + 1] = temp
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
