---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.06. Number Of 2s In Range](https://leetcode.cn/problems/number-of-2s-in-range-lcci)

[中文文档](/lcci/17.06.Number%20Of%202s%20In%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một phương thức để đếm số lần chữ số 2 xuất hiện trong tất cả các số từ 0 đến n (bao gồm cả n).</p>
<p><strong>Ví dụ:</strong></p>
<pre>

<strong>Đầu vào: </strong>25

<strong>Đầu ra: </strong>9

<strong>Giải thích: </strong>(2, 12, 20, 21, 22, 23, 24, 25)(Lưu ý rằng 22 được tính là hai chữ số 2.)</pre>

<p>Lưu ý:</p>
<ul>
	<li><code>n &lt;= 10^9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Đếm chữ số $2$ trong $[0,n]$. Công thức đóng theo từng vị trí có thể dùng được, nhưng việc tách high/low/current rất dễ bị nhầm.
>
> Digit DP lưu các vị trí còn lại, số lượng chữ số 2 đã có và việc prefix có đang tight hay không, nên memoization chỉ phụ thuộc vào các trạng thái đó.
>
> Tách $n$ thành $a[1..l]$; $dfs(pos,cnt,limit)$ liệt kê $0\ldots up$ từ hàng cao xuống. Một $limit$ tight sẽ giới hạn chữ số tại $a[pos]$. Đáp án là $dfs(l,0,True)$.

<!-- thinking:end -->

Bài toán này về cơ bản là đếm số lần xuất hiện của chữ số $2$ trong đoạn $[l,..r]$. Số lượng này liên quan đến số chữ số và chữ số ở mỗi vị trí. Ta có thể dùng ý tưởng Digit DP để giải bài toán này. Với Digit DP, độ dài của số ít ảnh hưởng đến độ phức tạp.

Với đoạn $[l,..r]$, thông thường ta chuyển nó thành $[1,..r]$ rồi lấy hiệu với $[1,..l - 1]$, tức là,

$$
ans = \sum_{i=1}^{r} ans_i -  \sum_{i=1}^{l-1} ans_i
$$

Tuy nhiên, với bài toán này, ta chỉ cần tìm giá trị trong đoạn $[1,..r]$.

Ở đây, ta dùng memoization để triển khai Digit DP. Ta bắt đầu từ trên xuống dưới để lấy số lượng phương án, sau đó trả kết quả theo từng tầng và cộng dồn, cuối cùng thu được đáp án tại điểm bắt đầu tìm kiếm.

Các bước cơ bản như sau:

1. Chuyển số $n$ thành một mảng int $a$, trong đó $a[1]$ là chữ số có ý nghĩa thấp nhất, còn $a[len]$ là chữ số có ý nghĩa cao nhất.
2. Thiết kế hàm $dfs()$ dựa trên thông tin của bài toán. Với bài toán này, ta định nghĩa $dfs(pos, cnt, limit)$, và đáp án là $dfs(len, 0, true)$.

Trong đó:

- `pos` biểu thị vị trí chữ số, bắt đầu từ chữ số có ý nghĩa thấp nhất hoặc chữ số đầu tiên, thường tùy thuộc vào tính chất xây dựng chữ số của bài toán. Với bài toán này, ta chọn bắt đầu từ chữ số có ý nghĩa cao nhất, vì vậy giá trị ban đầu của `pos` là `len`.
- `cnt` biểu thị số lượng $2$ trong số hiện tại.
- `limit` biểu thị giới hạn đối với các chữ số có thể điền. Nếu không có giới hạn, ta có thể chọn $[0,1,..9]$; ngược lại, ta chỉ có thể chọn $[0,..a[pos]]$. Nếu `limit` là `true` và đã đạt giá trị lớn nhất, thì `limit` tiếp theo cũng là `true`. Nếu `limit` là `true` nhưng chưa đạt giá trị lớn nhất, hoặc nếu `limit` là `false`, thì `limit` tiếp theo là `false`.

Chi tiết triển khai hàm này, vui lòng xem code bên dưới.

Độ phức tạp thời gian là $O(\log n)$.

Các bài tương tự:

- [233. Number of Digit One](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0233.Number%20of%20Digit%20One/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOf2sInRange(self, n: int) -> int:
        @cache
        def dfs(pos, cnt, limit):
            if pos <= 0:
                return cnt
            up = a[pos] if limit else 9
            ans = 0
            for i in range(up + 1):
                ans += dfs(pos - 1, cnt + (i == 2), limit and i == up)
            return ans

        a = [0] * 12
        l = 0
        while n:
            l += 1
            a[l] = n % 10
            n //= 10
        return dfs(l, 0, True)
```

#### Java

```java
class Solution {
    private int[] a = new int[12];
    private int[][] dp = new int[12][12];

    public int numberOf2sInRange(int n) {
        int len = 0;
        while (n > 0) {
            a[++len] = n % 10;
            n /= 10;
        }
        for (var e : dp) {
            Arrays.fill(e, -1);
        }
        return dfs(len, 0, true);
    }

    private int dfs(int pos, int cnt, boolean limit) {
        if (pos <= 0) {
            return cnt;
        }
        if (!limit && dp[pos][cnt] != -1) {
            return dp[pos][cnt];
        }
        int up = limit ? a[pos] : 9;
        int ans = 0;
        for (int i = 0; i <= up; ++i) {
            ans += dfs(pos - 1, cnt + (i == 2 ? 1 : 0), limit && i == up);
        }
        if (!limit) {
            dp[pos][cnt] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int a[12];
    int dp[12][12];

    int numberOf2sInRange(int n) {
        int len = 0;
        while (n) {
            a[++len] = n % 10;
            n /= 10;
        }
        memset(dp, -1, sizeof dp);
        return dfs(len, 0, true);
    }

    int dfs(int pos, int cnt, bool limit) {
        if (pos <= 0) {
            return cnt;
        }
        if (!limit && dp[pos][cnt] != -1) {
            return dp[pos][cnt];
        }
        int ans = 0;
        int up = limit ? a[pos] : 9;
        for (int i = 0; i <= up; ++i) {
            ans += dfs(pos - 1, cnt + (i == 2), limit && i == up);
        }
        if (!limit) {
            dp[pos][cnt] = ans;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOf2sInRange(n int) int {
	a := make([]int, 12)
	dp := make([][]int, 12)
	for i := range dp {
		dp[i] = make([]int, 12)
		for j := range dp[i] {
			dp[i][j] = -1
		}
	}
	l := 0
	for n > 0 {
		l++
		a[l] = n % 10
		n /= 10
	}
	var dfs func(int, int, bool) int
	dfs = func(pos, cnt int, limit bool) int {
		if pos <= 0 {
			return cnt
		}
		if !limit && dp[pos][cnt] != -1 {
			return dp[pos][cnt]
		}
		up := 9
		if limit {
			up = a[pos]
		}
		ans := 0
		for i := 0; i <= up; i++ {
			t := cnt
			if i == 2 {
				t++
			}
			ans += dfs(pos-1, t, limit && i == up)
		}
		if !limit {
			dp[pos][cnt] = ans
		}
		return ans
	}
	return dfs(l, 0, true)
}
```

#### Swift

```swift
class Solution {
    private var a = [Int](repeating: 0, count: 12)
    private var dp = [[Int]](repeating: [Int](repeating: -1, count: 12), count: 12)

    func numberOf2sInRange(_ n: Int) -> Int {
        var n = n
        var len = 0
        while n > 0 {
            len += 1
            a[len] = n % 10
            n /= 10
        }
        for i in 0..<12 {
            dp[i] = [Int](repeating: -1, count: 12)
        }
        return dfs(len, 0, true)
    }

    private func dfs(_ pos: Int, _ cnt: Int, _ limit: Bool) -> Int {
        if pos <= 0 {
            return cnt
        }
        if !limit && dp[pos][cnt] != -1 {
            return dp[pos][cnt]
        }
        let up = limit ? a[pos] : 9
        var ans = 0
        for i in 0...up {
            ans += dfs(pos - 1, cnt + (i == 2 ? 1 : 0), limit && i == up)
        }
        if !limit {
            dp[pos][cnt] = ans
        }
        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
