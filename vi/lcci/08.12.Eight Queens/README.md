---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [08.12. Eight Queens](https://leetcode.cn/problems/eight-queens-lcci)

[中文文档](/lcci/08.12.Eight%20Queens/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một thuật toán để in ra mọi cách sắp xếp n quân hậu trên bàn cờ n x n sao cho không quân nào cùng hàng, cột hoặc đường chéo. Trong trường hợp này, &quot;đường chéo&quot; nghĩa là tất cả các đường chéo, không chỉ hai đường chéo đi qua tâm bàn cờ.</p>

<p><strong>Lưu ý: </strong>Bài toán này là phiên bản tổng quát của bài toán ban đầu trong sách.</p>

<p><strong>Ví dụ:</strong></p>

<pre>

<strong> Đầu vào</strong>: 4

<strong> Đầu ra</strong>: [[&quot;.Q..&quot;,&quot;...Q&quot;,&quot;Q...&quot;,&quot;..Q.&quot;],[&quot;..Q.&quot;,&quot;Q...&quot;,&quot;...Q&quot;,&quot;.Q..&quot;]]

<strong> Giải thích</strong>: 4 quân hậu có hai lời giải sau

[

&nbsp;[&quot;.Q..&quot;, &nbsp;// solution 1

&nbsp; &quot;...Q&quot;,

&nbsp; &quot;Q...&quot;,

&nbsp; &quot;..Q.&quot;],



&nbsp;[&quot;..Q.&quot;, &nbsp;// solution 2

&nbsp; &quot;Q...&quot;,

&nbsp; &quot;...Q&quot;,

&nbsp; &quot;.Q..&quot;]

]

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS (Backtracking)

<!-- thinking:start -->

> **Tư duy**
>
> Cần đặt $n$ quân hậu không ăn nhau. Nếu thử các hoán vị rồi quét các đường chéo, ta sẽ phải thực hiện $n!$ lần kiểm tra tuyến tính.
>
> Khi đặt quân theo từng hàng, xung đột chỉ nằm ở một cột và hai đường chéo, nên có thể dùng các mảng để kiểm tra trong $O(1)$.
>
> $col[j]$, $dg[i+j]$ và $udg[n-i+j]$ đánh dấu các vị trí đã có quân; $dfs(i)$ thử các cột của hàng $i$ rồi hoàn tác trạng thái bàn cờ. $n$ đủ nhỏ để liệt kê mọi lời giải.

<!-- thinking:end -->

Chúng ta định nghĩa ba mảng $col$, $dg$ và $udg$ để biểu diễn việc cột, đường chéo chính và đường chéo phụ tương ứng đã có quân hậu hay chưa. Nếu có một quân hậu tại vị trí $(i, j)$ thì $col[j]$, $dg[i + j]$ và $udg[n - i + j]$ đều bằng $1$. Ngoài ra, chúng ta dùng một mảng $g$ để ghi lại trạng thái hiện tại của bàn cờ, trong đó mọi phần tử của $g$ ban đầu đều là `'.'`.

Tiếp theo, chúng ta định nghĩa hàm $dfs(i)$, biểu diễn việc bắt đầu đặt quân hậu từ hàng thứ $i$.

Trong $dfs(i)$, nếu $i = n$ thì nghĩa là đã đặt xong tất cả quân hậu. Chúng ta đưa $g$ hiện tại vào mảng kết quả rồi kết thúc đệ quy.

Nếu không, chúng ta lần lượt xét từng cột $j$ của hàng hiện tại. Nếu vị trí $(i, j)$ không có quân hậu, tức là $col[j]$, $dg[i + j]$ và $udg[n - i + j]$ đều bằng $0$, thì chúng ta có thể đặt một quân hậu: đổi $g[i][j]$ thành `'Q'` và đặt $col[j]$, $dg[i + j]$ và $udg[n - i + j]$ thành $1$. Sau đó, chúng ta tiếp tục tìm kiếm ở hàng kế tiếp, tức là gọi $dfs(i + 1)$. Khi đệ quy kết thúc, cần đổi $g[i][j]$ trở lại `'.'` và đặt $col[j]$, $dg[i + j]$ và $udg[n - i + j]$ về $0$.

Trong hàm chính, chúng ta gọi $dfs(0)$ để bắt đầu đệ quy, cuối cùng trả về mảng kết quả.

Độ phức tạp thời gian là $O(n^2 \times n!)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số nguyên được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        def dfs(i: int):
            if i == n:
                ans.append(["".join(row) for row in g])
                return
            for j in range(n):
                if col[j] + dg[i + j] + udg[n - i + j] == 0:
                    g[i][j] = "Q"
                    col[j] = dg[i + j] = udg[n - i + j] = 1
                    dfs(i + 1)
                    col[j] = dg[i + j] = udg[n - i + j] = 0
                    g[i][j] = "."

        ans = []
        g = [["."] * n for _ in range(n)]
        col = [0] * n
        dg = [0] * (n << 1)
        udg = [0] * (n << 1)
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    private List<List<String>> ans = new ArrayList<>();
    private int[] col;
    private int[] dg;
    private int[] udg;
    private String[][] g;
    private int n;

    public List<List<String>> solveNQueens(int n) {
        this.n = n;
        col = new int[n];
        dg = new int[n << 1];
        udg = new int[n << 1];
        g = new String[n][n];
        for (int i = 0; i < n; ++i) {
            Arrays.fill(g[i], ".");
        }
        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i == n) {
            List<String> t = new ArrayList<>();
            for (int j = 0; j < n; ++j) {
                t.add(String.join("", g[j]));
            }
            ans.add(t);
            return;
        }
        for (int j = 0; j < n; ++j) {
            if (col[j] + dg[i + j] + udg[n - i + j] == 0) {
                g[i][j] = "Q";
                col[j] = dg[i + j] = udg[n - i + j] = 1;
                dfs(i + 1);
                col[j] = dg[i + j] = udg[n - i + j] = 0;
                g[i][j] = ".";
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<int> col(n);
        vector<int> dg(n << 1);
        vector<int> udg(n << 1);
        vector<vector<string>> ans;
        vector<string> t(n, string(n, '.'));
        function<void(int)> dfs = [&](int i) -> void {
            if (i == n) {
                ans.push_back(t);
                return;
            }
            for (int j = 0; j < n; ++j) {
                if (col[j] + dg[i + j] + udg[n - i + j] == 0) {
                    t[i][j] = 'Q';
                    col[j] = dg[i + j] = udg[n - i + j] = 1;
                    dfs(i + 1);
                    col[j] = dg[i + j] = udg[n - i + j] = 0;
                    t[i][j] = '.';
                }
            }
        };
        dfs(0);
        return ans;
    }
};
```

#### Go

```go
func solveNQueens(n int) (ans [][]string) {
	col := make([]int, n)
	dg := make([]int, n<<1)
	udg := make([]int, n<<1)
	t := make([][]byte, n)
	for i := range t {
		t[i] = make([]byte, n)
		for j := range t[i] {
			t[i][j] = '.'
		}
	}
	var dfs func(int)
	dfs = func(i int) {
		if i == n {
			tmp := make([]string, n)
			for i := range tmp {
				tmp[i] = string(t[i])
			}
			ans = append(ans, tmp)
			return
		}
		for j := 0; j < n; j++ {
			if col[j]+dg[i+j]+udg[n-i+j] == 0 {
				col[j], dg[i+j], udg[n-i+j] = 1, 1, 1
				t[i][j] = 'Q'
				dfs(i + 1)
				t[i][j] = '.'
				col[j], dg[i+j], udg[n-i+j] = 0, 0, 0
			}
		}
	}
	dfs(0)
	return
}
```

#### TypeScript

```ts
function solveNQueens(n: number): string[][] {
    const col: number[] = Array(n).fill(0);
    const dg: number[] = Array(n << 1).fill(0);
    const udg: number[] = Array(n << 1).fill(0);
    const ans: string[][] = [];
    const t: string[][] = Array.from({ length: n }, () => Array(n).fill('.'));
    const dfs = (i: number) => {
        if (i === n) {
            ans.push(t.map(x => x.join('')));
            return;
        }
        for (let j = 0; j < n; ++j) {
            if (col[j] + dg[i + j] + udg[n - i + j] === 0) {
                t[i][j] = 'Q';
                col[j] = dg[i + j] = udg[n - i + j] = 1;
                dfs(i + 1);
                col[j] = dg[i + j] = udg[n - i + j] = 0;
                t[i][j] = '.';
            }
        }
    };
    dfs(0);
    return ans;
}
```

#### C#

```cs
public class Solution {
    private int n;
    private int[] col;
    private int[] dg;
    private int[] udg;
    private IList<IList<string>> ans = new List<IList<string>>();
    private IList<string> t = new List<string>();

    public IList<IList<string>> SolveNQueens(int n) {
        this.n = n;
        col = new int[n];
        dg = new int[n << 1];
        udg = new int[n << 1];
        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i == n) {
            ans.Add(new List<string>(t));
            return;
        }
        for (int j = 0; j < n; ++j) {
            if (col[j] + dg[i + j] + udg[n - i + j] == 0) {
                char[] row = new char[n];
                Array.Fill(row, '.');
                row[j] = 'Q';
                t.Add(new string(row));
                col[j] = dg[i + j] = udg[n - i + j] = 1;
                dfs(i + 1);
                col[j] = dg[i + j] = udg[n - i + j] = 0;
                t.RemoveAt(t.Count - 1);
            }
        }
    }
}
```

#### Swift

```swift
class Solution {
    private var ans: [[String]] = []
    private var col: [Int] = Array(repeating: 0, count: 0)
    private var dg: [Int] = Array(repeating: 0, count: 0)
    private var udg: [Int] = Array(repeating: 0, count: 0)
    private var g: [[String]] = Array(repeating: Array(repeating: ".", count: 0), count: 0)
    private var n: Int = 0

    func solveNQueens(_ n: Int) -> [[String]] {
        self.n = n
        col = Array(repeating: 0, count: n)
        dg = Array(repeating: 0, count: n * 2)
        udg = Array(repeating: 0, count: n * 2)
        g = Array(repeating: Array(repeating: ".", count: n), count: n)
        dfs(0)
        return ans
    }

    private func dfs(_ i: Int) {
        guard i < n else {
            let t = g.map { $0.joined() }
            ans.append(t)
            return
        }
        for j in 0..<n {
            if col[j] + dg[i + j] + udg[n - i + j] == 0 {
                g[i][j] = "Q"
                col[j] = 1
                dg[i + j] = 1
                udg[n - i + j] = 1
                dfs(i + 1)
                col[j] = 0
                dg[i + j] = 0
                udg[n - i + j] = 0
                g[i][j] = "."
            }
        }
    }

}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
