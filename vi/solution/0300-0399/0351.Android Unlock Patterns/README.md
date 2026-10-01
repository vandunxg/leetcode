---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [351. Android Unlock Patterns 🔒](https://leetcode.com/problems/android-unlock-patterns)

[中文文档](/solution/0300-0399/0351.Android%20Unlock%20Patterns/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết bị Android có màn hình khóa đặc biệt gồm các chấm xếp thành lưới <code>3 x 3</code>. Người dùng có thể đặt “mẫu mở khóa” bằng cách nối các chấm theo một thứ tự cụ thể, tạo thành một chuỗi đoạn thẳng nối tiếp nhau, trong đó hai đầu mỗi đoạn là hai chấm liên tiếp trong chuỗi. Một chuỗi gồm <code>k</code> chấm là mẫu mở khóa <strong>hợp lệ</strong> nếu thỏa mãn cả hai điều kiện sau:</p>

<ul>
	<li>Tất cả các chấm trong chuỗi đều <strong>khác nhau</strong>.</li>
	<li>Nếu đoạn thẳng nối hai chấm liên tiếp trong chuỗi đi qua <strong>tâm</strong> của một chấm khác, chấm đó <strong>phải xuất hiện trước đó</strong> trong chuỗi. Không được đi xuyên qua tâm của chấm chưa được chọn.
	<ul>
		<li>Ví dụ, nối chấm <code>2</code> với <code>9</code> mà chưa đi qua chấm <code>5</code> hay <code>6</code> vẫn hợp lệ, vì đoạn thẳng từ chấm <code>2</code> đến chấm <code>9</code> không đi qua tâm của chấm <code>5</code> hoặc <code>6</code>.</li>
		<li>Ngược lại, nối chấm <code>1</code> với <code>3</code> khi chấm <code>2</code> chưa xuất hiện trước đó là không hợp lệ, vì đoạn thẳng từ chấm <code>1</code> đến chấm <code>3</code> đi qua tâm chấm <code>2</code>.</li>
	</ul>
	</li>
</ul>

<p>Dưới đây là một số mẫu mở khóa hợp lệ và không hợp lệ:</p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0351.Android%20Unlock%20Patterns/images/android-unlock.png" style="width: 418px; height: 128px;" /></p>

<ul>
	<li>Mẫu thứ nhất <code>[4,1,3,6]</code> không hợp lệ vì đoạn thẳng nối chấm <code>1</code> và <code>3</code> đi qua chấm <code>2</code>, nhưng chấm <code>2</code> chưa xuất hiện trước đó trong chuỗi.</li>
	<li>Mẫu thứ hai <code>[4,1,9,2]</code> không hợp lệ vì đoạn thẳng nối chấm <code>1</code> và <code>9</code> đi qua chấm <code>5</code>, nhưng chấm <code>5</code> chưa xuất hiện trước đó trong chuỗi.</li>
	<li>Mẫu thứ ba <code>[2,4,1,3,6]</code> hợp lệ vì thỏa mãn các điều kiện. Đoạn thẳng nối chấm <code>1</code> và <code>3</code> thỏa mãn điều kiện vì chấm <code>2</code> đã xuất hiện trước đó trong chuỗi.</li>
	<li>Mẫu thứ tư <code>[6,5,4,1,9,2]</code> hợp lệ vì thỏa mãn các điều kiện. Đoạn thẳng nối chấm <code>1</code> và <code>9</code> thỏa mãn điều kiện vì chấm <code>5</code> đã xuất hiện trước đó trong chuỗi.</li>
</ul>

<p>Cho hai số nguyên <code>m</code> và <code>n</code>, hãy trả về <em><strong>số mẫu mở khóa hợp lệ và khác nhau</strong> trên màn hình khóa dạng lưới của Android, có độ dài tối thiểu </em><code>m</code><em> chấm và tối đa </em><code>n</code><em> chấm.</em></p>

<p>Hai mẫu mở khóa được xem là <strong>khác nhau</strong> nếu một chuỗi có chấm không xuất hiện trong chuỗi kia, hoặc thứ tự các chấm khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> m = 1, n = 1
<strong>Đầu ra:</strong> 9
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> m = 1, n = 2
<strong>Đầu ra:</strong> 65
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các mẫu mở khóa có độ dài trong khoảng $[m,n]$; chấm nằm giữa trên đường nối phải được chọn trước đó. Với 9 phím chỉ có $9\times 2^9$ trạng thái, nên dùng DFS là phù hợp.
>
> `cross[i][j]` lưu chấm trung gian bắt buộc. Từ $i$, thử các chấm $j$ chưa dùng mà không có chấm trung gian hoặc chấm trung gian đã được thăm. Đếm các mẫu có độ dài trong $[m,n]$. Tận dụng tính đối xứng: số mẫu bắt đầu từ $1$ và từ $2$ đều được nhân $4$, sau đó cộng số mẫu bắt đầu từ tâm $5$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfPatterns(self, m: int, n: int) -> int:
        def dfs(i: int, cnt: int = 1) -> int:
            if cnt > n:
                return 0
            vis[i] = True
            ans = int(cnt >= m)
            for j in range(1, 10):
                x = cross[i][j]
                if not vis[j] and (x == 0 or vis[x]):
                    ans += dfs(j, cnt + 1)
            vis[i] = False
            return ans

        cross = [[0] * 10 for _ in range(10)]
        cross[1][3] = cross[3][1] = 2
        cross[1][7] = cross[7][1] = 4
        cross[1][9] = cross[9][1] = 5
        cross[2][8] = cross[8][2] = 5
        cross[3][7] = cross[7][3] = 5
        cross[3][9] = cross[9][3] = 6
        cross[4][6] = cross[6][4] = 5
        cross[7][9] = cross[9][7] = 8
        vis = [False] * 10
        return dfs(1) * 4 + dfs(2) * 4 + dfs(5)
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[][] cross = new int[10][10];
    private boolean[] vis = new boolean[10];

    public int numberOfPatterns(int m, int n) {
        this.m = m;
        this.n = n;
        cross[1][3] = cross[3][1] = 2;
        cross[1][7] = cross[7][1] = 4;
        cross[1][9] = cross[9][1] = 5;
        cross[2][8] = cross[8][2] = 5;
        cross[3][7] = cross[7][3] = 5;
        cross[3][9] = cross[9][3] = 6;
        cross[4][6] = cross[6][4] = 5;
        cross[7][9] = cross[9][7] = 8;
        return dfs(1, 1) * 4 + dfs(2, 1) * 4 + dfs(5, 1);
    }

    private int dfs(int i, int cnt) {
        if (cnt > n) {
            return 0;
        }
        vis[i] = true;
        int ans = cnt >= m ? 1 : 0;
        for (int j = 1; j < 10; ++j) {
            int x = cross[i][j];
            if (!vis[j] && (x == 0 || vis[x])) {
                ans += dfs(j, cnt + 1);
            }
        }
        vis[i] = false;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfPatterns(int m, int n) {
        int cross[10][10];
        memset(cross, 0, sizeof(cross));
        bool vis[10];
        memset(vis, false, sizeof(vis));
        cross[1][3] = cross[3][1] = 2;
        cross[1][7] = cross[7][1] = 4;
        cross[1][9] = cross[9][1] = 5;
        cross[2][8] = cross[8][2] = 5;
        cross[3][7] = cross[7][3] = 5;
        cross[3][9] = cross[9][3] = 6;
        cross[4][6] = cross[6][4] = 5;
        cross[7][9] = cross[9][7] = 8;

        function<int(int, int)> dfs = [&](int i, int cnt) {
            if (cnt > n) {
                return 0;
            }
            vis[i] = true;
            int ans = cnt >= m ? 1 : 0;
            for (int j = 1; j < 10; ++j) {
                int x = cross[i][j];
                if (!vis[j] && (x == 0 || vis[x])) {
                    ans += dfs(j, cnt + 1);
                }
            }
            vis[i] = false;
            return ans;
        };

        return dfs(1, 1) * 4 + dfs(2, 1) * 4 + dfs(5, 1);
    }
};
```

#### Go

```go
func numberOfPatterns(m int, n int) int {
	cross := [10][10]int{}
	vis := [10]bool{}
	cross[1][3] = 2
	cross[1][7] = 4
	cross[1][9] = 5
	cross[2][8] = 5
	cross[3][7] = 5
	cross[3][9] = 6
	cross[4][6] = 5
	cross[7][9] = 8
	cross[3][1] = 2
	cross[7][1] = 4
	cross[9][1] = 5
	cross[8][2] = 5
	cross[7][3] = 5
	cross[9][3] = 6
	cross[6][4] = 5
	cross[9][7] = 8
	var dfs func(int, int) int
	dfs = func(i, cnt int) int {
		if cnt > n {
			return 0
		}
		vis[i] = true
		ans := 0
		if cnt >= m {
			ans++
		}
		for j := 1; j < 10; j++ {
			x := cross[i][j]
			if !vis[j] && (x == 0 || vis[x]) {
				ans += dfs(j, cnt+1)
			}
		}
		vis[i] = false
		return ans
	}
	return dfs(1, 1)*4 + dfs(2, 1)*4 + dfs(5, 1)
}
```

#### TypeScript

```ts
function numberOfPatterns(m: number, n: number): number {
    const cross: number[][] = Array(10)
        .fill(0)
        .map(() => Array(10).fill(0));
    const vis: boolean[] = Array(10).fill(false);
    cross[1][3] = cross[3][1] = 2;
    cross[1][7] = cross[7][1] = 4;
    cross[1][9] = cross[9][1] = 5;
    cross[2][8] = cross[8][2] = 5;
    cross[3][7] = cross[7][3] = 5;
    cross[3][9] = cross[9][3] = 6;
    cross[4][6] = cross[6][4] = 5;
    cross[7][9] = cross[9][7] = 8;
    const dfs = (i: number, cnt: number): number => {
        if (cnt > n) {
            return 0;
        }
        vis[i] = true;
        let ans = 0;
        if (cnt >= m) {
            ++ans;
        }
        for (let j = 1; j < 10; ++j) {
            const x = cross[i][j];
            if (!vis[j] && (x === 0 || vis[x])) {
                ans += dfs(j, cnt + 1);
            }
        }
        vis[i] = false;
        return ans;
    };
    return dfs(1, 1) * 4 + dfs(2, 1) * 4 + dfs(5, 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
