---
comments: true
difficulty: Medium
rating: 1940
source: Weekly Contest 352 Q3
tags:
    - Queue
    - Array
    - Ordered Set
    - Sliding Window
    - Monotonic Queue
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2762. Continuous Subarrays](https://leetcode.com/problems/continuous-subarrays)

[中文文档](/solution/2700-2799/2762.Continuous%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Một mảng con của <code>nums</code> được gọi là <strong>liên tục</strong> nếu:</p>

<ul>
	<li>Gọi <code>i</code>, <code>i + 1</code>, ..., <code>j</code><sub> </sub> là các chỉ số trong mảng con. Khi đó, với mọi cặp chỉ số <code>i &lt;= i<sub>1</sub>, i<sub>2</sub> &lt;= j</code>, ta có <code><font face="monospace">0 &lt;=</font> |nums[i<sub>1</sub>] - nums[i<sub>2</sub>]| &lt;= 2</code>.</li>
</ul>

<p>Trả về <em>tổng số mảng con <strong>liên tục</strong>.</em></p>

<p>Mảng con là một dãy phần tử <strong>không rỗng</strong> liên tiếp trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,4,2,4]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong>
Mảng con liên tục có kích thước 1: [5], [4], [2], [4].
Mảng con liên tục có kích thước 2: [5,4], [4,2], [2,4].
Mảng con liên tục có kích thước 3: [4,2,4].
Không có mảng con nào có kích thước 4.
Tổng số mảng con liên tục = 4 + 3 + 1 = 8.
Có thể chứng minh rằng không còn mảng con liên tục nào khác.
</pre>

<p>&nbsp;</p>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Mảng con liên tục có kích thước 1: [1], [2], [3].
Mảng con liên tục có kích thước 2: [1,2], [2,3].
Mảng con liên tục có kích thước 3: [1,2,3].
Tổng số mảng con liên tục = 3 + 2 + 1 = 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Danh sách có thứ tự + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con là liên tục khi và chỉ khi hiệu giữa giá trị lớn nhất và nhỏ nhất không vượt quá $2$. Kiểm tra mọi cặp đầu mút sẽ có độ phức tạp bậc hai, quá chậm khi độ dài mảng là $10^5$.
>
> Sau khi đầu phải thêm một giá trị, loại các phần tử ở đầu trái khỏi danh sách đã sắp xếp cho đến khi khoảng cách giữa giá trị lớn nhất và nhỏ nhất không vượt quá $2$. Số mảng con hợp lệ kết thúc tại vị trí này chính là kích thước của danh sách.

<!-- thinking:end -->

Ta có thể sử dụng hai con trỏ $i$ và $j$ để duy trì hai đầu mút trái và phải của mảng con hiện tại, đồng thời sử dụng một danh sách có thứ tự để duy trì tất cả phần tử trong mảng con hiện tại.

Duyệt qua mảng $nums$. Với số hiện tại $nums[i]$ đang được duyệt, ta thêm nó vào danh sách có thứ tự. Nếu hiệu giữa giá trị lớn nhất và nhỏ nhất trong danh sách có thứ tự lớn hơn $2$, ta lặp để di chuyển con trỏ $i$ sang phải, liên tục xóa $nums[i]$ khỏi danh sách có thứ tự, cho đến khi danh sách rỗng hoặc hiệu lớn nhất giữa các phần tử trong danh sách có thứ tự không lớn hơn $2$. Khi đó, số mảng con liên tiếp là $j - i + 1$, và ta cộng số này vào đáp án.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def continuousSubarrays(self, nums: List[int]) -> int:
        ans = i = 0
        sl = SortedList()
        for x in nums:
            sl.add(x)
            while sl[-1] - sl[0] > 2:
                sl.remove(nums[i])
                i += 1
            ans += len(sl)
        return ans
```

#### Java

```java
class Solution {
    public long continuousSubarrays(int[] nums) {
        long ans = 0;
        int i = 0, n = nums.length;
        TreeMap<Integer, Integer> tm = new TreeMap<>();
        for (int j = 0; j < n; ++j) {
            tm.merge(nums[j], 1, Integer::sum);
            while (tm.lastEntry().getKey() - tm.firstEntry().getKey() > 2) {
                tm.merge(nums[i], -1, Integer::sum);
                if (tm.get(nums[i]) == 0) {
                    tm.remove(nums[i]);
                }
                ++i;
            }
            ans += j - i + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long continuousSubarrays(vector<int>& nums) {
        long long ans = 0;
        int i = 0, n = nums.size();
        multiset<int> s;
        for (int j = 0; j < n; ++j) {
            s.insert(nums[j]);
            while (*s.rbegin() - *s.begin() > 2) {
                s.erase(s.find(nums[i++]));
            }
            ans += j - i + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func continuousSubarrays(nums []int) (ans int64) {
	i := 0
	tm := treemap.NewWithIntComparator()
	for j, x := range nums {
		if v, ok := tm.Get(x); ok {
			tm.Put(x, v.(int)+1)
		} else {
			tm.Put(x, 1)
		}
		for {
			a, _ := tm.Min()
			b, _ := tm.Max()
			if b.(int)-a.(int) > 2 {
				if v, _ := tm.Get(nums[i]); v.(int) == 1 {
					tm.Remove(nums[i])
				} else {
					tm.Put(nums[i], v.(int)-1)
				}
				i++
			} else {
				break
			}
		}
		ans += int64(j - i + 1)
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hàng đợi đơn điệu + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Danh sách đã sắp xếp phải trả thêm một thừa số logarit cho mỗi lần cập nhật. Vì chỉ các giá trị cực trị mới quan trọng, ta chỉ cần hai deque đơn điệu chứa các chỉ số. Khi khoảng cách vượt quá $2$, loại bỏ phần tử cực trị cũ hơn và dịch đầu trái. Cách đếm không thay đổi, đồng thời mỗi chỉ số chỉ được thêm và loại khỏi một deque đúng một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function continuousSubarrays(nums: number[]): number {
    const [minQ, maxQ]: [number[], number[]] = [[], []];
    const n = nums.length;
    let res = 0;

    for (let r = 0, l = 0; r < n; r++) {
        const x = nums[r];
        while (minQ.length && nums[minQ.at(-1)!] > x) minQ.pop();
        while (maxQ.length && nums[maxQ.at(-1)!] < x) maxQ.pop();
        minQ.push(r);
        maxQ.push(r);

        while (minQ.length && maxQ.length && nums[maxQ[0]] - nums[minQ[0]] > 2) {
            if (maxQ[0] < minQ[0]) {
                l = maxQ[0] + 1;
                maxQ.shift();
            } else {
                l = minQ[0] + 1;
                minQ.shift();
            }
        }

        res += r - l + 1;
    }

    return res;
}
```

#### JavaScript

```js
function continuousSubarrays(nums) {
    const [minQ, maxQ] = [[], []];
    const n = nums.length;
    let res = 0;

    for (let r = 0, l = 0; r < n; r++) {
        const x = nums[r];
        while (minQ.length && nums[minQ.at(-1)] > x) minQ.pop();
        while (maxQ.length && nums[maxQ.at(-1)] < x) maxQ.pop();
        minQ.push(r);
        maxQ.push(r);

        while (minQ.length && maxQ.length && nums[maxQ[0]] - nums[minQ[0]] > 2) {
            if (maxQ[0] < minQ[0]) {
                l = maxQ[0] + 1;
                maxQ.shift();
            } else {
                l = minQ[0] + 1;
                minQ.shift();
            }
        }

        res += r - l + 1;
    }

    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
