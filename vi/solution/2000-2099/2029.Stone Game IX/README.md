---
comments: true
difficulty: Medium
rating: 2277
source: Weekly Contest 261 Q3
tags:
    - Greedy
    - Minimax
    - Array
    - Math
    - Counting
    - Game Theory
    - Nim Game
    - Zero-Sum Game
---

<!-- problem:start -->

# [2029. Stone Game IX](https://leetcode.com/problems/stone-game-ix)

[中文文档](/solution/2000-2099/2029.Stone%20Game%20IX/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob tiếp tục chơi trò chơi với những viên đá. Có một hàng gồm n viên đá, mỗi viên có một giá trị đi kèm. Cho mảng số nguyên <code>stones</code>, trong đó <code>stones[i]</code> là <strong>giá trị</strong> của viên đá thứ <code>i<sup>th</sup></code>.</p>

<p>Alice và Bob luân phiên thực hiện lượt chơi, với <strong>Alice</strong> đi trước. Ở mỗi lượt, người chơi có thể lấy đi bất kỳ viên đá nào trong <code>stones</code>. Người chơi lấy đi một viên đá sẽ <strong>thua</strong> nếu <strong>tổng</strong> giá trị của <strong>tất cả các viên đá đã lấy</strong> chia hết cho <code>3</code>. Bob sẽ tự động thắng nếu không còn viên đá nào (ngay cả khi đến lượt Alice).</p>

<p>Giả sử cả hai người chơi đều chơi <strong>tối ưu</strong>, hãy trả về <code>true</code> <em>nếu Alice thắng và</em> <code>false</code> <em>nếu Bob thắng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [2,1]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>&nbsp;Trò chơi diễn ra như sau:
- Lượt 1: Alice có thể lấy một trong hai viên đá.
- Lượt 2: Bob lấy viên đá còn lại.
Tổng giá trị các viên đá đã lấy là 1 + 2 = 3 và chia hết cho 3. Vì vậy, Bob thua và Alice thắng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [2]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>&nbsp;Alice sẽ lấy viên đá duy nhất, tổng giá trị các viên đá đã lấy là 2.
Vì tất cả viên đá đã bị lấy và tổng giá trị không chia hết cho 3, Bob thắng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [5,1,2,4,3]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>&nbsp;Bob luôn thắng. Một cách để Bob thắng được mô tả dưới đây:
- Lượt 1: Alice có thể lấy viên đá thứ hai có giá trị 1. Tổng các giá trị đã lấy = 1.
- Lượt 2: Bob lấy viên đá thứ năm có giá trị 3. Tổng các giá trị đã lấy = 1 + 3 = 4.
- Lượt 3: Alice lấy viên đá thứ tư có giá trị 4. Tổng các giá trị đã lấy = 1 + 3 + 4 = 8.
- Lượt 4: Bob lấy viên đá thứ ba có giá trị 2. Tổng các giá trị đã lấy = 1 + 3 + 4 + 2 = 10.
- Lượt 5: Alice lấy viên đá đầu tiên có giá trị 5. Tổng các giá trị đã lấy = 1 + 3 + 4 + 2 + 5 = 15.
Alice thua vì tổng các giá trị đã lấy (15) chia hết cho 3. Bob thắng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= stones.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= stones[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ các phần dư modulo $3$ mới quan trọng; $n \le 10^5$ nên không thể mô phỏng toàn bộ trò chơi. Phần dư $0$ không làm thay đổi tổng hiện tại và có thể được xen vào; Alice không thể bắt đầu bằng $0$.
>
> Bắt đầu bằng $1$ cho chuỗi tối ưu $1,1,2,1,2,\ldots$ (trường hợp bắt đầu bằng $2$ là đối xứng). Sau khi tính cả các phần tử bằng 0, Alice thắng khi độ dài là số lẻ và hai nhóm phần tử khác 0 không đồng thời hết theo cùng cách.
>
> Kiểm tra $cnt$ và phiên bản hoán đổi $(cnt[0],cnt[2],cnt[1])$; chỉ cần một trong hai cách bắt đầu thắng.

<!-- thinking:end -->

Vì mục tiêu của người chơi là đảm bảo tổng giá trị các viên đá đã lấy không chia hết cho $3$, nên ta chỉ cần xét phần dư khi chia giá trị mỗi viên đá cho $3$.

Ta dùng mảng $\textit{cnt}$ có độ dài $3$ để đếm số viên đá còn lại có giá trị tương ứng với từng phần dư modulo $3$. Trong đó, $\textit{cnt}[0]$ là số viên đá có phần dư $0$, còn $\textit{cnt}[1]$ và $\textit{cnt}[2]$ lần lượt là số viên đá có phần dư $1$ và $2$.

Ở lượt đầu tiên, Alice không thể lấy viên đá có phần dư $0$, vì khi đó tổng giá trị các viên đá đã lấy sẽ chia hết cho $3$. Do đó, Alice chỉ có thể lấy viên đá có phần dư $1$ hoặc $2$.

Trước tiên, xét trường hợp Alice lấy viên đá có phần dư $1$. Khi Alice lấy một viên đá có phần dư $1$, phần dư của tổng giá trị các viên đá đã lấy khi chia cho $3$ sẽ không thay đổi, nên các viên đá có phần dư $0$ có thể được lấy ở bất kỳ lượt nào; tạm thời ta không xét chúng. Vì vậy, Bob chỉ có thể lấy các viên đá có phần dư $1$, sau đó Alice lấy các viên đá có phần dư $2$, rồi tiếp tục như vậy theo chuỗi $1, 1, 2, 1, 2, \ldots$. Trong trường hợp này, nếu lượt cuối là lượt lẻ và vẫn còn viên đá, Alice thắng; ngược lại, Bob thắng.

Với trường hợp Alice lấy viên đá có phần dư $2$ ở lượt đầu tiên, ta cũng có kết luận tương tự.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{stones}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stoneGameIX(self, stones: List[int]) -> bool:
        def check(cnt: List[int]) -> bool:
            if cnt[1] == 0:
                return False
            cnt[1] -= 1
            r = 1 + min(cnt[1], cnt[2]) * 2 + cnt[0]
            if cnt[1] > cnt[2]:
                cnt[1] -= 1
                r += 1
            return r % 2 == 1 and cnt[1] != cnt[2]

        c1 = [0] * 3
        for x in stones:
            c1[x % 3] += 1
        c2 = [c1[0], c1[2], c1[1]]
        return check(c1) or check(c2)
```

#### Java

```java
class Solution {
    public boolean stoneGameIX(int[] stones) {
        int[] c1 = new int[3];
        for (int x : stones) {
            c1[x % 3]++;
        }
        int[] c2 = {c1[0], c1[2], c1[1]};
        return check(c1) || check(c2);
    }

    private boolean check(int[] cnt) {
        if (--cnt[1] < 0) {
            return false;
        }
        int r = 1 + Math.min(cnt[1], cnt[2]) * 2 + cnt[0];
        if (cnt[1] > cnt[2]) {
            --cnt[1];
            ++r;
        }
        return r % 2 == 1 && cnt[1] != cnt[2];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool stoneGameIX(vector<int>& stones) {
        vector<int> c1(3);
        for (int x : stones) {
            ++c1[x % 3];
        }
        vector<int> c2 = {c1[0], c1[2], c1[1]};
        auto check = [](auto& cnt) -> bool {
            if (--cnt[1] < 0) {
                return false;
            }
            int r = 1 + min(cnt[1], cnt[2]) * 2 + cnt[0];
            if (cnt[1] > cnt[2]) {
                --cnt[1];
                ++r;
            }
            return r % 2 && cnt[1] != cnt[2];
        };
        return check(c1) || check(c2);
    }
};
```

#### Go

```go
func stoneGameIX(stones []int) bool {
	c1 := [3]int{}
	for _, x := range stones {
		c1[x%3]++
	}
	c2 := [3]int{c1[0], c1[2], c1[1]}
	check := func(cnt [3]int) bool {
		if cnt[1] == 0 {
			return false
		}
		cnt[1]--
		r := 1 + min(cnt[1], cnt[2])*2 + cnt[0]
		if cnt[1] > cnt[2] {
			cnt[1]--
			r++
		}
		return r%2 == 1 && cnt[1] != cnt[2]
	}
	return check(c1) || check(c2)
}
```

#### TypeScript

```ts
function stoneGameIX(stones: number[]): boolean {
    const c1: number[] = Array(3).fill(0);
    for (const x of stones) {
        ++c1[x % 3];
    }
    const c2: number[] = [c1[0], c1[2], c1[1]];
    const check = (cnt: number[]): boolean => {
        if (--cnt[1] < 0) {
            return false;
        }
        let r = 1 + Math.min(cnt[1], cnt[2]) * 2 + cnt[0];
        if (cnt[1] > cnt[2]) {
            --cnt[1];
            ++r;
        }
        return r % 2 === 1 && cnt[1] !== cnt[2];
    };
    return check(c1) || check(c2);
}
```

#### JavaScript

```js
function stoneGameIX(stones) {
    const c1 = Array(3).fill(0);
    for (const x of stones) {
        ++c1[x % 3];
    }
    const c2 = [c1[0], c1[2], c1[1]];
    const check = cnt => {
        if (--cnt[1] < 0) {
            return false;
        }
        let r = 1 + Math.min(cnt[1], cnt[2]) * 2 + cnt[0];
        if (cnt[1] > cnt[2]) {
            --cnt[1];
            ++r;
        }
        return r % 2 === 1 && cnt[1] !== cnt[2];
    };
    return check(c1) || check(c2);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng phép kiểm tra tính chẵn lẻ ở dạng công thức đóng. Ta cũng có thể giảm dần các số đếm theo chuỗi phần dư và khi phần dư cần thiết đã hết, kiểm tra xem các viên đá có phần dư $0$ có khiến chỉ số lượt chơi là số lẻ hay không.
>
> Vẫn chỉ cần đếm trong $O(n)$ rồi mô phỏng với số bước hằng số, nhờ đó dễ kiểm tra các trường hợp biên hơn.

<!-- thinking:end -->

Tương tự Lời giải 1, ta đếm phần dư của các giá trị viên đá khi chia cho $3$. Nước đi đầu tiên của Alice chỉ có thể lấy viên đá có phần dư $1$ hoặc $2$.

Với mỗi cách bắt đầu, cả hai người chơi tuân theo chuỗi phần dư tối ưu: bắt đầu bằng $1$ cho chuỗi $1, 1, 2, 1, 2, \ldots$; bắt đầu bằng $2$ là trường hợp đối xứng. Khi phần dư cần thiết đã hết, Alice thắng nếu các viên đá có phần dư $0$ còn lại khiến số lượt hiện tại là số lẻ.

Trả về $\text{true}$ nếu một trong hai cách bắt đầu giúp Alice thắng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng $\textit{stones}$.

<!-- tabs:start -->

#### TypeScript

```ts
function stoneGameIX(stones: number[]): boolean {
    if (stones.length === 1) return false;

    const cnt = Array(3).fill(0);
    for (const x of stones) cnt[x % 3]++;

    const check = (x: number, cnt: number[]): boolean => {
        let c = 1;
        if (--cnt[x] < 0) return false;

        while (cnt[1] || cnt[2]) {
            if (cnt[x]) {
                cnt[x]--;
                x = x === 1 ? 2 : 1;
            } else return (c + cnt[0]) % 2 === 1;
            c++;
        }

        return false;
    };

    return check(1, [...cnt]) || check(2, [...cnt]);
}
```

#### JavaScript

```js
function stoneGameIX(stones) {
    if (stones.length === 1) return false;

    const cnt = Array(3).fill(0);
    for (const x of stones) cnt[x % 3]++;

    const check = (x, cnt) => {
        let c = 1;
        if (--cnt[x] < 0) return false;

        while (cnt[1] || cnt[2]) {
            if (cnt[x]) {
                cnt[x]--;
                x = x === 1 ? 2 : 1;
            } else return (c + cnt[0]) % 2 === 1;
            c++;
        }

        return false;
    };

    return check(1, [...cnt]) || check(2, [...cnt]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
