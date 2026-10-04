---
comments: true
difficulty: Medium
rating: 1875
source: Biweekly Contest 110 Q3
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2808. Minimum Seconds to Equalize a Circular Array](https://leetcode.com/problems/minimum-seconds-to-equalize-a-circular-array)

[Tài liệu tiếng Trung](/solution/2800-2899/2808.Minimum%20Seconds%20to%20Equalize%20a%20Circular%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>được đánh chỉ số từ 0</strong> <code>nums</code> gồm <code>n</code> số nguyên.</p>

<p>Mỗi giây, bạn thực hiện thao tác sau trên mảng:</p>

<ul>
	<li>Với mọi chỉ số <code>i</code> trong đoạn <code>[0, n - 1]</code>, thay <code>nums[i]</code> bằng một trong các giá trị <code>nums[i]</code>, <code>nums[(i - 1 + n) % n]</code> hoặc <code>nums[(i + 1) % n]</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng tất cả phần tử được thay thế đồng thời.</p>

<p>Hãy trả về <em><strong>số giây nhỏ nhất</strong> cần thiết để tất cả phần tử trong mảng</em> <code>nums</code> <em>trở nên bằng nhau</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể làm cho các phần tử trong mảng bằng nhau sau 1 giây như sau:
- Ở giây thứ <sup>1</sup>, thay các giá trị tại mỗi chỉ số bằng [nums[3],nums[1],nums[3],nums[3]]. Sau khi thay thế, nums = [2,2,2,2].
Có thể chứng minh rằng 1 là số giây nhỏ nhất cần thiết để làm cho các phần tử trong mảng bằng nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,3,3,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể làm cho các phần tử trong mảng bằng nhau sau 2 giây như sau:
- Ở giây thứ <sup>1</sup>, thay các giá trị tại mỗi chỉ số bằng [nums[0],nums[2],nums[2],nums[2],nums[3]]. Sau khi thay thế, nums = [2,3,3,3,3].
- Ở giây thứ <sup>2</sup>, thay các giá trị tại mỗi chỉ số bằng [nums[1],nums[1],nums[2],nums[3],nums[4]]. Sau khi thay thế, nums = [3,3,3,3,3].
Có thể chứng minh rằng 2 là số giây nhỏ nhất cần thiết để làm cho các phần tử trong mảng bằng nhau.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,5,5,5]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Ta không cần thực hiện thao tác nào vì tất cả phần tử trong mảng ban đầu đã giống nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Giá trị chung cuối cùng phải xuất hiện trong mảng. Mỗi bản sao của $x$ lan rộng một bước sang trái và phải sau mỗi giây, vì vậy thời gian trên vòng tròn bằng một nửa khoảng cách lớn nhất giữa hai lần xuất hiện liên tiếp, tính cả khoảng cách nối qua điểm đầu-cuối. Ta nhóm các chỉ số theo giá trị rồi lấy giá trị nhỏ nhất của $\lfloor t/2\rfloor$.

<!-- thinking:end -->

Ta giả sử cuối cùng tất cả phần tử đều trở thành $x$, và $x$ phải là một phần tử trong mảng.

Giá trị $x$ có thể lan rộng thêm một vị trí sang trái và phải sau mỗi giây. Nếu có nhiều phần tử cùng bằng $x$, thời gian cần để phủ kín toàn bộ mảng phụ thuộc vào khoảng cách lớn nhất giữa hai phần tử $x$ liền kề.

Do đó, ta lần lượt chọn mỗi phần tử làm $x$ cuối cùng, tính khoảng cách lớn nhất $t$ giữa hai phần tử liền kề có cùng giá trị $x$, rồi đáp án là $\min\limits_{x \in nums} \left\lfloor \frac{t}{2} \right\rfloor$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSeconds(self, nums: List[int]) -> int:
        d = defaultdict(list)
        for i, x in enumerate(nums):
            d[x].append(i)
        ans = inf
        n = len(nums)
        for idx in d.values():
            t = idx[0] + n - idx[-1]
            for i, j in pairwise(idx):
                t = max(t, j - i)
            ans = min(ans, t // 2)
        return ans
```

#### Java

```java
class Solution {
    public int minimumSeconds(List<Integer> nums) {
        Map<Integer, List<Integer>> d = new HashMap<>();
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            d.computeIfAbsent(nums.get(i), k -> new ArrayList<>()).add(i);
        }
        int ans = 1 << 30;
        for (List<Integer> idx : d.values()) {
            int m = idx.size();
            int t = idx.get(0) + n - idx.get(m - 1);
            for (int i = 1; i < m; ++i) {
                t = Math.max(t, idx.get(i) - idx.get(i - 1));
            }
            ans = Math.min(ans, t / 2);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSeconds(vector<int>& nums) {
        unordered_map<int, vector<int>> d;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            d[nums[i]].push_back(i);
        }
        int ans = 1 << 30;
        for (auto& [_, idx] : d) {
            int m = idx.size();
            int t = idx[0] + n - idx[m - 1];
            for (int i = 1; i < m; ++i) {
                t = max(t, idx[i] - idx[i - 1]);
            }
            ans = min(ans, t / 2);
        }
        return ans;
    }
};
```

#### Go

```go
func minimumSeconds(nums []int) int {
	d := map[int][]int{}
	for i, x := range nums {
		d[x] = append(d[x], i)
	}
	ans := 1 << 30
	n := len(nums)
	for _, idx := range d {
		m := len(idx)
		t := idx[0] + n - idx[m-1]
		for i := 1; i < m; i++ {
			t = max(t, idx[i]-idx[i-1])
		}
		ans = min(ans, t/2)
	}
	return ans
}
```

#### TypeScript

```ts
function minimumSeconds(nums: number[]): number {
    const d: Map<number, number[]> = new Map();
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        if (!d.has(nums[i])) {
            d.set(nums[i], []);
        }
        d.get(nums[i])!.push(i);
    }
    let ans = 1 << 30;
    for (const [_, idx] of d) {
        const m = idx.length;
        let t = idx[0] + n - idx[m - 1];
        for (let i = 1; i < m; ++i) {
            t = Math.max(t, idx[i] - idx[i - 1]);
        }
        ans = Math.min(ans, t >> 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
