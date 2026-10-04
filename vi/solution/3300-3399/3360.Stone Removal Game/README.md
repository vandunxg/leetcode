---
comments: true
difficulty: Easy
rating: 1267
source: Biweekly Contest 144 Q1
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [3360. Stone Removal Game](https://leetcode.com/problems/stone-removal-game)

[中文文档](/solution/3300-3399/3360.Stone%20Removal%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob đang chơi một trò chơi, trong đó họ lần lượt lấy đá khỏi một đống đá, với <em>Alice đi trước</em>.</p>

<ul>
	<li>Trong lượt đầu tiên, Alice lấy <strong>chính xác</strong> 10 viên đá.</li>
	<li>Trong mỗi lượt tiếp theo, mỗi người chơi lấy <strong>chính xác</strong> ít hơn <strong> </strong>1 viên đá<strong> </strong> so với đối thủ ở lượt trước.</li>
</ul>

<p>Người chơi không thể thực hiện lượt đi sẽ thua.</p>

<p>Cho một số nguyên dương <code>n</code>, hãy trả về <code>true</code> nếu Alice thắng trò chơi và <code>false</code> nếu ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 12</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Alice lấy 10 viên đá trong lượt đầu tiên, còn lại 2 viên đá cho Bob.</li>
	<li>Bob không thể lấy 9 viên đá, nên Alice thắng.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Alice không thể lấy 10 viên đá, nên Alice thua.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Người chơi lần lượt lấy $10,9,\ldots$ viên đá. Vì $n \le 50$, ta mô phỏng cho đến khi không thể thực hiện lượt đi.
>
> Nếu số lượt đi thành công $k$ là số lẻ, Alice đã thực hiện lượt đi hợp lệ cuối cùng và thắng.
>
> Mỗi bước làm giảm $x$ đi một, nên vòng lặp có độ phức tạp $O(\sqrt{n})$ và không cần quy hoạch động cho trò chơi.

<!-- thinking:end -->

Ta mô phỏng quá trình chơi theo mô tả đề bài cho đến khi trò chơi không thể tiếp tục.

Cụ thể, ta duy trì hai biến $x$ và $k$, lần lượt biểu diễn số viên đá có thể lấy trong lượt hiện tại và số thao tác đã thực hiện. Ban đầu, $x = 10$ và $k = 0$.

Trong mỗi vòng lặp, nếu số viên đá có thể lấy trong lượt hiện tại $x$ không vượt quá số viên đá còn lại $n$, ta lấy $x$ viên đá, giảm $x$ đi $1$ và tăng $k$ lên $1$. Ngược lại, ta không thể thực hiện thao tác và trò chơi kết thúc.

Cuối cùng, ta kiểm tra tính chẵn lẻ của $k$. Nếu $k$ là số lẻ, Alice thắng; nếu không, Bob thắng.

Độ phức tạp thời gian là $O(\sqrt{n})$, trong đó $n$ là số viên đá. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canAliceWin(self, n: int) -> bool:
        x, k = 10, 0
        while n >= x:
            n -= x
            x -= 1
            k += 1
        return k % 2 == 1
```

#### Java

```java
class Solution {
    public boolean canAliceWin(int n) {
        int x = 10, k = 0;
        while (n >= x) {
            n -= x;
            --x;
            ++k;
        }
        return k % 2 == 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canAliceWin(int n) {
        int x = 10, k = 0;
        while (n >= x) {
            n -= x;
            --x;
            ++k;
        }
        return k % 2;
    }
};
```

#### Go

```go
func canAliceWin(n int) bool {
	x, k := 10, 0
	for n >= x {
		n -= x
		x--
		k++
	}
	return k%2 == 1
}
```

#### TypeScript

```ts
function canAliceWin(n: number): boolean {
    let [x, k] = [10, 0];
    while (n >= x) {
        n -= x;
        --x;
        ++k;
    }
    return k % 2 === 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
