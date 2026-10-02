---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Hash Table
    - Math
---

<!-- problem:start -->

# [781. Rabbits in Forest](https://leetcode.com/problems/rabbits-in-forest)

[中文文档](/solution/0700-0799/0781.Rabbits%20in%20Forest/README.md)

## Mô tả

<!-- description:start -->

<p>Có một khu rừng với số lượng thỏ chưa biết. Ta hỏi n con thỏ <strong>&quot;Có bao nhiêu con thỏ khác cùng màu với bạn?&quot;</strong> và lưu câu trả lời vào mảng số nguyên <code>answers</code>, trong đó <code>answers[i]</code> là câu trả lời của con thỏ thứ <code>i<sup>th</sup></code>.</p>

<p>Cho mảng <code>answers</code>, hãy trả về <em>số thỏ ít nhất có thể có trong khu rừng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> answers = [1,1,2]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Hai con thỏ trả lời &quot;1&quot; có thể cùng màu, chẳng hạn màu đỏ.
Con thỏ trả lời &quot;2&quot; không thể màu đỏ, nếu không các câu trả lời sẽ mâu thuẫn.
Giả sử con thỏ trả lời &quot;2&quot; có màu xanh dương.
Khi đó trong rừng phải có thêm 2 con thỏ màu xanh dương không xuất hiện trong mảng câu trả lời.
Vậy số thỏ ít nhất có thể có trong rừng là 5: gồm 3 con đã trả lời và 2 con chưa trả lời.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> answers = [10,10,10]
<strong>Đầu ra:</strong> 11
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= answers.length &lt;= 1000</code></li>
	<li><code>0 &lt;= answers[i] &lt; 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Hash Map

<!-- thinking:start -->

> **Tư duy**
>
> Câu trả lời $x$ có nghĩa là có $x+1$ con thỏ cùng màu đó. Các câu trả lời khác nhau không thể cùng màu; những con thỏ có cùng câu trả lời được gom thành các nhóm kích thước $x+1$.
>
> Để số thỏ ít nhất, chia $v$ câu trả lời $x$ thành $lceil v/(x+1)\rceil$ nhóm.
>
> Đếm số lần xuất hiện, rồi cộng $	extit{groups}\times(x+1)$ cho mỗi giá trị $x$.

<!-- thinking:end -->

Theo mô tả bài toán, các con thỏ có cùng câu trả lời có thể cùng màu, còn các con thỏ có câu trả lời khác nhau thì không thể cùng màu.

Vì vậy, ta dùng hash map $\textit{cnt}$ để ghi nhận số lần xuất hiện của mỗi câu trả lời. Với mỗi câu trả lời $x$ có $v$ lần xuất hiện, ta tính số thỏ tối thiểu dựa trên nguyên tắc mỗi màu có $x + 1$ con thỏ, rồi cộng vào đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{answers}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numRabbits(self, answers: List[int]) -> int:
        cnt = Counter(answers)
        ans = 0
        for x, v in cnt.items():
            group = x + 1
            ans += (v + group - 1) // group * group
        return ans
```

#### Java

```java
class Solution {
    public int numRabbits(int[] answers) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : answers) {
            cnt.merge(x, 1, Integer::sum);
        }
        int ans = 0;
        for (var e : cnt.entrySet()) {
            int group = e.getKey() + 1;
            ans += (e.getValue() + group - 1) / group * group;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numRabbits(vector<int>& answers) {
        unordered_map<int, int> cnt;
        for (int x : answers) {
            ++cnt[x];
        }
        int ans = 0;
        for (auto& [x, v] : cnt) {
            int group = x + 1;
            ans += (v + group - 1) / group * group;
        }
        return ans;
    }
};
```

#### Go

```go
func numRabbits(answers []int) (ans int) {
	cnt := map[int]int{}
	for _, x := range answers {
		cnt[x]++
	}
	for x, v := range cnt {
		group := x + 1
		ans += (v + group - 1) / group * group
	}
	return
}
```

#### TypeScript

```ts
function numRabbits(answers: number[]): number {
    const cnt = new Map<number, number>();
    for (const x of answers) {
        cnt.set(x, (cnt.get(x) || 0) + 1);
    }
    let ans = 0;
    for (const [x, v] of cnt.entries()) {
        const group = x + 1;
        ans += Math.floor((v + group - 1) / group) * group;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
