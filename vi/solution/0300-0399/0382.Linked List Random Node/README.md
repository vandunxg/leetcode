---
comments: true
difficulty: Medium
tags:
    - Reservoir Sampling
    - Linked List
    - Math
    - Randomized
---

<!-- problem:start -->

# [382. Linked List Random Node](https://leetcode.com/problems/linked-list-random-node)

[中文文档](/solution/0300-0399/0382.Linked%20List%20Random%20Node/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một danh sách liên kết đơn, hãy trả về giá trị của một node được chọn ngẫu nhiên trong danh sách. Mỗi node phải có <strong>xác suất được chọn như nhau</strong>.</p>

<p>Hãy triển khai class <code>Solution</code>:</p>

<ul>
	<li><code>Solution(ListNode head)</code> Khởi tạo object với head <code>head</code> của danh sách liên kết đơn.</li>
	<li><code>int getRandom()</code> Chọn ngẫu nhiên một node trong danh sách và trả về giá trị của node đó. Mọi node trong danh sách đều phải có xác suất được chọn như nhau.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0382.Linked%20List%20Random%20Node/images/getrand-linked-list.jpg" style="width: 302px; height: 62px;" />
<pre>
<strong>Đầu vào</strong>
[&quot;Solution&quot;, &quot;getRandom&quot;, &quot;getRandom&quot;, &quot;getRandom&quot;, &quot;getRandom&quot;, &quot;getRandom&quot;]
[[[1, 2, 3]], [], [], [], [], []]
<strong>Đầu ra</strong>
[null, 1, 3, 2, 2, 3]

<strong>Giải thích</strong>
Solution solution = new Solution([1, 2, 3]);
solution.getRandom(); // return 1
solution.getRandom(); // return 3
solution.getRandom(); // return 2
solution.getRandom(); // return 2
solution.getRandom(); // return 3
// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Số node trong danh sách liên kết nằm trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>4</sup> &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
	<li><code>getRandom</code> được gọi nhiều nhất <code>10<sup>4</sup></code> lần.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Nếu danh sách liên kết cực lớn và bạn không biết trước độ dài của nó thì sao?</li>
	<li>Bạn có thể giải bài toán hiệu quả mà không dùng thêm bộ nhớ không?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần trả về một node với xác suất đồng đều trong danh sách có độ dài chưa biết. Đếm số node rồi chọn theo chỉ số cần duyệt hai lượt; reservoir sampling chỉ cần một lượt.
>
> Khi đến node thứ $n$, thay đáp án hiện tại bằng node đó với xác suất $1/n$. Nhờ vậy, cuối cùng mỗi vị trí đều có xác suất $1/n$ được chọn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def __init__(self, head: Optional[ListNode]):
        self.head = head

    def getRandom(self) -> int:
        n = ans = 0
        head = self.head
        while head:
            n += 1
            x = random.randint(1, n)
            if n == x:
                ans = head.val
            head = head.next
        return ans


# Your Solution object will be instantiated and called as such:
# obj = Solution(head)
# param_1 = obj.getRandom()
```

#### Java

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    private ListNode head;
    private Random random = new Random();

    public Solution(ListNode head) {
        this.head = head;
    }

    public int getRandom() {
        int ans = 0, n = 0;
        for (ListNode node = head; node != null; node = node.next) {
            ++n;
            int x = 1 + random.nextInt(n);
            if (n == x) {
                ans = node.val;
            }
        }
        return ans;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(head);
 * int param_1 = obj.getRandom();
 */
```

#### C++

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* head;

    Solution(ListNode* head) {
        this->head = head;
    }

    int getRandom() {
        int n = 0, ans = 0;
        for (ListNode* node = head; node != nullptr; node = node->next) {
            n += 1;
            int x = 1 + rand() % n;
            if (n == x) ans = node->val;
        }
        return ans;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(head);
 * int param_1 = obj->getRandom();
 */
```

#### Go

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
type Solution struct {
	head *ListNode
}

func Constructor(head *ListNode) Solution {
	return Solution{head}
}

func (this *Solution) GetRandom() int {
	n, ans := 0, 0
	for node := this.head; node != nil; node = node.Next {
		n++
		x := 1 + rand.Intn(n)
		if n == x {
			ans = node.Val
		}
	}
	return ans
}

/**
 * Your Solution object will be instantiated and called as such:
 * obj := Constructor(head);
 * param_1 := obj.GetRandom();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
