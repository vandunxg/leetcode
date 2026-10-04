---
comments: true
difficulty: Medium
rating: 1363
source: Weekly Contest 418 Q1
tags:
    - Bit Manipulation
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3309. Maximum Possible Number by Binary Concatenation](https://leetcode.com/problems/maximum-possible-number-by-binary-concatenation)

[中文文档](/solution/3300-3399/3309.Maximum%20Possible%20Number%20by%20Binary%20Concatenation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có kích thước 3.</p>

<p>Trả về <strong>số lớn nhất</strong> có <em>biểu diễn nhị phân</em> được tạo bằng cách <strong>nối</strong> <em>biểu diễn nhị phân</em> của <strong>tất cả</strong> phần tử trong <code>nums</code> theo một thứ tự nào đó.</p>

<p><strong>Lưu ý</strong> rằng biểu diễn nhị phân của mọi số <em>không</em> chứa các số 0 ở đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> 30</p>

<p><strong>Giải thích:</strong></p>

<p>Nối các số theo thứ tự <code>[3, 1, 2]</code> để nhận được kết quả <code>&quot;11110&quot;</code>, là biểu diễn nhị phân của 30.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,8,16]</span></p>

<p><strong>Đầu ra:</strong> 1296</p>

<p><strong>Giải thích:</strong></p>

<p>Nối các số theo thứ tự <code>[2, 8, 16]</code> để nhận được kết quả <code>&quot;10100010000&quot;</code>, là biểu diễn nhị phân của 1296.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums.length == 3</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 127</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mảng có đúng ba số, nên chỉ có $6$ hoán vị cần thử.
>
> Giá trị nguyên của một phép nối được quyết định bởi chuỗi bit sau khi nối; không cần thêm quy tắc phá hòa nào.
>
> Với mỗi hoán vị, ta nối các biểu diễn nhị phân của các số rồi phân tích kết quả thành một số nguyên nhị phân, đồng thời giữ lại giá trị lớn nhất.

<!-- thinking:end -->

Theo mô tả bài toán, độ dài của mảng $\textit{nums}$ là $3$. Ta có thể liệt kê tất cả các hoán vị của $\textit{nums}$, có tổng cộng $3! = 6$ hoán vị. Sau đó, chuyển các phần tử của mảng đã hoán vị thành các chuỗi nhị phân, nối các chuỗi này lại, rồi chuyển chuỗi nhị phân thu được thành số thập phân để lấy giá trị lớn nhất.

Độ phức tạp thời gian là $O(\log M)$, trong đó $M$ là giá trị lớn nhất của các phần tử trong $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxGoodNumber(self, nums: List[int]) -> int:
        ans = 0
        for arr in permutations(nums):
            num = int("".join(bin(i)[2:] for i in arr), 2)
            ans = max(ans, num)
        return ans
```

#### Java

```java
class Solution {
    private int[] nums;

    public int maxGoodNumber(int[] nums) {
        this.nums = nums;
        int ans = f(0, 1, 2);
        ans = Math.max(ans, f(0, 2, 1));
        ans = Math.max(ans, f(1, 0, 2));
        ans = Math.max(ans, f(1, 2, 0));
        ans = Math.max(ans, f(2, 0, 1));
        ans = Math.max(ans, f(2, 1, 0));
        return ans;
    }

    private int f(int i, int j, int k) {
        String a = Integer.toBinaryString(nums[i]);
        String b = Integer.toBinaryString(nums[j]);
        String c = Integer.toBinaryString(nums[k]);
        return Integer.parseInt(a + b + c, 2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxGoodNumber(vector<int>& nums) {
        int ans = 0;
        auto f = [&](vector<int>& nums) {
            int res = 0;
            vector<int> t;
            for (int x : nums) {
                for (; x; x >>= 1) {
                    t.push_back(x & 1);
                }
            }
            while (t.size()) {
                res = res * 2 + t.back();
                t.pop_back();
            }
            return res;
        };
        for (int i = 0; i < 6; ++i) {
            ans = max(ans, f(nums));
            next_permutation(nums.begin(), nums.end());
        }
        return ans;
    }
};
```

#### Go

```go
func maxGoodNumber(nums []int) int {
	f := func(i, j, k int) int {
		a := strconv.FormatInt(int64(nums[i]), 2)
		b := strconv.FormatInt(int64(nums[j]), 2)
		c := strconv.FormatInt(int64(nums[k]), 2)
		res, _ := strconv.ParseInt(a+b+c, 2, 64)
		return int(res)
	}
	ans := f(0, 1, 2)
	ans = max(ans, f(0, 2, 1))
	ans = max(ans, f(1, 0, 2))
	ans = max(ans, f(1, 2, 0))
	ans = max(ans, f(2, 0, 1))
	ans = max(ans, f(2, 1, 0))
	return ans
}
```

#### TypeScript

```ts
function maxGoodNumber(nums: number[]): number {
    const f = (i: number, j: number, k: number): number => {
        const a = nums[i].toString(2);
        const b = nums[j].toString(2);
        const c = nums[k].toString(2);
        const res = parseInt(a + b + c, 2);
        return res;
    };

    let ans = f(0, 1, 2);
    ans = Math.max(ans, f(0, 2, 1));
    ans = Math.max(ans, f(1, 0, 2));
    ans = Math.max(ans, f(1, 2, 0));
    ans = Math.max(ans, f(2, 0, 1));
    ans = Math.max(ans, f(2, 1, 0));

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
