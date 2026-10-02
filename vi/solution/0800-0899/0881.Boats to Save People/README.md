---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Two Pointers
    - Sorting
    - Timsort
---

<!-- problem:start -->

# [881. Boats to Save People](https://leetcode.com/problems/boats-to-save-people)

[中文文档](/solution/0800-0899/0881.Boats%20to%20Save%20People/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>people</code>, trong đó <code>people[i]</code> là cân nặng của người thứ <code>i<sup>th</sup></code>, cùng với <strong>số lượng thuyền không giới hạn</strong>; mỗi thuyền chở được tổng cân nặng tối đa là <code>limit</code>. Mỗi thuyền chở tối đa hai người cùng lúc, với điều kiện tổng cân nặng của họ không vượt quá <code>limit</code>.</p>

<p>Hãy trả về <em>số thuyền ít nhất cần dùng để chở tất cả mọi người</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> people = [1,2], limit = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 1 thuyền (1, 2)
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> people = [3,2,2,1], limit = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 3 thuyền (1, 2), (2) và (3)
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> people = [3,5,3,4], limit = 5
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 4 thuyền (3), (3), (4), (5)
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= people.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= people[i] &lt;= limit &lt;= 3 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thuyền chở tối đa hai người có tổng cân nặng không vượt quá $\textit{limit}$. Vì $n\le 5\cdot 10^4$, hãy ghép người nặng nhất với người nhẹ nhất nếu có thể.
>
> Sắp xếp rồi dùng hai con trỏ: nếu hai đầu ghép vừa thì cả hai cùng lên thuyền; nếu không, người nặng hơn đi một mình. Mỗi bước dùng một thuyền cho đến khi hai con trỏ vượt nhau.

<!-- thinking:end -->

Sau khi sắp xếp, dùng hai con trỏ lần lượt trỏ đến đầu và cuối mảng. Mỗi lần, so sánh tổng hai phần tử được trỏ tới với `limit`. Nếu tổng không vượt quá `limit`, cả hai con trỏ cùng tiến một bước về giữa. Nếu không, chỉ con trỏ phải dịch chuyển. Cộng số thuyền đã dùng vào đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài mảng `people`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numRescueBoats(self, people: List[int], limit: int) -> int:
        people.sort()
        ans = 0
        i, j = 0, len(people) - 1
        while i <= j:
            if people[i] + people[j] <= limit:
                i += 1
            j -= 1
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);
        int ans = 0;
        for (int i = 0, j = people.length - 1; i <= j; --j) {
            if (people[i] + people[j] <= limit) {
                ++i;
            }
            ++ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {
        sort(people.begin(), people.end());
        int ans = 0;
        for (int i = 0, j = people.size() - 1; i <= j; --j) {
            if (people[i] + people[j] <= limit) {
                ++i;
            }
            ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func numRescueBoats(people []int, limit int) int {
	sort.Ints(people)
	ans := 0
	for i, j := 0, len(people)-1; i <= j; j-- {
		if people[i]+people[j] <= limit {
			i++
		}
		ans++
	}
	return ans
}
```

#### TypeScript

```ts
function numRescueBoats(people: number[], limit: number): number {
    people.sort((a, b) => a - b);
    let ans = 0;
    for (let i = 0, j = people.length - 1; i <= j; --j) {
        if (people[i] + people[j] <= limit) {
            ++i;
        }
        ++ans;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
