---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Floyd Cycle Detection
---

<!-- problem:start -->

# [457. Circular Array Loop](https://leetcode.com/problems/circular-array-loop)

[中文文档](/solution/0400-0499/0457.Circular%20Array%20Loop/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi một trò chơi với mảng số nguyên khác 0 <code>nums</code> có dạng <strong>vòng tròn</strong>. Mỗi <code>nums[i]</code> cho biết số bước cần di chuyển tiến/lùi nếu đang ở chỉ số <code>i</code>:</p>

<ul>
	<li>Nếu <code>nums[i]</code> dương, di chuyển <code>nums[i]</code> bước về phía <strong>trước</strong>; còn</li>
	<li>Nếu <code>nums[i]</code> âm, di chuyển <code>abs(nums[i])</code> bước về phía <strong>sau</strong>.</li>
</ul>

<p>Vì mảng có dạng <strong>vòng tròn</strong>, khi đi tiếp từ phần tử cuối cùng, ta sẽ đến phần tử đầu tiên; khi đi lùi từ phần tử đầu tiên, ta sẽ đến phần tử cuối cùng.</p>

<p>Một <strong>cycle</strong> trong mảng là dãy chỉ số <code>seq</code> có độ dài <code>k</code> và thỏa mãn:</p>

<ul>
	<li>Làm theo quy tắc di chuyển ở trên sẽ tạo ra dãy chỉ số lặp lại <code>seq[0] -&gt; seq[1] -&gt; ... -&gt; seq[k - 1] -&gt; seq[0] -&gt; ...</code>.</li>
	<li>Mọi <code>nums[seq[j]]</code> đều <strong>dương</strong> hoặc đều <strong>âm</strong>.</li>
	<li><code>k &gt; 1</code>.</li>
</ul>

<p>Trả về <code>true</code><em> nếu mảng </em><code>nums</code><em> có </em><strong>cycle</strong><em>; nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0457.Circular%20Array%20Loop/images/img1.jpg" style="width: 402px; height: 289px;" />
<pre>
<strong>Đầu vào:</strong> nums = [2,-1,1,2,2]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Đồ thị minh họa cách các chỉ số nối với nhau. Các node màu trắng di chuyển về phía trước, còn node màu đỏ di chuyển về phía sau.
Ta thấy cycle 0 --&gt; 2 --&gt; 3 --&gt; 0 --&gt; ..., và tất cả node trong cycle đều màu trắng (di chuyển cùng một hướng).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0457.Circular%20Array%20Loop/images/img2.jpg" style="width: 402px; height: 390px;" />
<pre>
<strong>Đầu vào:</strong> nums = [-1,-2,-3,-4,-5,6]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Đồ thị minh họa cách các chỉ số nối với nhau. Các node màu trắng di chuyển về phía trước, còn node màu đỏ di chuyển về phía sau.
Cycle duy nhất có độ dài 1, nên ta trả về false.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0457.Circular%20Array%20Loop/images/img3.jpg" style="width: 497px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> nums = [1,-1,5,1,4]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Đồ thị minh họa cách các chỉ số nối với nhau. Các node màu trắng di chuyển về phía trước, còn node màu đỏ di chuyển về phía sau.
Ta thấy chu trình 0 --&gt; 1 --&gt; 0 --&gt; ... có độ dài &gt; 1, nhưng gồm một node di chuyển về phía trước và một node di chuyển về phía sau, nên <strong>đây không phải cycle hợp lệ</strong>.
Ta cũng thấy cycle 3 --&gt; 4 --&gt; 3 --&gt; ..., và tất cả node trong cycle đều màu trắng (di chuyển cùng một hướng).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5000</code></li>
	<li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>nums[i] != 0</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán với độ phức tạp thời gian <code>O(n)</code> và độ phức tạp không gian phụ <code>O(1)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các bước nhảy vòng quanh mảng; ta cần tìm cycle dài hơn $1$ mà mọi bước đều cùng hướng. Lưu toàn bộ đường đi sẽ tốn bộ nhớ, đồng thời phải loại self-loop và các lần đổi hướng.
>
> Từ mỗi chỉ số chưa xét, dùng hai con trỏ Floyd đuổi nhau khi tích các giá trị vẫn dương (cùng dấu). Nếu hai con trỏ gặp nhau tại vị trí không phải self-loop thì đã tìm thấy cycle. Đặt các phần tử trên đường đi thành 0 để không dùng lại chúng làm điểm bắt đầu.
>
> Đặt giá trị thành 0 để đánh dấu và tránh bắt đầu lại trên đường đi không có cycle; điều kiện $\textit{slow}\ne \textit{next}(\textit{slow})$ loại cycle độ dài 1.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def circularArrayLoop(self, nums: List[int]) -> bool:
        n = len(nums)

        def next(i):
            return (i + nums[i] % n + n) % n

        for i in range(n):
            if nums[i] == 0:
                continue
            slow, fast = i, next(i)
            while nums[slow] * nums[fast] > 0 and nums[slow] * nums[next(fast)] > 0:
                if slow == fast:
                    if slow != next(slow):
                        return True
                    break
                slow, fast = next(slow), next(next(fast))
            j = i
            while nums[j] * nums[next(j)] > 0:
                nums[j] = 0
                j = next(j)
        return False
```

#### Java

```java
class Solution {
    private int n;
    private int[] nums;

    public boolean circularArrayLoop(int[] nums) {
        n = nums.length;
        this.nums = nums;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == 0) {
                continue;
            }
            int slow = i, fast = next(i);
            while (nums[slow] * nums[fast] > 0 && nums[slow] * nums[next(fast)] > 0) {
                if (slow == fast) {
                    if (slow != next(slow)) {
                        return true;
                    }
                    break;
                }
                slow = next(slow);
                fast = next(next(fast));
            }
            int j = i;
            while (nums[j] * nums[next(j)] > 0) {
                nums[j] = 0;
                j = next(j);
            }
        }
        return false;
    }

    private int next(int i) {
        return (i + nums[i] % n + n) % n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool circularArrayLoop(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            if (!nums[i]) continue;
            int slow = i, fast = next(nums, i);
            while (nums[slow] * nums[fast] > 0 && nums[slow] * nums[next(nums, fast)] > 0) {
                if (slow == fast) {
                    if (slow != next(nums, slow)) return true;
                    break;
                }
                slow = next(nums, slow);
                fast = next(nums, next(nums, fast));
            }
            int j = i;
            while (nums[j] * nums[next(nums, j)] > 0) {
                nums[j] = 0;
                j = next(nums, j);
            }
        }
        return false;
    }

    int next(vector<int>& nums, int i) {
        int n = nums.size();
        return (i + nums[i] % n + n) % n;
    }
};
```

#### Go

```go
func circularArrayLoop(nums []int) bool {
	for i, num := range nums {
		if num == 0 {
			continue
		}
		slow, fast := i, next(nums, i)
		for nums[slow]*nums[fast] > 0 && nums[slow]*nums[next(nums, fast)] > 0 {
			if slow == fast {
				if slow != next(nums, slow) {
					return true
				}
				break
			}
			slow, fast = next(nums, slow), next(nums, next(nums, fast))
		}
		j := i
		for nums[j]*nums[next(nums, j)] > 0 {
			nums[j] = 0
			j = next(nums, j)
		}
	}
	return false
}

func next(nums []int, i int) int {
	n := len(nums)
	return (i + nums[i]%n + n) % n
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
