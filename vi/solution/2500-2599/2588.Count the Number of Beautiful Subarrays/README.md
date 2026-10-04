---
comments: true
difficulty: Medium
rating: 1696
source: Weekly Contest 336 Q3
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [2588. Count the Number of Beautiful Subarrays](https://leetcode.com/problems/count-the-number-of-beautiful-subarrays)

[中文文档](/solution/2500-2599/2588.Count%20the%20Number%20of%20Beautiful%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code>. Trong một thao tác, bạn có thể:</p>

<ul>
	<li>Chọn hai chỉ số khác nhau <code>i</code> và <code>j</code> sao cho <code>0 &lt;= i, j &lt; nums.length</code>.</li>
	<li>Chọn một số nguyên không âm <code>k</code> sao cho bit thứ <code>k<sup>th</sup></code> (<strong>đánh chỉ số từ 0</strong>) trong biểu diễn nhị phân của <code>nums[i]</code> và <code>nums[j]</code> đều bằng <code>1</code>.</li>
	<li>Trừ <code>2<sup>k</sup></code> khỏi <code>nums[i]</code> và <code>nums[j]</code>.</li>
</ul>

<p>Một mảng con là <strong>đẹp</strong> nếu có thể biến tất cả phần tử của nó thành <code>0</code> sau khi thực hiện thao tác trên một số lần bất kỳ, kể cả không thực hiện lần nào.</p>

<p>Trả về <em>số lượng <strong>mảng con đẹp</strong> trong mảng</em> <code>nums</code>.</p>

<p>Một mảng con là một dãy phần tử liên tiếp và <strong>không rỗng</strong> trong một mảng.</p>

<p><strong>Lưu ý</strong>: Các mảng con mà tất cả phần tử ban đầu đều bằng 0 cũng được xem là đẹp vì không cần thực hiện thao tác nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,1,2,4]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 mảng con đẹp trong nums: [4,<u>3,1,2</u>,4] và [<u>4,3,1,2,4</u>].
- Ta có thể biến tất cả phần tử trong mảng con [3,1,2] thành 0 như sau:
  - Chọn [<u>3</u>, 1, <u>2</u>] và k = 1. Trừ 2<sup>1</sup> khỏi cả hai số. Mảng con trở thành [1, 1, 0].
  - Chọn [<u>1</u>, <u>1</u>, 0] và k = 0. Trừ 2<sup>0</sup> khỏi cả hai số. Mảng con trở thành [0, 0, 0].
- Ta có thể biến tất cả phần tử trong mảng con [4,3,1,2,4] thành 0 như sau:
  - Chọn [<u>4</u>, 3, 1, 2, <u>4</u>] và k = 2. Trừ 2<sup>2</sup> khỏi cả hai số. Mảng con trở thành [0, 3, 1, 2, 0].
  - Chọn [0, <u>3</u>, <u>1</u>, 2, 0] và k = 0. Trừ 2<sup>0</sup> khỏi cả hai số. Mảng con trở thành [0, 2, 0, 2, 0].
  - Chọn [0, <u>2</u>, 0, <u>2</u>, 0] và k = 1. Trừ 2<sup>1</sup> khỏi cả hai số. Mảng con trở thành [0, 0, 0, 0, 0].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,10,4]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có mảng con đẹp nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix XOR + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con đẹp khi và chỉ khi ta có thể biến nó thành các số 0 bằng cách liên tục trừ $2^k$ khỏi hai bit 1, tức là số lượng bit 1 ở mỗi vị trí trong tất cả phần tử đều chẵn, hay XOR bằng $0$. Nếu kiểm tra mọi đoạn thì độ phức tạp là bậc hai.
>
> Hai prefix XOR bằng nhau xác định một đoạn có XOR bằng 0. Hash map đếm các prefix; $\textit{mask}$ hiện tại cộng thêm số prefix trước đó có cùng giá trị. Giá trị $0$ ban đầu dùng để tính các đoạn bắt đầu từ đầu mảng.

<!-- thinking:end -->

Ta nhận thấy một mảng con có thể trở thành một mảng toàn số $0$ khi và chỉ khi số lượng bit $1$s ở mỗi vị trí trong tất cả phần tử của mảng con là chẵn.

Nếu tồn tại hai chỉ số $i$ và $j$ sao cho $i \lt j$, đồng thời các mảng con $nums[0,..,i]$ và $nums[0,..,j]$ có tính chẵn lẻ của số lượng bit $1$s ở mỗi vị trí giống nhau, thì ta có thể biến mảng con $nums[i + 1,..,j]$ thành một mảng toàn số $0$.

Do đó, ta có thể sử dụng phương pháp prefix XOR và hash table $cnt$ để đếm số lần xuất hiện của mỗi giá trị prefix XOR. Ta duyệt qua mảng, với mỗi phần tử $x$, tính giá trị prefix XOR $mask$, sau đó cộng số lần xuất hiện của $mask$ vào đáp án. Tiếp theo, ta tăng số lần xuất hiện của $mask$ lên $1$.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def beautifulSubarrays(self, nums: List[int]) -> int:
        cnt = Counter({0: 1})
        ans = mask = 0
        for x in nums:
            mask ^= x
            ans += cnt[mask]
            cnt[mask] += 1
        return ans
```

#### Java

```java
class Solution {
    public long beautifulSubarrays(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        cnt.put(0, 1);
        long ans = 0;
        int mask = 0;
        for (int x : nums) {
            mask ^= x;
            ans += cnt.merge(mask, 1, Integer::sum) - 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long beautifulSubarrays(vector<int>& nums) {
        unordered_map<int, int> cnt{{0, 1}};
        long long ans = 0;
        int mask = 0;
        for (int x : nums) {
            mask ^= x;
            ans += cnt[mask]++;
        }
        return ans;
    }
};
```

#### Go

```go
func beautifulSubarrays(nums []int) (ans int64) {
	cnt := map[int]int{0: 1}
	mask := 0
	for _, x := range nums {
		mask ^= x
		ans += int64(cnt[mask])
		cnt[mask]++
	}
	return
}
```

#### TypeScript

```ts
function beautifulSubarrays(nums: number[]): number {
    const cnt = new Map();
    cnt.set(0, 1);
    let ans = 0;
    let mask = 0;
    for (const x of nums) {
        mask ^= x;
        ans += cnt.get(mask) || 0;
        cnt.set(mask, (cnt.get(mask) || 0) + 1);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn beautiful_subarrays(nums: Vec<i32>) -> i64 {
        let mut cnt = HashMap::new();
        cnt.insert(0, 1);
        let mut ans = 0;
        let mut mask = 0;
        for &x in nums.iter() {
            mask ^= x;
            ans += *cnt.get(&mask).unwrap_or(&0);
            *cnt.entry(mask).or_insert(0) += 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
