---
comments: true
difficulty: Medium
tags:
    - Queue
    - Array
    - Sorting
    - Simulation
---

<!-- problem:start -->

# [950. Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order)

[中文文档](/solution/0900-0999/0950.Reveal%20Cards%20In%20Increasing%20Order/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>deck</code>. Có một bộ bài, mỗi lá mang một số nguyên duy nhất. Số trên lá bài thứ <code>i<sup>th</sup></code> là <code>deck[i]</code>.</p>

<p>Bạn có thể xếp bộ bài theo thứ tự tùy ý. Ban đầu, tất cả lá bài đều úp trong cùng một bộ bài.</p>

<p>Lặp lại các bước sau cho đến khi lật hết tất cả lá bài:</p>

<ol>
	<li>Lấy lá trên cùng của bộ bài, lật ngửa rồi bỏ nó ra khỏi bộ bài.</li>
	<li>Nếu bộ bài vẫn còn lá, đưa lá trên cùng tiếp theo xuống dưới cùng.</li>
	<li>Nếu vẫn còn lá chưa lật, quay lại bước 1. Nếu không, dừng lại.</li>
</ol>

<p>Hãy trả về <em>thứ tự xếp bộ bài sao cho khi lật, các lá bài xuất hiện theo thứ tự tăng dần</em>.</p>

<p><strong>Lưu ý</strong>, phần tử đầu tiên trong đáp án được xem là lá trên cùng của bộ bài.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> deck = [17,13,11,2,3,5,7]
<strong>Đầu ra:</strong> [2,13,3,11,5,17,7]
<strong>Giải thích:</strong> 
Ta nhận được bộ bài theo thứ tự [17,13,11,2,3,5,7] (thứ tự ban đầu không quan trọng) rồi xếp lại.
Sau khi xếp, bộ bài là [2,13,3,11,5,17,7], trong đó 2 ở trên cùng.
Lật 2 và đưa 13 xuống dưới cùng. Bộ bài lúc này là [3,11,5,17,7,13].
Lật 3 và đưa 11 xuống dưới cùng. Bộ bài lúc này là [5,17,7,13,11].
Lật 5 và đưa 17 xuống dưới cùng. Bộ bài lúc này là [7,13,11,17].
Lật 7 và đưa 13 xuống dưới cùng. Bộ bài lúc này là [11,17,13].
Lật 11 và đưa 17 xuống dưới cùng. Bộ bài lúc này là [13,17].
Lật 13 và đưa 17 xuống dưới cùng. Bộ bài lúc này là [17].
Lật 17.
Vì các lá bài được lật theo thứ tự tăng dần nên đây là đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> deck = [1,1000]
<strong>Đầu ra:</strong> [1,1000]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= deck.length &lt;= 1000</code></li>
	<li><code>1 &lt;= deck[i] &lt;= 10<sup>6</sup></code></li>
	<li>Tất cả giá trị trong <code>deck</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khi lật bài, ta lấy lá đầu tiên rồi chuyển lá đầu mới xuống cuối. Mô phỏng xuôi sẽ cần biết trước thứ tự ban đầu, trong khi thứ tự lật cần tăng dần. Hãy đảo ngược quy trình: đi từ lá lớn đến lá nhỏ, xoay lá cuối hiện tại lên đầu (nếu queue không rỗng), rồi thêm lá đang xét vào đầu. Queue thu được chính là thứ tự bộ bài ban đầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def deckRevealedIncreasing(self, deck: List[int]) -> List[int]:
        q = deque()
        for v in sorted(deck, reverse=True):
            if q:
                q.appendleft(q.pop())
            q.appendleft(v)
        return list(q)
```

#### Java

```java
class Solution {
    public int[] deckRevealedIncreasing(int[] deck) {
        Deque<Integer> q = new ArrayDeque<>();
        Arrays.sort(deck);
        int n = deck.length;
        for (int i = n - 1; i >= 0; --i) {
            if (!q.isEmpty()) {
                q.offerFirst(q.pollLast());
            }
            q.offerFirst(deck[i]);
        }
        int[] ans = new int[n];
        for (int i = n - 1; i >= 0; --i) {
            ans[i] = q.pollLast();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> deckRevealedIncreasing(vector<int>& deck) {
        sort(deck.rbegin(), deck.rend());
        deque<int> q;
        for (int v : deck) {
            if (!q.empty()) {
                q.push_front(q.back());
                q.pop_back();
            }
            q.push_front(v);
        }
        return vector<int>(q.begin(), q.end());
    }
};
```

#### Go

```go
func deckRevealedIncreasing(deck []int) []int {
	sort.Sort(sort.Reverse(sort.IntSlice(deck)))
	q := []int{}
	for _, v := range deck {
		if len(q) > 0 {
			q = append([]int{q[len(q)-1]}, q[:len(q)-1]...)
		}
		q = append([]int{v}, q...)
	}
	return q
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
