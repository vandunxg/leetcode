---
comments: true
difficulty: Medium
rating: 1392
source: Biweekly Contest 114 Q2
tags:
    - Greedy
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2870. Minimum Number of Operations to Make Array Empty](https://leetcode.com/problems/minimum-number-of-operations-to-make-array-empty)

[中文文档](/solution/2800-2899/2870.Minimum%20Number%20of%20Operations%20to%20Make%20Array%20Empty/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>được đánh chỉ số từ 0</strong> <code>nums</code> gồm các số nguyên dương.</p>

<p>Có hai loại thao tác mà bạn có thể thực hiện trên mảng <strong>một số lần bất kỳ</strong>:</p>

<ul>
	<li>Chọn <strong>hai</strong> phần tử có giá trị <strong>bằng nhau</strong> và <strong>xóa</strong> chúng khỏi mảng.</li>
	<li>Chọn <strong>ba</strong> phần tử có giá trị <strong>bằng nhau</strong> và <strong>xóa</strong> chúng khỏi mảng.</li>
</ul>

<p>Trả về <em><strong>số thao tác nhỏ nhất</strong> cần thực hiện để làm mảng rỗng, hoặc </em><code>-1</code><em> nếu không thể thực hiện được.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,3,2,2,4,2,3,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau để làm mảng rỗng:
- Thực hiện thao tác đầu tiên trên các phần tử ở chỉ số 0 và 3. Mảng sau đó là nums = [3,3,2,4,2,3,4].
- Thực hiện thao tác đầu tiên trên các phần tử ở chỉ số 2 và 4. Mảng sau đó là nums = [3,3,4,3,4].
- Thực hiện thao tác thứ hai trên các phần tử ở chỉ số 0, 1 và 3. Mảng sau đó là nums = [4,4].
- Thực hiện thao tác đầu tiên trên các phần tử ở chỉ số 0 và 1. Mảng sau đó là nums = [].
Có thể chứng minh rằng không thể làm mảng rỗng trong ít hơn 4 thao tác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,2,2,3,3]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể làm mảng rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài toán này giống với bài <a href="https://leetcode.com/problems/minimum-rounds-to-complete-all-tasks/description/" target="_blank">2244: Minimum Rounds to Complete All Tasks.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác xóa hai hoặc ba phần tử có cùng giá trị. Nếu có tần suất nào bằng $1$, ta không thể làm mảng rỗng; nếu không, số thao tác ít nhất với số lượng $c$ là $\lceil c/3\rceil$, được viết là $(c+2)//3$.

<!-- thinking:end -->

Ta dùng một hash table $count$ để đếm số lần xuất hiện của mỗi phần tử trong mảng. Sau đó, ta duyệt qua hash table. Với mỗi phần tử $x$, nếu nó xuất hiện $c$ lần, ta có thể thực hiện $\lfloor \frac{c+2}{3} \rfloor$ thao tác để xóa $x$. Cuối cùng, ta trả về tổng số thao tác của tất cả các phần tử.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        count = Counter(nums)
        ans = 0
        for c in count.values():
            if c == 1:
                return -1
            ans += (c + 2) // 3
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) {
            // count.put(num, count.getOrDefault(num, 0) + 1);
            count.merge(num, 1, Integer::sum);
        }
        int ans = 0;
        for (int c : count.values()) {
            if (c < 2) {
                return -1;
            }
            int r = c % 3;
            int d = c / 3;
            switch (r) {
                case (0) -> {
                    ans += d;
                }
                default -> {
                    ans += d + 1;
                }
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
    int minOperations(vector<int>& nums) {
        unordered_map<int, int> count;
        for (int num : nums) {
            ++count[num];
        }
        int ans = 0;
        for (auto& [_, c] : count) {
            if (c < 2) {
                return -1;
            }
            ans += (c + 2) / 3;
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) (ans int) {
	count := map[int]int{}
	for _, num := range nums {
		count[num]++
	}
	for _, c := range count {
		if c < 2 {
			return -1
		}
		ans += (c + 2) / 3
	}
	return
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    const count: Map<number, number> = new Map();
    for (const num of nums) {
        count.set(num, (count.get(num) ?? 0) + 1);
    }
    let ans = 0;
    for (const [_, c] of count) {
        if (c < 2) {
            return -1;
        }
        ans += ((c + 2) / 3) | 0;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
