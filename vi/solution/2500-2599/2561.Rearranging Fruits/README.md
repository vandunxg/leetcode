---
comments: true
difficulty: Hard
rating: 2221
source: Weekly Contest 331 Q4
tags:
    - Greedy
    - Sort
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2561. Rearranging Fruits](https://leetcode.com/problems/rearranging-fruits)

[中文文档](/solution/2500-2599/2561.Rearranging%20Fruits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có hai giỏ trái cây, mỗi giỏ chứa <code>n</code> quả. Cho hai mảng số nguyên <code>basket1</code> và <code>basket2</code> được đánh chỉ số từ <strong>0</strong>, lần lượt biểu diễn giá của các loại quả trong mỗi giỏ. Bạn muốn làm cho hai giỏ <strong>giống nhau</strong>. Để thực hiện việc đó, bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý:</p>

<ul>
	<li>Chọn hai chỉ số <code>i</code> và <code>j</code>, rồi đổi loại quả thứ <code>i<sup><font size="1">th</font></sup></code> của <code>basket1</code> với loại quả thứ <code>j<sup><font size="1">th</font></sup></code> của <code>basket2</code>.</li>
	<li>Chi phí của phép đổi là <code>min(basket1[i], basket2[j])</code>.</li>
</ul>

<p>Hai giỏ được xem là giống nhau nếu sau khi sắp xếp theo giá quả, chúng trở thành hai giỏ hoàn toàn giống nhau.</p>

<p>Trả về <em>chi phí nhỏ nhất để làm cho hai giỏ giống nhau, hoặc </em><code>-1</code><em> nếu không thể.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> basket1 = [4,2,2,2], basket2 = [1,4,1,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Đổi chỉ số 1 của basket1 với chỉ số 0 của basket2, với chi phí là 1. Khi đó basket1 = [4,1,2,2] và basket2 = [2,4,1,2]. Sắp xếp lại hai mảng thì chúng trở nên giống nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> basket1 = [2,3,4,1], basket2 = [3,2,5,1]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng không thể làm cho hai giỏ giống nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>basket1.length == basket2.length</code></li>
	<li><code>1 &lt;= basket1.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= basket1[i], basket2[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Xây dựng

<!-- thinking:start -->

> **Tư duy**
>
> Đổi các loại quả giữa hai giỏ cho đến khi hai multiset giống nhau; chi phí của một lần đổi là giá trị nhỏ hơn. Chênh lệch tần suất cho biết những loại quả nào cần được chuyển; nếu có chênh lệch lẻ thì không thể thực hiện.
>
> Các giá trị cần rời khỏi giỏ được sắp xếp, rồi ghép nửa rẻ hơn với nửa đắt hơn. Đổi trực tiếp có chi phí là quả có giá nhỏ hơn; đi qua giá trị nhỏ nhất toàn cục $mi$ có chi phí $2mi$. Cộng $\min(x,2mi)$ trên nửa rẻ hơn.

<!-- thinking:end -->

Trước hết, ta có thể loại bỏ các phần tử chung khỏi hai mảng. Với các số còn lại, số lần xuất hiện của mỗi số phải là số chẵn, nếu không thì không thể tạo ra hai mảng giống nhau. Gọi hai mảng sau khi loại bỏ các phần tử chung là $a$ và $b$.

Tiếp theo, ta xem xét cách thực hiện các phép đổi.

Nếu muốn đổi số nhỏ nhất trong $a$, ta cần tìm số lớn nhất trong $b$ để đổi với nó; tương tự, nếu muốn đổi số nhỏ nhất trong $b$, ta cần tìm số lớn nhất trong $a$ để đổi với nó. Việc này có thể thực hiện bằng cách sắp xếp.

Tuy nhiên, còn một cách đổi khác. Ta có thể dùng một số nhỏ nhất $mi$ làm phần tử trung gian: trước tiên đổi số trong $a$ với $mi$, sau đó đổi $mi$ với số trong $b$. Khi đó, chi phí đổi là $2 \times mi$.

Trong phần cài đặt, ta có thể gộp trực tiếp hai mảng $a$ và $b$ thành một mảng $nums$, rồi sắp xếp mảng $nums$. Sau đó, ta duyệt nửa đầu của các số, tính chi phí nhỏ nhất trong mỗi lần và cộng vào đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, basket1: List[int], basket2: List[int]) -> int:
        cnt = Counter()
        for a, b in zip(basket1, basket2):
            cnt[a] += 1
            cnt[b] -= 1
        mi = min(cnt)
        nums = []
        for x, v in cnt.items():
            if v % 2:
                return -1
            nums.extend([x] * (abs(v) // 2))
        nums.sort()
        m = len(nums) // 2
        return sum(min(x, mi * 2) for x in nums[:m])
```

#### Java

```java
class Solution {
    public long minCost(int[] basket1, int[] basket2) {
        int n = basket1.length;
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            cnt.merge(basket1[i], 1, Integer::sum);
            cnt.merge(basket2[i], -1, Integer::sum);
        }
        int mi = 1 << 30;
        List<Integer> nums = new ArrayList<>();
        for (var e : cnt.entrySet()) {
            int x = e.getKey(), v = e.getValue();
            if (v % 2 != 0) {
                return -1;
            }
            for (int i = Math.abs(v) / 2; i > 0; --i) {
                nums.add(x);
            }
            mi = Math.min(mi, x);
        }
        Collections.sort(nums);
        int m = nums.size();
        long ans = 0;
        for (int i = 0; i < m / 2; ++i) {
            ans += Math.min(nums.get(i), mi * 2);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minCost(vector<int>& basket1, vector<int>& basket2) {
        int n = basket1.size();
        unordered_map<int, int> cnt;
        for (int i = 0; i < n; ++i) {
            cnt[basket1[i]]++;
            cnt[basket2[i]]--;
        }
        int mi = 1 << 30;
        vector<int> nums;
        for (auto& [x, v] : cnt) {
            if (v % 2) {
                return -1;
            }
            for (int i = abs(v) / 2; i; --i) {
                nums.push_back(x);
            }
            mi = min(mi, x);
        }
        ranges::sort(nums);
        int m = nums.size();
        long long ans = 0;
        for (int i = 0; i < m / 2; ++i) {
            ans += min(nums[i], mi * 2);
        }
        return ans;
    }
};
```

#### Go

```go
func minCost(basket1 []int, basket2 []int) (ans int64) {
	cnt := map[int]int{}
	for i, a := range basket1 {
		cnt[a]++
		cnt[basket2[i]]--
	}
	mi := 1 << 30
	nums := []int{}
	for x, v := range cnt {
		if v%2 != 0 {
			return -1
		}
		for i := abs(v) / 2; i > 0; i-- {
			nums = append(nums, x)
		}
		mi = min(mi, x)
	}
	sort.Ints(nums)
	m := len(nums)
	for i := 0; i < m/2; i++ {
		ans += int64(min(nums[i], mi*2))
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minCost(basket1: number[], basket2: number[]): number {
    const n = basket1.length;
    const cnt: Map<number, number> = new Map();
    for (let i = 0; i < n; i++) {
        cnt.set(basket1[i], (cnt.get(basket1[i]) || 0) + 1);
        cnt.set(basket2[i], (cnt.get(basket2[i]) || 0) - 1);
    }
    let mi = Number.MAX_SAFE_INTEGER;
    const nums: number[] = [];
    for (const [x, v] of cnt.entries()) {
        if (v % 2 !== 0) {
            return -1;
        }
        for (let i = 0; i < Math.abs(v) / 2; i++) {
            nums.push(x);
        }
        mi = Math.min(mi, x);
    }

    nums.sort((a, b) => a - b);
    const m = nums.length;
    let ans = 0;
    for (let i = 0; i < m / 2; i++) {
        ans += Math.min(nums[i], mi * 2);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn min_cost(basket1: Vec<i32>, basket2: Vec<i32>) -> i64 {
        let n = basket1.len();
        let mut cnt: HashMap<i32, i32> = HashMap::new();

        for i in 0..n {
            *cnt.entry(basket1[i]).or_insert(0) += 1;
            *cnt.entry(basket2[i]).or_insert(0) -= 1;
        }

        let mut mi = i32::MAX;
        let mut nums = Vec::new();

        for (x, v) in cnt {
            if v % 2 != 0 {
                return -1;
            }
            for _ in 0..(v.abs() / 2) {
                nums.push(x);
            }
            mi = mi.min(x);
        }

        nums.sort();

        let m = nums.len();
        let mut ans = 0;

        for i in 0..(m / 2) {
            ans += nums[i].min(mi * 2) as i64;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
