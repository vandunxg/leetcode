---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [4066. Maximum Equal Adjacent Pairs After at Most One Replacement](https://leetcode.com/problems/maximum-equal-adjacent-pairs-after-at-most-one-replacement)

[Tài liệu tiếng Trung](/solution/4000-4099/4066.Maximum%20Equal%20Adjacent%20Pairs%20After%20at%20Most%20One%20Replacement/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số bắt đầu từ <strong>1</strong>.</p>

<p>Bạn có thể chọn hai giá trị <strong>phân biệt</strong> <code>x</code> và <code>y</code>, rồi thực hiện thao tác sau <strong>không quá</strong> một lần:</p>

<ul>
	<li>Thay mọi lần xuất hiện của <code>x</code> trong <code>nums</code> bằng <code>y</code>.</li>
</ul>

<p>Hãy trả về số lượng <strong>lớn nhất</strong> các cặp phần tử kề nhau bằng nhau có thể có sau khi thực hiện thao tác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Một cách chọn tối ưu là chọn <code>x = 3</code> và <code>y = 2</code>.</li>
	<li>Mảng sau khi thay thế là <code>[1, 2, 2, 2]</code>.</li>
	<li>Có 2 cặp phần tử kề nhau bằng nhau: <code>(nums[2], nums[3])</code> và <code>(nums[3], nums[4])</code>.</li>
	<li>Do đó, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Một cách chọn tối ưu là chọn <code>x = 1</code> và <code>y = 2</code>.</li>
	<li>Mảng sau khi thay thế là <code>[2, 2, 2, 2, 2]</code>.</li>
	<li>Có 4 cặp phần tử kề nhau bằng nhau: <code>(nums[1], nums[2])</code>, <code>(nums[2], nums[3])</code>, <code>(nums[3], nums[4])</code> và <code>(nums[4], nums[5])</code>.</li>
	<li>Do đó, đáp án là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Một cách chọn tối ưu là không thực hiện thao tác nào.</li>
	<li>Vì vậy, mảng sau cùng là <code>[1, 1, 1]</code>.</li>
	<li>Có 2 cặp phần tử kề nhau bằng nhau: <code>(nums[1], nums[2])</code> và <code>(nums[2], nums[3])</code>.</li>
	<li>Do đó, đáp án là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Map

<!-- thinking:start -->

> **Tư duy**
>
> $n$ có thể bằng $10^5$ và các giá trị có thể bằng $10^9$. Việc thử mọi cặp giá trị phân biệt $x,y$, thực hiện thay thế rồi đếm lại các cặp phần tử kề nhau bằng nhau sẽ tạo ra quá nhiều ứng viên.
>
> Một cặp phần tử kề nhau vốn đã bằng nhau sẽ vẫn bằng nhau dù thay giá trị nào bằng giá trị nào. Một lần thay thế chỉ làm các vị trí kề nhau bằng nhau nếu giá trị của chúng chính xác là cặp $x,y$ đó. Một cặp không thứ tự khác tương ứng với một thao tác khác.
>
> Hãy đếm các vị trí kề nhau vốn đã bằng nhau, sau đó đếm các vị trí kề nhau không bằng nhau theo từng cặp không thứ tự và cộng thêm số lượng lớn nhất trong các cặp đó. Không thực hiện thao tác nào tương ứng với giá trị lớn nhất bằng $0$.

<!-- thinking:end -->

Các vị trí kề nhau vốn đã bằng nhau sẽ vẫn bằng nhau sau mọi phép thay thế: nếu cả hai đều chứa $x$, cả hai sẽ trở thành $y$, còn mọi giá trị khác không đổi. Gọi $\textit{ans}$ là số lượng các cặp như vậy.

Một thao tác chọn hai giá trị phân biệt và thay mọi lần xuất hiện của một giá trị bằng giá trị còn lại. Một cặp phần tử kề nhau mới trở nên bằng nhau phải ban đầu chính xác là hai giá trị đó. Với một cặp phần tử kề nhau không bằng nhau $x,y$, đặt giá trị nhỏ hơn lên trước và mã hóa

$$
\textit{key}=(x\ll 30)\mid y.
$$

Vì $x,y\le 10^9$, key vừa với một số nguyên $64$-bit. $\textit{cnt}[\textit{key}]$ là số lần cặp đó xuất hiện ở các vị trí kề nhau. Với mỗi thao tác ứng viên, số lượng cặp phần tử kề nhau mới trở nên bằng nhau là giá trị lớn nhất trong các biến đếm này, ký hiệu là $\textit{mx}$. Bỏ qua thao tác tương ứng với $\textit{mx}=0$. Đáp án là $\textit{ans}+\textit{mx}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxEqualAdjacentPairs(self, nums: list[int]) -> int:
        cnt = defaultdict(int)
        ans = mx = 0
        for x, y in pairwise(nums):
            if x == y:
                ans += 1
            else:
                if x > y:
                    x, y = y, x
                key = x << 30 | y
                cnt[key] += 1
                mx = max(mx, cnt[key])
        ans += mx
        return ans
```

#### Java

```java
class Solution {
    public int maxEqualAdjacentPairs(int[] nums) {
        Map<Long, Integer> cnt = new HashMap<>();
        int ans = 0, mx = 0;

        for (int i = 0; i + 1 < nums.length; i++) {
            int x = nums[i], y = nums[i + 1];
            if (x == y) {
                ans++;
            } else {
                if (x > y) {
                    int t = x;
                    x = y;
                    y = t;
                }
                long key = ((long) x << 30) | y;
                int v = cnt.merge(key, 1, Integer::sum);
                mx = Math.max(mx, v);
            }
        }
        ans += mx;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxEqualAdjacentPairs(vector<int>& nums) {
        unordered_map<long long, int> cnt;
        int ans = 0, mx = 0;

        for (int i = 0; i + 1 < nums.size(); i++) {
            int x = nums[i], y = nums[i + 1];
            if (x == y) {
                ans++;
            } else {
                if (x > y) {
                    swap(x, y);
                }
                long long key = ((long long) x << 30) | y;
                mx = max(mx, ++cnt[key]);
            }
        }
        ans += mx;
        return ans;
    }
};
```

#### Go

```go
func maxEqualAdjacentPairs(nums []int) int {
	cnt := map[int64]int{}
	ans, mx := 0, 0

	for i := 0; i+1 < len(nums); i++ {
		x, y := nums[i], nums[i+1]
		if x == y {
			ans++
		} else {
			if x > y {
				x, y = y, x
			}
			key := int64(x)<<30 | int64(y)
			cnt[key]++
			if cnt[key] > mx {
				mx = cnt[key]
			}
		}
	}
	ans += mx
	return ans
}
```

#### TypeScript

```ts
function maxEqualAdjacentPairs(nums: number[]): number {
    const cnt = new Map<number, number>();
    let ans = 0;
    let mx = 0;

    for (let i = 0; i + 1 < nums.length; i++) {
        let x = nums[i];
        let y = nums[i + 1];
        if (x === y) {
            ans++;
        } else {
            if (x > y) {
                [x, y] = [y, x];
            }
            const key = x * 2 ** 30 + y;
            cnt.set(key, (cnt.get(key) || 0) + 1);
            mx = Math.max(mx, cnt.get(key)!);
        }
    }
    ans += mx;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
