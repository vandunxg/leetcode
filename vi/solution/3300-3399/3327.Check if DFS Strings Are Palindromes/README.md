---
comments: true
difficulty: Hard
rating: 2454
source: Weekly Contest 420 Q4
tags:
    - Tree
    - Depth-First Search
    - Array
    - Hash Table
    - String
    - Hash Function
---

<!-- problem:start -->

# [3327. Check if DFS Strings Are Palindromes](https://leetcode.com/problems/check-if-dfs-strings-are-palindromes)

[中文文档](/solution/3300-3399/3327.Check%20if%20DFS%20Strings%20Are%20Palindromes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây có gốc tại node 0, gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Cây được biểu diễn bằng mảng <code>parent</code> có kích thước <code>n</code>, trong đó <code>parent[i]</code> là node cha của node <code>i</code>. Vì node 0 là gốc nên <code>parent[0] == -1</code>.</p>

<p>Đồng thời, bạn được cho một chuỗi <code>s</code> có độ dài <code>n</code>, trong đó <code>s[i]</code> là ký tự được gán cho node <code>i</code>.</p>

<p>Xét một chuỗi rỗng <code>dfsStr</code>, và định nghĩa hàm đệ quy <code>dfs(int x)</code> nhận node <code>x</code> làm tham số, thực hiện lần lượt các bước sau:</p>

<ul>
    <li>Duyệt từng node con <code>y</code> của <code>x</code> <strong>theo thứ tự tăng dần của số hiệu node</strong>, rồi gọi <code>dfs(y)</code>.</li>
    <li>Thêm ký tự <code>s[x]</code> vào cuối chuỗi <code>dfsStr</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng <code>dfsStr</code> được dùng chung trong tất cả các lời gọi đệ quy của <code>dfs</code>.</p>

<p>Bạn cần tìm mảng boolean <code>answer</code> có kích thước <code>n</code>. Với mỗi chỉ số <code>i</code> từ <code>0</code> đến <code>n - 1</code>, thực hiện:</p>

<ul>
    <li>Xóa chuỗi <code>dfsStr</code> rồi gọi <code>dfs(i)</code>.</li>
    <li>Nếu chuỗi <code>dfsStr</code> thu được là một <span data-keyword="palindrome-string">chuỗi đối xứng</span>, đặt <code>answer[i]</code> thành <code>true</code>. Ngược lại, đặt <code>answer[i]</code> thành <code>false</code>.</li>
</ul>

<p>Trả về mảng <code>answer</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3327.Check%20if%20DFS%20Strings%20Are%20Palindromes/images/tree1drawio.png" style="width: 240px; height: 256px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">parent = [-1,0,0,1,1,2], s = &quot;aababa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true,true,false,true,true,true]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Gọi <code>dfs(0)</code> thu được chuỗi <code>dfsStr = &quot;abaaba&quot;</code>, đây là một chuỗi đối xứng.</li>
    <li>Gọi <code>dfs(1)</code> thu được chuỗi <code>dfsStr = &quot;aba&quot;</code>, đây là một chuỗi đối xứng.</li>
    <li>Gọi <code>dfs(2)</code> thu được chuỗi <code>dfsStr = &quot;ab&quot;</code>, đây <strong>không</strong> phải là một chuỗi đối xứng.</li>
    <li>Gọi <code>dfs(3)</code> thu được chuỗi <code>dfsStr = &quot;a&quot;</code>, đây là một chuỗi đối xứng.</li>
    <li>Gọi <code>dfs(4)</code> thu được chuỗi <code>dfsStr = &quot;b&quot;</code>, đây là một chuỗi đối xứng.</li>
    <li>Gọi <code>dfs(5)</code> thu được chuỗi <code>dfsStr = &quot;a&quot;</code>, đây là một chuỗi đối xứng.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3327.Check%20if%20DFS%20Strings%20Are%20Palindromes/images/tree2drawio-1.png" style="width: 260px; height: 167px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">parent = [-1,0,0,0,0], s = &quot;aabcb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true,true,true,true,true]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mỗi lần gọi <code>dfs(x)</code> đều thu được một chuỗi đối xứng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>n == parent.length == s.length</code></li>
    <li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= parent[i] &lt;= n - 1</code> với mọi <code>i &gt;= 1</code>.</li>
    <li><code>parent[0] == -1</code></li>
    <li><code>parent</code> biểu diễn một cây hợp lệ.</li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Hash chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{dfsStr}$ của một cây con là một đoạn liên tiếp, và ta cần kiểm tra xem đoạn đó có đối xứng hay không. Với $n \le 10^5$, ta không thể duyệt lại chuỗi cho từng node.
>
> Một lần DFS sẽ ghi lại toàn bộ $\textit{dfsStr}$ của cây và lưu khoảng $[l, r]$ tương ứng với mỗi node.
>
> Hash của chuỗi và hash của chuỗi đảo ngược cho phép ta so sánh nửa đầu của $[l, r]$ với nửa tương ứng trong chuỗi đảo ngược trong $O(1)$, từ đó xác định chuỗi có đối xứng hay không.

<!-- thinking:end -->

Ta có thể dùng Depth-First Search (DFS) để duyệt cây và tạo toàn bộ $\textit{dfsStr}$, đồng thời xác định khoảng $[l, r]$ của từng node.

Sau đó, ta dùng hashing chuỗi để tính giá trị hash của cả $\textit{dfsStr}$ và chuỗi $\textit{dfsStr}$ đảo ngược, nhằm kiểm tra chuỗi có đối xứng hay không.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Hashing:
    __slots__ = ["mod", "h", "p"]

    def __init__(self, s: List[str], base: int, mod: int):
        self.mod = mod
        self.h = [0] * (len(s) + 1)
        self.p = [1] * (len(s) + 1)
        for i in range(1, len(s) + 1):
            self.h[i] = (self.h[i - 1] * base + ord(s[i - 1])) % mod
            self.p[i] = (self.p[i - 1] * base) % mod

    def query(self, l: int, r: int) -> int:
        return (self.h[r] - self.h[l - 1] * self.p[r - l + 1]) % self.mod


class Solution:
    def findAnswer(self, parent: List[int], s: str) -> List[bool]:
        def dfs(i: int):
            l = len(dfsStr) + 1
            for j in g[i]:
                dfs(j)
            dfsStr.append(s[i])
            r = len(dfsStr)
            pos[i] = (l, r)

        n = len(s)
        g = [[] for _ in range(n)]
        for i in range(1, n):
            g[parent[i]].append(i)
        dfsStr = []
        pos = {}
        dfs(0)

        base, mod = 13331, 998244353
        h1 = Hashing(dfsStr, base, mod)
        h2 = Hashing(dfsStr[::-1], base, mod)
        ans = []
        for i in range(n):
            l, r = pos[i]
            k = r - l + 1
            v1 = h1.query(l, l + k // 2 - 1)
            v2 = h2.query(n - r + 1, n - r + 1 + k // 2 - 1)
            ans.append(v1 == v2)
        return ans
```

#### Java

```java
class Hashing {
    private final long[] p;
    private final long[] h;
    private final long mod;

    public Hashing(String word, long base, int mod) {
        int n = word.length();
        p = new long[n + 1];
        h = new long[n + 1];
        p[0] = 1;
        this.mod = mod;
        for (int i = 1; i <= n; i++) {
            p[i] = p[i - 1] * base % mod;
            h[i] = (h[i - 1] * base + word.charAt(i - 1)) % mod;
        }
    }

    public long query(int l, int r) {
        return (h[r] - h[l - 1] * p[r - l + 1] % mod + mod) % mod;
    }
}

class Solution {
    private char[] s;
    private int[][] pos;
    private List<Integer>[] g;
    private StringBuilder dfsStr = new StringBuilder();

    public boolean[] findAnswer(int[] parent, String s) {
        this.s = s.toCharArray();
        int n = s.length();
        g = new List[n];
        pos = new int[n][0];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 1; i < n; ++i) {
            g[parent[i]].add(i);
        }
        dfs(0);
        final int base = 13331;
        final int mod = 998244353;
        Hashing h1 = new Hashing(dfsStr.toString(), base, mod);
        Hashing h2 = new Hashing(new StringBuilder(dfsStr).reverse().toString(), base, mod);
        boolean[] ans = new boolean[n];
        for (int i = 0; i < n; ++i) {
            int l = pos[i][0], r = pos[i][1];
            int k = r - l + 1;
            long v1 = h1.query(l, l + k / 2 - 1);
            long v2 = h2.query(n + 1 - r, n + 1 - r + k / 2 - 1);
            ans[i] = v1 == v2;
        }
        return ans;
    }

    private void dfs(int i) {
        int l = dfsStr.length() + 1;
        for (int j : g[i]) {
            dfs(j);
        }
        dfsStr.append(s[i]);
        int r = dfsStr.length();
        pos[i] = new int[] {l, r};
    }
}
```

#### C++

```cpp
class Hashing {
private:
    vector<long long> p;
    vector<long long> h;
    long long mod;

public:
    Hashing(string word, long long base, int mod) {
        int n = word.size();
        p.resize(n + 1);
        h.resize(n + 1);
        p[0] = 1;
        this->mod = mod;
        for (int i = 1; i <= n; i++) {
            p[i] = (p[i - 1] * base) % mod;
            h[i] = (h[i - 1] * base + word[i - 1] - 'a') % mod;
        }
    }

    long long query(int l, int r) {
        return (h[r] - h[l - 1] * p[r - l + 1] % mod + mod) % mod;
    }
};

class Solution {
public:
    vector<bool> findAnswer(vector<int>& parent, string s) {
        int n = s.size();
        vector<int> g[n];
        for (int i = 1; i < n; ++i) {
            g[parent[i]].push_back(i);
        }
        string dfsStr;
        vector<pair<int, int>> pos(n);
        auto dfs = [&](this auto&& dfs, int i) -> void {
            int l = dfsStr.size() + 1;
            for (int j : g[i]) {
                dfs(j);
            }
            dfsStr.push_back(s[i]);
            int r = dfsStr.size();
            pos[i] = {l, r};
        };
        dfs(0);

        const int base = 13331;
        const int mod = 998244353;
        Hashing h1(dfsStr, base, mod);
        reverse(dfsStr.begin(), dfsStr.end());
        Hashing h2(dfsStr, base, mod);
        vector<bool> ans(n);
        for (int i = 0; i < n; ++i) {
            auto [l, r] = pos[i];
            int k = r - l + 1;
            long long v1 = h1.query(l, l + k / 2 - 1);
            long long v2 = h2.query(n - r + 1, n - r + 1 + k / 2 - 1);
            ans[i] = v1 == v2;
        }
        return ans;
    }
};
```

#### Go

```go
type Hashing struct {
    p   []int64
    h   []int64
    mod int64
}

func NewHashing(word string, base, mod int64) *Hashing {
    n := len(word)
    p := make([]int64, n+1)
    h := make([]int64, n+1)
    p[0] = 1
    for i := 1; i <= n; i++ {
        p[i] = p[i-1] * base % mod
        h[i] = (h[i-1]*base + int64(word[i-1])) % mod
    }
    return &Hashing{p, h, mod}
}

func (hs *Hashing) query(l, r int) int64 {
    return (hs.h[r] - hs.h[l-1]*hs.p[r-l+1]%hs.mod + hs.mod) % hs.mod
}

func findAnswer(parent []int, s string) (ans []bool) {
    n := len(s)
    g := make([][]int, n)
    for i := 1; i < n; i++ {
        g[parent[i]] = append(g[parent[i]], i)
    }
    dfsStr := []byte{}
    pos := make([][2]int, n)
    var dfs func(int)
    dfs = func(i int) {
        l := len(dfsStr) + 1
        for _, j := range g[i] {
            dfs(j)
        }
        dfsStr = append(dfsStr, s[i])
        r := len(dfsStr)
        pos[i] = [2]int{l, r}
    }

    const base = 13331
    const mod = 998244353
    dfs(0)
    h1 := NewHashing(string(dfsStr), base, mod)
    for i, j := 0, len(dfsStr)-1; i < j; i, j = i+1, j-1 {
        dfsStr[i], dfsStr[j] = dfsStr[j], dfsStr[i]
    }
    h2 := NewHashing(string(dfsStr), base, mod)
    for i := 0; i < n; i++ {
        l, r := pos[i][0], pos[i][1]
        k := r - l + 1
        v1 := h1.query(l, l+k/2-1)
        v2 := h2.query(n-r+1, n-r+1+k/2-1)
        ans = append(ans, v1 == v2)
    }
    return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
