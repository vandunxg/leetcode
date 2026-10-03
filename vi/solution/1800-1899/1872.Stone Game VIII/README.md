---
comments: true
difficulty: Hard
rating: 2439
source: Weekly Contest 242 Q4
tags:
    - Minimax
    - Array
    - Math
    - Dynamic Programming
    - Game Theory
    - Prefix Sum
    - Zero-Sum Game
---

<!-- problem:start -->

# [1872. Stone Game VIII](https://leetcode.com/problems/stone-game-viii)

[中文文档](/solution/1800-1899/1872.Stone%20Game%20VIII/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob lần lượt chơi một trò chơi, trong đó <strong>Alice đi trước</strong>.</p>

<p>Có <code>n</code> viên đá xếp thành một hàng. Trong lượt của mỗi người chơi, khi số viên đá còn <strong>lớn hơn một</strong>, người đó thực hiện các bước sau:</p>

<ol>
	<li>Chọn một số nguyên <code>x &gt; 1</code> và <strong>loại bỏ</strong> <code>x</code> viên đá ngoài cùng bên trái khỏi hàng.</li>
	<li>Cộng <strong>tổng</strong> giá trị của các viên đá đã <strong>loại bỏ</strong> vào điểm của người chơi.</li>
	<li>Đặt một <strong>viên đá mới</strong> có giá trị bằng tổng đó vào phía bên trái của hàng.</li>
</ol>

<p>Trò chơi kết thúc khi trong hàng chỉ còn <strong>chỉ</strong> <strong>một</strong> viên đá.</p>

<p><strong>Chênh lệch điểm số</strong> giữa Alice và Bob là <code>(Alice&#39;s score - Bob&#39;s score)</code>. Mục tiêu của Alice là <strong>tối đa hóa</strong> chênh lệch điểm số, còn mục tiêu của Bob là <strong>tối thiểu hóa</strong> chênh lệch điểm số.</p>

<p>Cho mảng số nguyên <code>stones</code> có độ dài <code>n</code>, trong đó <code>stones[i]</code> biểu thị giá trị của viên đá thứ <code>i<sup>th</sup></code> <strong>tính từ trái sang</strong>, hãy trả về <em><strong>chênh lệch điểm số</strong> giữa Alice và Bob nếu cả hai đều chơi <strong>tối ưu</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [-1,2,-3,4,-5]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
- Alice loại bỏ 4 viên đá đầu tiên, cộng (-1) + 2 + (-3) + 4 = 2 vào điểm của mình và đặt một viên đá có giá trị 2 vào bên
  trái. stones = [2,-5].
- Bob loại bỏ 2 viên đá đầu tiên, cộng 2 + (-5) = -3 vào điểm của mình và đặt một viên đá có giá trị -3 vào
  bên trái. stones = [-3].
Chênh lệch điểm số của họ là 2 - (-3) = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [7,-6,5,10,5,-2,-6]
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong>
- Alice loại bỏ tất cả các viên đá, cộng 7 + (-6) + 5 + 10 + 5 + (-2) + (-6) = 13 vào điểm của mình và đặt một
  viên đá có giá trị 13 vào bên trái. stones = [13].
Chênh lệch điểm số của họ là 13 - 0 = 13.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [-10,-12]
<strong>Đầu ra:</strong> -22
<strong>Giải thích:</strong>
- Alice chỉ có thể thực hiện một nước đi là loại bỏ cả hai viên đá. Cô ấy cộng (-10) + (-12) = -22 vào điểm của
  mình và đặt một viên đá có giá trị -22 vào bên trái. stones = [-22].
Chênh lệch điểm số của họ là (-22) - 0 = -22.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == stones.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= stones[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nước đi gộp một tiền tố gồm ít nhất hai đống và ghi điểm bằng tổng tiền tố mới; cả hai người chơi đều tối ưu chênh lệch điểm số. $n\le 10^5$ khiến việc thử mọi vị trí cắt là không thể.
>
> Việc gộp không làm thay đổi mảng prefix sum $s$. Từ chỉ số $i$, người chơi hiện tại có thể lấy $s[i]$ và để lại $i+1$ cho đối thủ, hoặc trì hoãn việc cắt. $dfs(i)=\max(dfs(i+1),s[i]-dfs(i+1))$, bắt đầu tại $i=1$. Tìm kiếm có ghi nhớ tạo ra số trạng thái tuyến tính.

<!-- thinking:end -->

Theo mô tả bài toán, mỗi lần lấy $x$ viên đá ngoài cùng bên trái, cộng tổng của chúng vào điểm rồi đặt một viên đá có giá trị bằng tổng đó ở ngoài cùng bên trái tương đương với việc gộp $x$ viên đá này thành một viên đá có giá trị bằng tổng, và prefix sum không đổi.

Ta có thể dùng mảng prefix sum $s$ có độ dài $n$ để biểu diễn prefix sum của mảng $stones$, trong đó $s[i]$ biểu thị tổng các phần tử $stones[0..i]$.

Tiếp theo, ta xây dựng hàm $dfs(i)$, biểu thị việc hiện tại ta lấy các viên đá từ $stones[i:]$ và trả về chênh lệch điểm số lớn nhất mà người chơi hiện tại có thể đạt được.

Hàm $dfs(i)$ được thực hiện như sau:

- Nếu $i \geq n - 1$, nghĩa là hiện tại ta chỉ có thể lấy tất cả các viên đá, nên trả về $s[n - 1]$.
- Ngược lại, ta có thể chọn lấy tất cả các viên đá từ $stones[i + 1:]$, khi đó chênh lệch điểm số nhận được là $dfs(i + 1)$; hoặc chọn lấy các viên đá $stones[:i]$, khi đó chênh lệch điểm số nhận được là $s[i] - dfs(i + 1)$. Ta lấy giá trị lớn hơn trong hai trường hợp, đó là chênh lệch điểm số lớn nhất mà người chơi hiện tại có thể đạt được.

Cuối cùng, chênh lệch điểm số giữa Alice và Bob là $dfs(1)$, tức là Alice phải bắt đầu trò chơi bằng cách lấy các viên đá từ $stones[1:]$.

Để tránh tính toán lặp lại, ta có thể dùng tìm kiếm có ghi nhớ.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài mảng $stones$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stoneGameVIII(self, stones: List[int]) -> int:
        @cache
        def dfs(i: int) -> int:
            if i >= len(stones) - 1:
                return s[-1]
            return max(dfs(i + 1), s[i] - dfs(i + 1))

        s = list(accumulate(stones))
        return dfs(1)
```

#### Java

```java
class Solution {
    private Integer[] f;
    private int[] s;
    private int n;

    public int stoneGameVIII(int[] stones) {
        n = stones.length;
        f = new Integer[n];
        for (int i = 1; i < n; ++i) {
            stones[i] += stones[i - 1];
        }
        s = stones;
        return dfs(1);
    }

    private int dfs(int i) {
        if (i >= n - 1) {
            return s[i];
        }
        if (f[i] == null) {
            f[i] = Math.max(dfs(i + 1), s[i] - dfs(i + 1));
        }
        return f[i];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int stoneGameVIII(vector<int>& stones) {
        int n = stones.size();
        for (int i = 1; i < n; ++i) {
            stones[i] += stones[i - 1];
        }
        int f[n];
        memset(f, -1, sizeof(f));
        function<int(int)> dfs = [&](int i) -> int {
            if (i >= n - 1) {
                return stones[i];
            }
            if (f[i] == -1) {
                f[i] = max(dfs(i + 1), stones[i] - dfs(i + 1));
            }
            return f[i];
        };
        return dfs(1);
    }
};
```

#### Go

```go
func stoneGameVIII(stones []int) int {
	n := len(stones)
	f := make([]int, n)
	for i := range f {
		f[i] = -1
	}
	for i := 1; i < n; i++ {
		stones[i] += stones[i-1]
	}
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n-1 {
			return stones[i]
		}
		if f[i] == -1 {
			f[i] = max(dfs(i+1), stones[i]-dfs(i+1))
		}
		return f[i]
	}
	return dfs(1)
}
```

#### TypeScript

```ts
function stoneGameVIII(stones: number[]): number {
    const n = stones.length;
    const f: number[] = Array(n).fill(-1);
    for (let i = 1; i < n; ++i) {
        stones[i] += stones[i - 1];
    }
    const dfs = (i: number): number => {
        if (i >= n - 1) {
            return stones[i];
        }
        if (f[i] === -1) {
            f[i] = Math.max(dfs(i + 1), stones[i] - dfs(i + 1));
        }
        return f[i];
    };
    return dfs(1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Prefix Sum + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 chỉ nhìn một bước về bên phải. Duyệt từ cuối về đầu: $f$ bắt đầu bằng $s[n-1]$, sau đó $f=\max(f,s[i]-f)$ với $i=n-2,\ldots,1$. Không gian phụ giảm xuống hằng số.

<!-- thinking:end -->

Ta cũng có thể dùng quy hoạch động để giải bài toán này.

Tương tự Lời giải 1, trước hết ta dùng mảng prefix sum $s$ có độ dài $n$ để biểu diễn prefix sum của mảng $stones$, trong đó $s[i]$ biểu thị tổng các phần tử $stones[0..i]$.

Ta định nghĩa $f[i]$ là chênh lệch điểm số lớn nhất mà người chơi hiện tại có thể đạt được khi lấy các viên đá từ $stones[i:]$.

Nếu người chơi chọn lấy các viên đá $stones[:i]$, điểm nhận được là $s[i]$. Khi đó, người chơi còn lại sẽ lấy các viên đá từ $stones[i+1:]$, và chênh lệch điểm số lớn nhất mà người chơi đó có thể đạt được là $f[i+1]$. Vì vậy, chênh lệch điểm số lớn nhất mà người chơi hiện tại có thể đạt được là $s[i] - f[i+1]$.

Nếu người chơi chọn lấy các viên đá từ $stones[i+1:]$, chênh lệch điểm số lớn nhất nhận được là $f[i+1]$.

Do đó, ta có công thức chuyển trạng thái:

$$
f[i] = \max\{s[i] - f[i+1], f[i+1]\}
$$

Cuối cùng, chênh lệch điểm số giữa Alice và Bob là $f[1]$, tức là Alice phải bắt đầu trò chơi bằng cách lấy các viên đá từ $stones[1:]$.

Ta nhận thấy $f[i]$ chỉ liên quan đến $f[i+1]$, nên chỉ cần dùng một biến $f$ để biểu diễn $f[i]$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(1)$. Ở đây, $n$ là độ dài mảng $stones$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stoneGameVIII(self, stones: List[int]) -> int:
        s = list(accumulate(stones))
        f = s[-1]
        for i in range(len(s) - 2, 0, -1):
            f = max(f, s[i] - f)
        return f
```

#### Java

```java
class Solution {
    public int stoneGameVIII(int[] stones) {
        int n = stones.length;
        for (int i = 1; i < n; ++i) {
            stones[i] += stones[i - 1];
        }
        int f = stones[n - 1];
        for (int i = n - 2; i > 0; --i) {
            f = Math.max(f, stones[i] - f);
        }
        return f;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int stoneGameVIII(vector<int>& stones) {
        int n = stones.size();
        for (int i = 1; i < n; ++i) {
            stones[i] += stones[i - 1];
        }
        int f = stones.back();
        for (int i = n - 2; i; --i) {
            f = max(f, stones[i] - f);
        }
        return f;
    }
};
```

#### TypeScript

```ts
function stoneGameVIII(stones: number[]): number {
    const n = stones.length;
    for (let i = 1; i < n; ++i) {
        stones[i] += stones[i - 1];
    }
    let f = stones[n - 1];
    for (let i = n - 2; i; --i) {
        f = Math.max(f, stones[i] - f);
    }
    return f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
