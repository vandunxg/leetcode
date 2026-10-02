---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - Math
    - Counting
    - Greatest Common Divisor
    - Number Theory
    - Euclidean Algorithm
---

<!-- problem:start -->

# [914. X of a Kind in a Deck of Cards](https://leetcode.com/problems/x-of-a-kind-in-a-deck-of-cards)

[中文文档](/solution/0900-0999/0914.X%20of%20a%20Kind%20in%20a%20Deck%20of%20Cards/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>deck</code>, trong đó <code>deck[i]</code> là số ghi trên lá bài thứ <code>i<sup>th</sup></code>.</p>

<p>Chia các lá bài thành <strong>một hoặc nhiều nhóm</strong> sao cho:</p>

<ul>
	<li>Mỗi nhóm có <strong>đúng</strong> <code>x</code> lá bài, với <code>x &gt; 1</code>, và</li>
	<li>Tất cả lá bài trong cùng một nhóm đều ghi cùng một số nguyên.</li>
</ul>

<p>Trả về <code>true</code><em> nếu có thể chia bài như vậy, nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> deck = [1,2,3,4,4,3,2,1]
<strong>Đầu ra:</strong> true
<strong>Giải thích</strong>: Có thể chia thành các nhóm [1,1],[2,2],[3,3],[4,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> deck = [1,1,1,2,2,2,3,3]
<strong>Đầu ra:</strong> false
<strong>Giải thích</strong>: Không có cách chia phù hợp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= deck.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= deck[i] &lt; 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ước chung lớn nhất

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần chia bài thành các nhóm có cùng kích thước $X\ge 2$, mỗi nhóm chỉ chứa một giá trị. $X$ phải chia hết tần suất xuất hiện của mọi giá trị, tức là ước chung của các tần suất đó. Tính $\gcd$ của chúng rồi kiểm tra giá trị này có ít nhất bằng $2$ hay không.

<!-- thinking:end -->

Trước tiên, ta dùng mảng hoặc hash table `cnt` để đếm số lần xuất hiện của mỗi giá trị. Điều kiện đề bài chỉ thỏa mãn khi $X$ là ước của ước chung lớn nhất trong các giá trị `cnt[i]`.

Vì vậy, ta tìm ước chung lớn nhất $g$ của số lần xuất hiện của tất cả các giá trị, rồi kiểm tra xem $g$ có lớn hơn hoặc bằng $2$ hay không.

Độ phức tạp thời gian là $O(n \times \log M)$ và độ phức tạp không gian là $O(n + \log M)$, trong đó $n$ là độ dài mảng `deck` và $M$ là giá trị lớn nhất trong mảng `deck`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasGroupsSizeX(self, deck: List[int]) -> bool:
        cnt = Counter(deck)
        return reduce(gcd, cnt.values()) >= 2
```

#### Java

```java
class Solution {
    public boolean hasGroupsSizeX(int[] deck) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : deck) {
            cnt.merge(x, 1, Integer::sum);
        }
        int g = cnt.get(deck[0]);
        for (int x : cnt.values()) {
            g = gcd(g, x);
        }
        return g >= 2;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool hasGroupsSizeX(vector<int>& deck) {
        unordered_map<int, int> cnt;
        for (int x : deck) {
            ++cnt[x];
        }
        int g = cnt[deck[0]];
        for (auto& [_, x] : cnt) {
            g = gcd(g, x);
        }
        return g >= 2;
    }
};
```

#### Go

```go
func hasGroupsSizeX(deck []int) bool {
	cnt := map[int]int{}
	for _, x := range deck {
		cnt[x]++
	}
	g := cnt[deck[0]]
	for _, x := range cnt {
		g = gcd(g, x)
	}
	return g >= 2
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function hasGroupsSizeX(deck: number[]): boolean {
    const cnt: Record<number, number> = {};
    for (const x of deck) {
        cnt[x] = (cnt[x] || 0) + 1;
    }
    const gcd = (a: number, b: number): number => (b === 0 ? a : gcd(b, a % b));
    let g = cnt[deck[0]];
    for (const [_, x] of Object.entries(cnt)) {
        g = gcd(g, x);
    }
    return g >= 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
