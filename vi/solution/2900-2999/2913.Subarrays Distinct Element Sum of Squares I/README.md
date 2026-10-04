---
comments: true
difficulty: Easy
rating: 1297
source: Biweekly Contest 116 Q1
tags:
    - Segment Tree
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2913. Subarrays Distinct Element Sum of Squares I](https://leetcode.com/problems/subarrays-distinct-element-sum-of-squares-i)

[中文文档](/solution/2900-2999/2913.Subarrays%20Distinct%20Element%20Sum%20of%20Squares%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>.</p>

<p><strong>Số lượng phần tử phân biệt</strong> của một mảng con trong <code>nums</code> được định nghĩa như sau:</p>

<ul>
	<li>Gọi <code>nums[i..j]</code> là một mảng con của <code>nums</code>, gồm tất cả các chỉ số từ <code>i</code> đến <code>j</code> sao cho <code>0 &lt;= i &lt;= j &lt; nums.length</code>. Khi đó, số lượng giá trị phân biệt trong <code>nums[i..j]</code> được gọi là số lượng phần tử phân biệt của <code>nums[i..j]</code>.</li>
</ul>

<p>Trả về <em>tổng <strong>bình phương</strong> của <strong>số lượng phần tử phân biệt</strong> của tất cả các mảng con của </em><code>nums</code>.</p>

<p>Mảng con là một dãy phần tử liên tiếp <strong>không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Có sáu mảng con khả dĩ:
[1]: 1 giá trị phân biệt
[2]: 1 giá trị phân biệt
[1]: 1 giá trị phân biệt
[1,2]: 2 giá trị phân biệt
[2,1]: 2 giá trị phân biệt
[1,2,1]: 2 giá trị phân biệt
Tổng bình phương của số lượng phần tử phân biệt trong tất cả các mảng con bằng 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> = 15.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có ba mảng con khả dĩ:
[1]: 1 giá trị phân biệt
[1]: 1 giá trị phân biệt
[1,1]: 1 giá trị phân biệt
Tổng bình phương của số lượng phần tử phân biệt trong tất cả các mảng con bằng 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> = 3.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính $\sum \mathrm{distinct}(sub)^2$. Vì $n \le 100$, ta có thể liệt kê tất cả $O(n^2)$ mảng con. Cố định đầu trái $i$ rồi mở rộng $j$, đồng thời thêm phần tử vào một set và cộng $|s|^2$ sau mỗi bước.
>
> Miền giá trị cũng không vượt quá $100$ nên các thao tác trên set đều nhanh. Không cần suy ra công thức đóng cho phần đóng góp.

<!-- thinking:end -->

Ta có thể liệt kê chỉ số đầu trái $i$ của mảng con, rồi với mỗi $i$, liệt kê chỉ số đầu phải $j$ trong đoạn $[i, n)$ và tính số lượng phần tử phân biệt của $nums[i..j]$ bằng cách thêm $nums[j]$ vào một set $s$, sau đó lấy bình phương kích thước của $s$ làm phần đóng góp của $nums[i..j]$ vào đáp án.

Sau khi liệt kê xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n^2)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumCounts(self, nums: List[int]) -> int:
        ans, n = 0, len(nums)
        for i in range(n):
            s = set()
            for j in range(i, n):
                s.add(nums[j])
                ans += len(s) * len(s)
        return ans
```

#### Java

```java
class Solution {
    public int sumCounts(List<Integer> nums) {
        int ans = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            int[] s = new int[101];
            int cnt = 0;
            for (int j = i; j < n; ++j) {
                if (++s[nums.get(j)] == 1) {
                    ++cnt;
                }
                ans += cnt * cnt;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumCounts(vector<int>& nums) {
        int ans = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            int s[101]{};
            int cnt = 0;
            for (int j = i; j < n; ++j) {
                if (++s[nums[j]] == 1) {
                    ++cnt;
                }
                ans += cnt * cnt;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sumCounts(nums []int) (ans int) {
	for i := range nums {
		s := [101]int{}
		cnt := 0
		for _, x := range nums[i:] {
			s[x]++
			if s[x] == 1 {
				cnt++
			}
			ans += cnt * cnt
		}
	}
	return
}
```

#### TypeScript

```ts
function sumCounts(nums: number[]): number {
    let ans = 0;
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        const s: number[] = Array(101).fill(0);
        let cnt = 0;
        for (const x of nums.slice(i)) {
            if (++s[x] === 1) {
                ++cnt;
            }
            ans += cnt * cnt;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
