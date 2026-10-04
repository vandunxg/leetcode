---
comments: true
difficulty: Hard
rating: 2046
source: Biweekly Contest 128 Q4
tags:
    - Stack
    - Array
    - Binary Search
    - Monotonic Stack
---

<!-- problem:start -->

# [3113. Find the Number of Subarrays Where Boundary Elements Are Maximum](https://leetcode.com/problems/find-the-number-of-subarrays-where-boundary-elements-are-maximum)

[中文文档](/solution/3100-3199/3113.Find%20the%20Number%20of%20Subarrays%20Where%20Boundary%20Elements%20Are%20Maximum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>dương</strong> <code>nums</code>.</p>

<p>Hãy trả về số <span data-keyword="subarray-nonempty">mảng con</span> của <code>nums</code> mà phần tử <strong>đầu tiên</strong> và <strong>cuối cùng</strong> của mảng con <em>bằng nhau</em> và đều bằng phần tử <strong>lớn nhất</strong> trong mảng con.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,3,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 6 mảng con mà phần tử đầu tiên và cuối cùng đều bằng phần tử lớn nhất trong mảng con:</p>

<ul>
	<li>mảng con <code>[<strong><u>1</u></strong>,4,3,3,2]</code>, với phần tử lớn nhất là 1. Phần tử đầu tiên là 1 và phần tử cuối cùng cũng là 1.</li>
	<li>mảng con <code>[1,<u><strong>4</strong></u>,3,3,2]</code>, với phần tử lớn nhất là 4. Phần tử đầu tiên là 4 và phần tử cuối cùng cũng là 4.</li>
	<li>mảng con <code>[1,4,<u><strong>3</strong></u>,3,2]</code>, với phần tử lớn nhất là 3. Phần tử đầu tiên là 3 và phần tử cuối cùng cũng là 3.</li>
	<li>mảng con <code>[1,4,3,<u><strong>3</strong></u>,2]</code>, với phần tử lớn nhất là 3. Phần tử đầu tiên là 3 và phần tử cuối cùng cũng là 3.</li>
	<li>mảng con <code>[1,4,3,3,<u><strong>2</strong></u>]</code>, với phần tử lớn nhất là 2. Phần tử đầu tiên là 2 và phần tử cuối cùng cũng là 2.</li>
	<li>mảng con <code>[1,4,<u><strong>3,3</strong></u>,2]</code>, với phần tử lớn nhất là 3. Phần tử đầu tiên là 3 và phần tử cuối cùng cũng là 3.</li>
</ul>

<p>Do đó, ta trả về 6.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 6 mảng con mà phần tử đầu tiên và cuối cùng đều bằng phần tử lớn nhất trong mảng con:</p>

<ul>
	<li>mảng con <code>[<u><strong>3</strong></u>,3,3]</code>, với phần tử lớn nhất là 3. Phần tử đầu tiên là 3 và phần tử cuối cùng cũng là 3.</li>
	<li>mảng con <code>[3,<strong><u>3</u></strong>,3]</code>, với phần tử lớn nhất là 3. Phần tử đầu tiên là 3 và phần tử cuối cùng cũng là 3.</li>
	<li>mảng con <code>[3,3,<u><strong>3</strong></u>]</code>, với phần tử lớn nhất là 3. Phần tử đầu tiên là 3 và phần tử cuối cùng cũng là 3.</li>
	<li>mảng con <code>[<strong><u>3,3</u></strong>,3]</code>, với phần tử lớn nhất là 3. Phần tử đầu tiên là 3 và phần tử cuối cùng cũng là 3.</li>
	<li>mảng con <code>[3,<u><strong>3,3</strong></u>]</code>, với phần tử lớn nhất là 3. Phần tử đầu tiên là 3 và phần tử cuối cùng cũng là 3.</li>
	<li>mảng con <code>[<u><strong>3,3,3</strong></u>]</code>, với phần tử lớn nhất là 3. Phần tử đầu tiên là 3 và phần tử cuối cùng cũng là 3.</li>
</ul>

<p>Do đó, ta trả về 6.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có một mảng con duy nhất của <code>nums</code> là <code>[<strong><u>1</u></strong>]</code>, với phần tử lớn nhất là 1. Phần tử đầu tiên là 1 và phần tử cuối cùng cũng là 1.</p>

<p>Do đó, ta trả về 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con hợp lệ có hai đầu mút bằng nhau và đồng thời là giá trị lớn nhất. Nếu kiểm tra giá trị lớn nhất ở giữa với mọi cặp đầu mút thì độ phức tạp là $O(n^2)$, quá chậm khi $n$ lớn.
>
> Khi hai đầu mút đều bằng $x$, không được xuất hiện phần tử nào lớn hơn ở giữa. Giá trị lớn hơn gần nhất ở bên trái sẽ cắt mọi ứng viên dài hơn kết thúc tại $x$ hiện tại.
>
> Ta duy trì một stack giảm dần gồm các giá trị và số lần chúng có thể mở rộng. Sau khi loại bỏ các nhóm nhỏ hơn, ta tăng số lượng ở đỉnh nếu đỉnh bằng $x$, hoặc bắt đầu một nhóm mới. Cộng số lượng ở đỉnh sau mỗi bước sẽ liệt kê mọi mảng con hợp lệ kết thúc tại vị trí hiện tại.

<!-- thinking:end -->

Ta xét mỗi phần tử $x$ trong mảng $nums$ làm phần tử ở hai đầu và giá trị lớn nhất của mảng con.

Mỗi mảng con có độ dài $1$ đều thỏa mãn điều kiện. Với các mảng con có độ dài lớn hơn $1$, mọi phần tử trong mảng con không được lớn hơn phần tử ở hai đầu $x$. Ta có thể cài đặt điều này bằng một stack đơn điệu.

Ta duy trì một stack đơn điệu giảm dần từ đáy lên đỉnh. Mỗi phần tử trong stack đơn điệu là một cặp $[x, cnt]$, biểu diễn phần tử $x$ và số lượng $cnt$ mảng con có $x$ là phần tử ở hai đầu và là giá trị lớn nhất.

Ta duyệt mảng $nums$ từ trái sang phải. Với mỗi phần tử $x$, ta liên tục lấy phần tử ở đỉnh stack ra cho đến khi stack rỗng hoặc phần tử đầu tiên của phần tử ở đỉnh stack lớn hơn hoặc bằng $x$. Nếu stack rỗng hoặc phần tử đầu tiên của phần tử ở đỉnh stack lớn hơn $x$, điều đó có nghĩa là ta đã gặp mảng con đầu tiên có $x$ là phần tử ở hai đầu và là giá trị lớn nhất. Độ dài của mảng con này là $1$, nên ta đẩy $[x, 1]$ vào stack. Nếu phần tử đầu tiên của phần tử ở đỉnh stack bằng $x$, điều đó có nghĩa là ta đã gặp một mảng con có $x$ là phần tử ở hai đầu và là giá trị lớn nhất, nên tăng phần tử thứ hai của phần tử ở đỉnh stack lên $1$. Sau đó, ta cộng phần tử thứ hai của phần tử ở đỉnh stack vào đáp án.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSubarrays(self, nums: List[int]) -> int:
        stk = []
        ans = 0
        for x in nums:
            while stk and stk[-1][0] < x:
                stk.pop()
            if not stk or stk[-1][0] > x:
                stk.append([x, 1])
            else:
                stk[-1][1] += 1
            ans += stk[-1][1]
        return ans
```

#### Java

```java
class Solution {
    public long numberOfSubarrays(int[] nums) {
        Deque<int[]> stk = new ArrayDeque<>();
        long ans = 0;
        for (int x : nums) {
            while (!stk.isEmpty() && stk.peek()[0] < x) {
                stk.pop();
            }
            if (stk.isEmpty() || stk.peek()[0] > x) {
                stk.push(new int[] {x, 1});
            } else {
                stk.peek()[1]++;
            }
            ans += stk.peek()[1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long numberOfSubarrays(vector<int>& nums) {
        vector<pair<int, int>> stk;
        long long ans = 0;
        for (int x : nums) {
            while (!stk.empty() && stk.back().first < x) {
                stk.pop_back();
            }
            if (stk.empty() || stk.back().first > x) {
                stk.push_back(make_pair(x, 1));
            } else {
                stk.back().second++;
            }
            ans += stk.back().second;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSubarrays(nums []int) (ans int64) {
	stk := [][2]int{}
	for _, x := range nums {
		for len(stk) > 0 && stk[len(stk)-1][0] < x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) == 0 || stk[len(stk)-1][0] > x {
			stk = append(stk, [2]int{x, 1})
		} else {
			stk[len(stk)-1][1]++
		}
		ans += int64(stk[len(stk)-1][1])
	}
	return
}
```

#### TypeScript

```ts
function numberOfSubarrays(nums: number[]): number {
    const stk: number[][] = [];
    let ans = 0;
    for (const x of nums) {
        while (stk.length > 0 && stk.at(-1)![0] < x) {
            stk.pop();
        }
        if (stk.length === 0 || stk.at(-1)![0] > x) {
            stk.push([x, 1]);
        } else {
            stk.at(-1)![1]++;
        }
        ans += stk.at(-1)![1];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
