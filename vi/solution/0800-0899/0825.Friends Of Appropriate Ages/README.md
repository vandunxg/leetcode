---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [825. Friends Of Appropriate Ages](https://leetcode.com/problems/friends-of-appropriate-ages)

[中文文档](/solution/0800-0899/0825.Friends%20Of%20Appropriate%20Ages/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> người dùng trên một mạng xã hội. Bạn được cho mảng số nguyên <code>ages</code>, trong đó <code>ages[i]</code> là tuổi của người thứ <code>i<sup>th</sup></code>.</p>

<p>Người <code>x</code> sẽ không gửi lời mời kết bạn cho người <code>y</code> (<code>x != y</code>) nếu thỏa mãn bất kỳ điều kiện nào sau đây:</p>

<ul>
	<li><code>age[y] &lt;= 0.5 * age[x] + 7</code></li>
	<li><code>age[y] &gt; age[x]</code></li>
	<li><code>age[y] &gt; 100 &amp;&amp; age[x] &lt; 100</code></li>
</ul>

<p>Nếu không, <code>x</code> sẽ gửi lời mời kết bạn cho <code>y</code>.</p>

<p>Lưu ý, nếu <code>x</code> gửi lời mời cho <code>y</code>, không nhất thiết <code>y</code> cũng gửi lời mời cho <code>x</code>. Ngoài ra, một người sẽ không gửi lời mời kết bạn cho chính mình.</p>

<p>Hãy trả về <em>tổng số lời mời kết bạn đã gửi</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> ages = [16,16]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hai người gửi lời mời kết bạn cho nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ages = [16,17,18]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các lời mời kết bạn được gửi như sau: 17 -&gt; 16, 18 -&gt; 17.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> ages = [20,30,100,110,120]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các lời mời kết bạn được gửi như sau: 110 -&gt; 100, 120 -&gt; 110, 120 -&gt; 100.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == ages.length</code></li>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= ages[i] &lt;= 120</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Việc $x$ có thể gửi lời mời cho $y$ chỉ phụ thuộc vào hai độ tuổi, nằm trong khoảng $1\ldots 120$, còn $n$ có thể lên tới $2\cdot 10^4$. Ghép từng người với nhau sẽ lặp lại các cặp tuổi giống nhau.
>
> Đếm số người ở mỗi độ tuổi, sau đó liệt kê các cặp tuổi và kiểm tra ba bất đẳng thức. Khi tính lời mời giữa những người cùng tuổi, cần loại trường hợp tự gửi cho mình; do đó tích là $x\cdot(y-[x=y])$.

<!-- thinking:end -->

Ta có thể dùng mảng $\textit{cnt}$ có độ dài $121$ để ghi nhận số người ở mỗi độ tuổi.

Tiếp theo, ta liệt kê mọi cặp tuổi $(\textit{ax}, \textit{ay})$. Nếu $\textit{ax}$ và $\textit{ay}$ thỏa mãn các điều kiện của đề bài, người có tuổi $\textit{ax}$ có thể gửi lời mời kết bạn cho người có tuổi $\textit{ay}$.

Nếu $\textit{ax} = \textit{ay}$, tức là hai độ tuổi giống nhau, số lời mời kết bạn giữa những người ở độ tuổi này là $\textit{cnt}[\textit{ax}] \times (\textit{cnt}[\textit{ax}] - 1)$. Nếu hai độ tuổi khác nhau, số lời mời kết bạn là $\textit{cnt}[\textit{ax}] \times \textit{cnt}[\textit{ay}]$. Ta cộng các số lượng này vào đáp án.

Độ phức tạp thời gian là $O(n + m^2)$, trong đó $n$ là độ dài của mảng $\textit{ages}$ và $m$ là tuổi tối đa, bằng $121$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numFriendRequests(self, ages: List[int]) -> int:
        cnt = [0] * 121
        for x in ages:
            cnt[x] += 1
        ans = 0
        for ax, x in enumerate(cnt):
            for ay, y in enumerate(cnt):
                if not (ay <= 0.5 * ax + 7 or ay > ax or (ay > 100 and ax < 100)):
                    ans += x * (y - int(ax == ay))
        return ans
```

#### Java

```java
class Solution {
    public int numFriendRequests(int[] ages) {
        final int m = 121;
        int[] cnt = new int[m];
        for (int x : ages) {
            ++cnt[x];
        }
        int ans = 0;
        for (int ax = 1; ax < m; ++ax) {
            for (int ay = 1; ay < m; ++ay) {
                if (!(ay <= 0.5 * ax + 7 || ay > ax || (ay > 100 && ax < 100))) {
                    ans += cnt[ax] * (cnt[ay] - (ax == ay ? 1 : 0));
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numFriendRequests(vector<int>& ages) {
        const int m = 121;
        vector<int> cnt(m);
        for (int x : ages) {
            ++cnt[x];
        }
        int ans = 0;
        for (int ax = 1; ax < m; ++ax) {
            for (int ay = 1; ay < m; ++ay) {
                if (!(ay <= 0.5 * ax + 7 || ay > ax || (ay > 100 && ax < 100))) {
                    ans += cnt[ax] * (cnt[ay] - (ax == ay ? 1 : 0));
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numFriendRequests(ages []int) (ans int) {
	cnt := [121]int{}
	for _, x := range ages {
		cnt[x]++
	}
	for ax, x := range cnt {
		for ay, y := range cnt {
			if ay <= ax/2+7 || ay > ax || (ay > 100 && ax < 100) {
				continue
			}
			if ax == ay {
				ans += x * (x - 1)
			} else {
				ans += x * y
			}
		}
	}

	return
}
```

#### TypeScript

```ts
function numFriendRequests(ages: number[]): number {
    const m = 121;
    const cnt = Array(m).fill(0);
    for (const x of ages) {
        cnt[x]++;
    }

    let ans = 0;
    for (let ax = 0; ax < m; ax++) {
        for (let ay = 0; ay < m; ay++) {
            if (ay <= 0.5 * ax + 7 || ay > ax || (ay > 100 && ax < 100)) {
                continue;
            }
            ans += cnt[ax] * (cnt[ay] - (ax === ay ? 1 : 0));
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
