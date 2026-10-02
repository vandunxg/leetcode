---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Hash Table
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [659. Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences)

[中文文档](/solution/0600-0699/0659.Split%20Array%20into%20Consecutive%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> đã được <strong>sắp xếp theo thứ tự không giảm</strong>.</p>

<p>Hãy xác định có thể chia <code>nums</code> thành <strong>một hoặc nhiều dãy con</strong> sao cho <strong>đồng thời</strong> thỏa mãn hai điều kiện sau hay không:</p>

<ul>
	<li>Mỗi dãy con là một <strong>dãy số nguyên liên tiếp tăng dần</strong> (nghĩa là mỗi số nguyên <strong>lớn hơn đúng một đơn vị</strong> so với số đứng trước).</li>
	<li>Mỗi dãy con có độ dài <strong>ít nhất</strong> <code>3</code>.</li>
</ul>

<p>Trả về <code>true</code><em> nếu có thể chia </em><code>nums</code><em> theo các điều kiện trên, nếu không thì trả về </em><code>false</code><em>.</em></p>

<p><strong>Dãy con</strong> của một mảng là mảng mới được tạo từ mảng ban đầu bằng cách xóa một số phần tử (có thể không xóa phần tử nào) mà không làm thay đổi thứ tự tương đối của các phần tử còn lại. (Ví dụ, <code>[1,3,5]</code> là dãy con của <code>[<u>1</u>,2,<u>3</u>,4,<u>5</u>]</code>, còn <code>[1,3,2]</code> thì không).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,3,4,5]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể chia nums thành các dãy con sau:
[<strong><u>1</u></strong>,<strong><u>2</u></strong>,<strong><u>3</u></strong>,3,4,5] --&gt; 1, 2, 3
[1,2,3,<strong><u>3</u></strong>,<strong><u>4</u></strong>,<strong><u>5</u></strong>] --&gt; 3, 4, 5
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,3,4,4,5,5]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể chia nums thành các dãy con sau:
[<strong><u>1</u></strong>,<strong><u>2</u></strong>,<strong><u>3</u></strong>,3,<strong><u>4</u></strong>,4,<strong><u>5</u></strong>,5] --&gt; 1, 2, 3, 4, 5
[1,2,3,<strong><u>3</u></strong>,4,<strong><u>4</u></strong>,5,<strong><u>5</u></strong>] --&gt; 3, 4, 5
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,4,5]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể chia nums thành các dãy con liên tiếp tăng dần có độ dài từ 3 trở lên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>nums</code> được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chia mảng thành các dãy con liên tiếp tăng dần có độ dài ít nhất $3$. Nếu luôn bắt đầu dãy mới, có thể còn lại những dãy quá ngắn.
>
> Dùng hash map lưu min-heap độ dài các dãy kết thúc tại từng giá trị. Thêm $v$ vào dãy ngắn nhất đang kết thúc ở $v-1$; nếu không có dãy nào như vậy thì bắt đầu dãy mới. Giá trị nhỏ nhất trong mỗi heap phải ít nhất là $3$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPossible(self, nums: List[int]) -> bool:
        d = defaultdict(list)
        for v in nums:
            if h := d[v - 1]:
                heappush(d[v], heappop(h) + 1)
            else:
                heappush(d[v], 1)
        return all(not v or v and v[0] > 2 for v in d.values())
```

#### Java

```java
class Solution {
    public boolean isPossible(int[] nums) {
        Map<Integer, PriorityQueue<Integer>> d = new HashMap<>();
        for (int v : nums) {
            if (d.containsKey(v - 1)) {
                var q = d.get(v - 1);
                d.computeIfAbsent(v, k -> new PriorityQueue<>()).offer(q.poll() + 1);
                if (q.isEmpty()) {
                    d.remove(v - 1);
                }
            } else {
                d.computeIfAbsent(v, k -> new PriorityQueue<>()).offer(1);
            }
        }
        for (var v : d.values()) {
            if (v.peek() < 3) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPossible(vector<int>& nums) {
        unordered_map<int, priority_queue<int, vector<int>, greater<int>>> d;
        for (int v : nums) {
            if (d.count(v - 1)) {
                auto& q = d[v - 1];
                d[v].push(q.top() + 1);
                q.pop();
                if (q.empty()) {
                    d.erase(v - 1);
                }
            } else {
                d[v].push(1);
            }
        }
        for (auto& [_, v] : d) {
            if (v.top() < 3) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isPossible(nums []int) bool {
	d := map[int]*hp{}
	for _, v := range nums {
		if d[v] == nil {
			d[v] = new(hp)
		}
		if h := d[v-1]; h != nil {
			heap.Push(d[v], heap.Pop(h).(int)+1)
			if h.Len() == 0 {
				delete(d, v-1)
			}
		} else {
			heap.Push(d[v], 1)
		}
	}
	for _, q := range d {
		if q.IntSlice[0] < 3 {
			return false
		}
	}
	return true
}

type hp struct{ sort.IntSlice }

func (h *hp) Push(v any) { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
