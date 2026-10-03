---
comments: true
difficulty: Easy
rating: 1354
source: Weekly Contest 285 Q1
tags:
    - Array
---

<!-- problem:start -->

# [2210. Count Hills and Valleys in an Array](https://leetcode.com/problems/count-hills-and-valleys-in-an-array)

[中文文档](/solution/2200-2299/2210.Count%20Hills%20and%20Valleys%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> <strong>đánh chỉ số từ 0</strong>. Một chỉ số <code>i</code> thuộc về một <strong>đỉnh</strong> trong <code>nums</code> nếu các phần tử lân cận gần nhất của <code>i</code> không bằng giá trị hiện tại của nó đều nhỏ hơn <code>nums[i]</code>. Tương tự, một chỉ số <code>i</code> thuộc về một <strong>thung lũng</strong> trong <code>nums</code> nếu các phần tử lân cận gần nhất của <code>i</code> không bằng giá trị hiện tại của nó đều lớn hơn <code>nums[i]</code>. Hai chỉ số liền kề <code>i</code> và <code>j</code> thuộc <strong>cùng</strong> một đỉnh hoặc thung lũng nếu <code>nums[i] == nums[j]</code>.</p>

<p>Lưu ý rằng để một chỉ số thuộc về một đỉnh hoặc thung lũng, phải có một phần tử lân cận khác nó ở <strong>cả</strong> bên trái và bên phải của chỉ số đó.</p>

<p>Trả về <i>số đỉnh và thung lũng trong </i><code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,1,1,6,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Tại chỉ số 0: Không có phần tử lân cận khác 2 ở bên trái, nên chỉ số 0 không phải là đỉnh cũng không phải là thung lũng.
Tại chỉ số 1: Các phần tử lân cận khác 4 gần nhất là 2 và 1. Vì 4 &gt; 2 và 4 &gt; 1, chỉ số 1 là một đỉnh.
Tại chỉ số 2: Các phần tử lân cận khác 1 gần nhất là 4 và 6. Vì 1 &lt; 4 và 1 &lt; 6, chỉ số 2 là một thung lũng.
Tại chỉ số 3: Các phần tử lân cận khác 1 gần nhất là 4 và 6. Vì 1 &lt; 4 và 1 &lt; 6, chỉ số 3 là một thung lũng, nhưng nó thuộc cùng thung lũng với chỉ số 2.
Tại chỉ số 4: Các phần tử lân cận khác 6 gần nhất là 1 và 5. Vì 6 &gt; 1 và 6 &gt; 5, chỉ số 4 là một đỉnh.
Tại chỉ số 5: Không có phần tử lân cận khác 5 ở bên phải, nên chỉ số 5 không phải là đỉnh cũng không phải là thung lũng.
Có 3 đỉnh và thung lũng, nên ta trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,6,5,5,4,1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Tại chỉ số 0: Không có phần tử lân cận khác 6 ở bên trái, nên chỉ số 0 không phải là đỉnh cũng không phải là thung lũng.
Tại chỉ số 1: Không có phần tử lân cận khác 6 ở bên trái, nên chỉ số 1 không phải là đỉnh cũng không phải là thung lũng.
Tại chỉ số 2: Các phần tử lân cận khác 5 gần nhất là 6 và 4. Vì 5 &lt; 6 và 5 &gt; 4, chỉ số 2 không phải là đỉnh cũng không phải là thung lũng.
Tại chỉ số 3: Các phần tử lân cận khác 5 gần nhất là 6 và 4. Vì 5 &lt; 6 và 5 &gt; 4, chỉ số 3 không phải là đỉnh cũng không phải là thung lũng.
Tại chỉ số 4: Các phần tử lân cận khác 4 gần nhất là 5 và 1. Vì 4 &lt; 5 và 4 &gt; 1, chỉ số 4 không phải là đỉnh cũng không phải là thung lũng.
Tại chỉ số 5: Không có phần tử lân cận khác 1 ở bên phải, nên chỉ số 5 không phải là đỉnh cũng không phải là thung lũng.
Có 0 đỉnh và thung lũng, nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Đỉnh và thung lũng được xác định dựa trên các phần tử lân cận gần nhất khác nhau, nên một dãy các giá trị bằng nhau tạo thành một đoạn phẳng. Với $n \le 100$, ta có thể quét về hai phía tại mỗi $i$, nhưng cách đó sẽ đọc lại cùng một đoạn phẳng nhiều lần.
>
> Một con trỏ $j$ ghi nhớ độ cao khác biệt gần nhất đã được xác định. Nếu $nums[i] = nums[i+1]$ thì ta vẫn đang ở trong một đoạn phẳng và bỏ qua; nếu không, ta so sánh $nums[i]$ với $nums[j]$ và $nums[i+1]$, sau đó gán $j = i$. Một lượt duyệt tuyến tính là đủ để đếm số đỉnh và thung lũng.

<!-- thinking:end -->

Ta khởi tạo một con trỏ $j$ trỏ đến vị trí có chỉ số $0$, sau đó duyệt mảng trong phạm vi $[1, n-1]$. Với mỗi vị trí $i$:

- Nếu $nums[i] = nums[i+1]$, bỏ qua.
- Ngược lại, nếu $nums[i]$ lớn hơn $nums[j]$ và $nums[i]$ lớn hơn $nums[i+1]$, thì $i$ là một đỉnh; nếu $nums[i]$ nhỏ hơn $nums[j]$ và $nums[i]$ nhỏ hơn $nums[i+1]$, thì $i$ là một thung lũng.
- Sau đó, cập nhật $j$ thành $i$ và tiếp tục duyệt.

Sau khi duyệt xong, ta có thể nhận được số đỉnh và thung lũng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countHillValley(self, nums: List[int]) -> int:
        ans = j = 0
        for i in range(1, len(nums) - 1):
            if nums[i] == nums[i + 1]:
                continue
            if nums[i] > nums[j] and nums[i] > nums[i + 1]:
                ans += 1
            if nums[i] < nums[j] and nums[i] < nums[i + 1]:
                ans += 1
            j = i
        return ans
```

#### Java

```java
class Solution {
    public int countHillValley(int[] nums) {
        int ans = 0;
        for (int i = 1, j = 0; i < nums.length - 1; ++i) {
            if (nums[i] == nums[i + 1]) {
                continue;
            }
            if (nums[i] > nums[j] && nums[i] > nums[i + 1]) {
                ++ans;
            }
            if (nums[i] < nums[j] && nums[i] < nums[i + 1]) {
                ++ans;
            }
            j = i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countHillValley(vector<int>& nums) {
        int ans = 0;
        for (int i = 1, j = 0; i < nums.size() - 1; ++i) {
            if (nums[i] == nums[i + 1]) {
                continue;
            }
            if (nums[i] > nums[j] && nums[i] > nums[i + 1]) {
                ++ans;
            }
            if (nums[i] < nums[j] && nums[i] < nums[i + 1]) {
                ++ans;
            }
            j = i;
        }
        return ans;
    }
};
```

#### Go

```go
func countHillValley(nums []int) int {
	ans := 0
	for i, j := 1, 0; i < len(nums)-1; i++ {
		if nums[i] == nums[i+1] {
			continue
		}
		if nums[i] > nums[j] && nums[i] > nums[i+1] {
			ans++
		}
		if nums[i] < nums[j] && nums[i] < nums[i+1] {
			ans++
		}
		j = i
	}
	return ans
}
```

#### TypeScript

```ts
function countHillValley(nums: number[]): number {
    let ans = 0;
    for (let i = 1, j = 0; i < nums.length - 1; ++i) {
        if (nums[i] === nums[i + 1]) {
            continue;
        }
        if (nums[i] > nums[j] && nums[i] > nums[i + 1]) {
            ans++;
        }
        if (nums[i] < nums[j] && nums[i] < nums[i + 1]) {
            ans++;
        }
        j = i;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_hill_valley(nums: Vec<i32>) -> i32 {
        let mut ans = 0;
        let mut j = 0;

        for i in 1..nums.len() - 1 {
            if nums[i] == nums[i + 1] {
                continue;
            }
            if nums[i] > nums[j] && nums[i] > nums[i + 1] {
                ans += 1;
            }
            if nums[i] < nums[j] && nums[i] < nums[i + 1] {
                ans += 1;
            }
            j = i;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
