---
comments: true
difficulty: Medium
rating: 1741
source: Weekly Contest 382 Q2
tags:
    - Array
    - Hash Table
    - Enumeration
---

<!-- problem:start -->

# [3020. Find the Maximum Number of Elements in Subset](https://leetcode.com/problems/find-the-maximum-number-of-elements-in-subset)

[中文文档](/solution/3000-3099/3020.Find%20the%20Maximum%20Number%20of%20Elements%20in%20Subset/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng gồm các số nguyên <strong>dương</strong> <code>nums</code>.</p>

<p>Bạn cần chọn một <span data-keyword="subset">tập con</span> của <code>nums</code> thỏa mãn điều kiện sau:</p>

<ul>
	<li>Bạn có thể sắp xếp các phần tử đã chọn vào một mảng <strong>đánh chỉ số từ 0</strong> sao cho mảng tuân theo mẫu: <code>[x, x<sup>2</sup>, x<sup>4</sup>, ..., x<sup>k/2</sup>, x<sup>k</sup>, x<sup>k/2</sup>, ..., x<sup>4</sup>, x<sup>2</sup>, x]</code> (<strong>Lưu ý</strong> rằng <code>k</code> có thể là bất kỳ lũy thừa <strong>không âm</strong> nào của <code>2</code>). Ví dụ, <code>[2, 4, 16, 4, 2]</code> và <code>[3, 9, 3]</code> tuân theo mẫu, còn <code>[2, 4, 8, 4, 2]</code> thì không.</li>
</ul>

<p>Hãy trả về <em>số lượng <strong>lớn nhất</strong> các phần tử trong một tập con thỏa mãn các điều kiện này.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,4,1,2,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể chọn tập con {4,2,2}, rồi sắp xếp thành mảng [2,4,2], tuân theo mẫu và 2<sup>2</sup> == 4. Do đó, đáp án là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,2,4]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể chọn tập con {1}, rồi sắp xếp thành mảng [1], tuân theo mẫu. Do đó, đáp án là 1. Lưu ý rằng ta cũng có thể chọn các tập con {2}, {3} hoặc {4}; có thể có nhiều tập con cho cùng một đáp án.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một tập con phải có dạng $x, x^2, x^4, \ldots, x^{2^{k}}, \ldots, x^4, x^2, x$ và có độ dài lẻ. Điều kiện $n \le 10^5$ khiến việc liệt kê các tập con là không khả thi.
>
> Chuỗi này được tạo ra bằng cách liên tục bình phương. Giá trị ở giữa chỉ được dùng một lần; mọi giá trị khác cần xuất hiện với số lần chẵn. Với mỗi $x$, ta bình phương liên tục cho đến khi số bản sao còn lại nhỏ hơn hai.
>
> Trường hợp $x=1$ không thay đổi sau khi bình phương nên được xử lý riêng như số lượng lớn nhất là số lẻ. Một hash map cung cấp tần suất xuất hiện.

<!-- thinking:end -->

Ta sử dụng một hash table $cnt$ để ghi lại số lần xuất hiện của mỗi phần tử trong mảng $nums$. Với mỗi phần tử $x$, ta liên tục bình phương nó cho đến khi số lần xuất hiện của nó trong hash table $cnt$ nhỏ hơn $2$. Tại thời điểm đó, ta kiểm tra xem số lần xuất hiện của $x$ trong hash table $cnt$ có bằng $1$ hay không. Nếu có, nghĩa là $x$ vẫn có thể được đưa vào tập con. Nếu không, ta cần loại bỏ một phần tử khỏi tập con để đảm bảo số phần tử trong tập con là số lẻ. Sau đó, ta cập nhật đáp án và tiếp tục duyệt phần tử tiếp theo.

Lưu ý rằng ta cần xử lý riêng trường hợp $x = 1$.

Độ phức tạp thời gian là $O(n \times \log \log M)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$, còn $M$ là giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumLength(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        ans = cnt[1] - (cnt[1] % 2 ^ 1)
        del cnt[1]
        for x in cnt:
            t = 0
            while cnt[x] > 1:
                x = x * x
                t += 2
            t += 1 if cnt[x] else -1
            ans = max(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public int maximumLength(int[] nums) {
        Map<Long, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge((long) x, 1, Integer::sum);
        }
        Integer t = cnt.remove(1L);
        int ans = t == null ? 0 : t - (t % 2 ^ 1);
        for (long x : cnt.keySet()) {
            t = 0;
            while (cnt.getOrDefault(x, 0) > 1) {
                x = x * x;
                t += 2;
            }
            t += cnt.getOrDefault(x, -1);
            ans = Math.max(ans, t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumLength(vector<int>& nums) {
        unordered_map<long long, int> cnt;
        for (int x : nums) {
            ++cnt[x];
        }
        int ans = cnt[1] - (cnt[1] % 2 ^ 1);
        cnt.erase(1);
        for (auto [v, _] : cnt) {
            int t = 0;
            long long x = v;
            while (cnt.count(x) && cnt[x] > 1) {
                x = x * x;
                t += 2;
            }
            t += cnt.count(x) ? 1 : -1;
            ans = max(ans, t);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumLength(nums []int) (ans int) {
	cnt := map[int]int{}
	for _, x := range nums {
		cnt[x]++
	}
	ans = cnt[1] - (cnt[1]%2 ^ 1)
	delete(cnt, 1)
	for x := range cnt {
		t := 0
		for cnt[x] > 1 {
			x = x * x
			t += 2
		}
		if cnt[x] > 0 {
			t += 1
		} else {
			t -= 1
		}
		ans = max(ans, t)
	}
	return
}
```

#### TypeScript

```ts
function maximumLength(nums: number[]): number {
    const cnt: Map<number, number> = new Map();
    for (const x of nums) {
        cnt.set(x, (cnt.get(x) ?? 0) + 1);
    }
    let ans = cnt.has(1) ? cnt.get(1)! - ((cnt.get(1)! % 2) ^ 1) : 0;
    cnt.delete(1);
    for (let [x, _] of cnt) {
        let t = 0;
        while (cnt.has(x) && cnt.get(x)! > 1) {
            x = x * x;
            t += 2;
        }
        t += cnt.has(x) ? 1 : -1;
        ans = Math.max(ans, t);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn maximum_length(nums: Vec<i32>) -> i32 {
        let mut cnt: HashMap<i64, i32> = HashMap::new();
        for &x in &nums {
            *cnt.entry(x as i64).or_insert(0) += 1;
        }

        let mut ans = 0;
        if let Some(t) = cnt.remove(&1) {
            ans = t - ((t % 2) ^ 1);
        }

        for &key in cnt.keys() {
            let mut x = key;
            let mut t = 0;
            while *cnt.get(&x).unwrap_or(&0) > 1 {
                x = x * x;
                t += 2;
            }
            t += cnt.get(&x).unwrap_or(&-1);
            ans = ans.max(t);
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MaximumLength(int[] nums) {
        Dictionary<long, int> cnt = new Dictionary<long, int>();
        foreach (int num in nums) {
            if (!cnt.ContainsKey(num)) {
                cnt[num] = 0;
            }
            cnt[num]++;
        }

        int ans = 0;
        if (cnt.ContainsKey(1)) {
            ans = cnt[1] - ((cnt[1] % 2) ^ 1);
            cnt.Remove(1);
        }

        foreach (long key in cnt.Keys) {
            long x = key;
            int t = 0;

            while (cnt.ContainsKey(x) && cnt[x] > 1) {
                x = x * x;
                t += 2;
            }

            t += (cnt.ContainsKey(x) && cnt[x] >= 1) ? 1 : -1;
            ans = Math.Max(ans, t);
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
