---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [446. Arithmetic Slices II - Subsequence](https://leetcode.com/problems/arithmetic-slices-ii-subsequence)

[中文文档](/solution/0400-0499/0446.Arithmetic%20Slices%20II%20-%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về <em>số lượng tất cả <strong>dãy con số học</strong> của</em> <code>nums</code>.</p>

<p>Một dãy số được gọi là dãy số học nếu có <strong>ít nhất ba phần tử</strong> và hiệu giữa mọi cặp phần tử liên tiếp đều bằng nhau.</p>

<ul>
	<li>Ví dụ, <code>[1, 3, 5, 7, 9]</code>, <code>[7, 7, 7, 7]</code> và <code>[3, -1, -5, -9]</code> là các dãy số học.</li>
	<li>Ví dụ, <code>[1, 1, 2, 5, 7]</code> không phải là dãy số học.</li>
</ul>

<p><strong>Dãy con</strong> của một mảng là dãy có thể tạo ra bằng cách xóa một số phần tử (có thể không xóa phần tử nào) khỏi mảng.</p>

<ul>
	<li>Ví dụ, <code>[2,5,10]</code> là dãy con của <code>[1,2,1,<strong><u>2</u></strong>,4,1,<u><strong>5</strong></u>,<u><strong>10</strong></u>]</code>.</li>
</ul>

<p>Các test case được tạo sao cho đáp án vừa với số nguyên <strong>32-bit</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,6,8,10]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Tất cả các dãy con số học là:
[2,4,6]
[4,6,8]
[6,8,10]
[2,4,6,8]
[4,6,8,10]
[2,4,6,8,10]
[2,6,10]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,7,7,7,7]
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Mọi dãy con của mảng này đều là dãy số học.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1&nbsp; &lt;= nums.length &lt;= 1000</code></li>
	<li><code>-2<sup>31</sup> &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Dãy con không cần liên tiếp và hiệu có thể rất lớn, nên không thể liệt kê mọi dãy con có độ dài ít nhất $3$. Với $n\le 1000$, độ phức tạp $O(n^2)$ là khả thi.
>
> Gọi $f[i][d]$ là số dãy con số học yếu (có ít nhất hai phần tử) kết thúc tại $i$ với công sai $d$. Với $j<i$ và $d=nums[i]-nums[j]$, khi thêm $nums[i]$ thì các dãy trong $f[j][d]$ trở thành dãy con số học hợp lệ; đồng thời $f[i][d]$ tăng thêm $f[j][d]+1$ (bao gồm cả cặp mới).
>
> Lưu các công sai trong hash map. Cộng $f[j][d]$ vào đáp án trước khi cập nhật $f[i][d]$, nhờ đó xử lý dãy yếu và dãy con số học hợp lệ trong cùng hai vòng lặp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfArithmeticSlices(self, nums: List[int]) -> int:
        f = [defaultdict(int) for _ in nums]
        ans = 0
        for i, x in enumerate(nums):
            for j, y in enumerate(nums[:i]):
                d = x - y
                ans += f[j][d]
                f[i][d] += f[j][d] + 1
        return ans
```

#### Java

```java
class Solution {
    public int numberOfArithmeticSlices(int[] nums) {
        int n = nums.length;
        Map<Long, Integer>[] f = new Map[n];
        Arrays.setAll(f, k -> new HashMap<>());
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                Long d = 1L * nums[i] - nums[j];
                int cnt = f[j].getOrDefault(d, 0);
                ans += cnt;
                f[i].merge(d, cnt + 1, Integer::sum);
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
    int numberOfArithmeticSlices(vector<int>& nums) {
        int n = nums.size();
        unordered_map<long long, int> f[n];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                long long d = 1LL * nums[i] - nums[j];
                int cnt = f[j][d];
                ans += cnt;
                f[i][d] += cnt + 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfArithmeticSlices(nums []int) (ans int) {
	f := make([]map[int]int, len(nums))
	for i := range f {
		f[i] = map[int]int{}
	}
	for i, x := range nums {
		for j, y := range nums[:i] {
			d := x - y
			cnt := f[j][d]
			ans += cnt
			f[i][d] += cnt + 1
		}
	}
	return
}
```

#### TypeScript

```ts
function numberOfArithmeticSlices(nums: number[]): number {
    const n = nums.length;
    const f: Map<number, number>[] = new Array(n).fill(0).map(() => new Map());
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            const d = nums[i] - nums[j];
            const cnt = f[j].get(d) || 0;
            ans += cnt;
            f[i].set(d, (f[i].get(d) || 0) + cnt + 1);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
