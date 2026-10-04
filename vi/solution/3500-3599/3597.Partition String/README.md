---
comments: true
difficulty: Medium
rating: 1347
source: Weekly Contest 456 Q1
tags:
    - Trie
    - Hash Table
    - String
    - Simulation
---

<!-- problem:start -->

# [3597. Partition String](https://leetcode.com/problems/partition-string)

[中文文档](/solution/3500-3599/3597.Partition%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, hãy phân hoạch chuỗi thành các <strong>segment không trùng lặp</strong> theo quy trình sau:</p>

<ul>
	<li>Bắt đầu xây dựng một segment tại chỉ số 0.</li>
	<li>Tiếp tục mở rộng segment hiện tại từng ký tự một cho đến khi segment hiện tại chưa từng xuất hiện trước đó.</li>
	<li>Khi segment là duy nhất, thêm nó vào danh sách các segment, đánh dấu nó là đã gặp và bắt đầu một segment mới từ chỉ số tiếp theo.</li>
	<li>Lặp lại cho đến khi đi đến cuối <code>s</code>.</li>
</ul>

<p>Trả về một mảng chuỗi <code>segments</code>, trong đó <code>segments[i]</code> là segment thứ <code>i<sup>th</sup></code> được tạo ra.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abbccccd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;a&quot;,&quot;b&quot;,&quot;bc&quot;,&quot;c&quot;,&quot;cc&quot;,&quot;d&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Chỉ số</th>
			<th style="border: 1px solid black;">Segment sau khi thêm</th>
			<th style="border: 1px solid black;">Các segment đã gặp</th>
			<th style="border: 1px solid black;">Segment hiện tại đã xuất hiện trước đó?</th>
			<th style="border: 1px solid black;">Segment mới</th>
			<th style="border: 1px solid black;">Các segment đã gặp sau khi cập nhật</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">&quot;a&quot;</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">Không</td>
			<td style="border: 1px solid black;">&quot;&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;]</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">&quot;b&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;]</td>
			<td style="border: 1px solid black;">Không</td>
			<td style="border: 1px solid black;">&quot;&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;]</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">&quot;b&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">&quot;b&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;]</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">&quot;bc&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;]</td>
			<td style="border: 1px solid black;">Không</td>
			<td style="border: 1px solid black;">&quot;&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;, &quot;bc&quot;]</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">&quot;c&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;, &quot;bc&quot;]</td>
			<td style="border: 1px solid black;">Không</td>
			<td style="border: 1px solid black;">&quot;&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;, &quot;bc&quot;, &quot;c&quot;]</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">&quot;c&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;, &quot;bc&quot;, &quot;c&quot;]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">&quot;c&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;, &quot;bc&quot;, &quot;c&quot;]</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">&quot;cc&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;, &quot;bc&quot;, &quot;c&quot;]</td>
			<td style="border: 1px solid black;">Không</td>
			<td style="border: 1px solid black;">&quot;&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;, &quot;bc&quot;, &quot;c&quot;, &quot;cc&quot;]</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">&quot;d&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;, &quot;bc&quot;, &quot;c&quot;, &quot;cc&quot;]</td>
			<td style="border: 1px solid black;">Không</td>
			<td style="border: 1px solid black;">&quot;&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;b&quot;, &quot;bc&quot;, &quot;c&quot;, &quot;cc&quot;, &quot;d&quot;]</td>
		</tr>
	</tbody>
</table>

<p>Do đó, kết quả cuối cùng là <code>[&quot;a&quot;, &quot;b&quot;, &quot;bc&quot;, &quot;c&quot;, &quot;cc&quot;, &quot;d&quot;]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aaaa&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;a&quot;,&quot;aa&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Chỉ số</th>
			<th style="border: 1px solid black;">Segment sau khi thêm</th>
			<th style="border: 1px solid black;">Các segment đã gặp</th>
			<th style="border: 1px solid black;">Segment hiện tại đã xuất hiện trước đó?</th>
			<th style="border: 1px solid black;">Segment mới</th>
			<th style="border: 1px solid black;">Các segment đã gặp sau khi cập nhật</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">&quot;a&quot;</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">Không</td>
			<td style="border: 1px solid black;">&quot;&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;]</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">&quot;a&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">&quot;a&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;]</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">&quot;aa&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;]</td>
			<td style="border: 1px solid black;">Không</td>
			<td style="border: 1px solid black;">&quot;&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;aa&quot;]</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">&quot;a&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;aa&quot;]</td>
			<td style="border: 1px solid black;">Có</td>
			<td style="border: 1px solid black;">&quot;a&quot;</td>
			<td style="border: 1px solid black;">[&quot;a&quot;, &quot;aa&quot;]</td>
		</tr>
	</tbody>
</table>

<p>Do đó, kết quả cuối cùng là <code>[&quot;a&quot;, &quot;aa&quot;]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường. </li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các segment phải là những chuỗi ngắn nhất chưa xuất hiện, theo thứ tự từ trái sang phải. Ta lưu các phần đã tạo vào một set; sau mỗi lần thêm một ký tự, tạo segment và xóa buffer khi segment đó là mới.
>
> Độ dài các segment tăng theo dạng $1+2+\cdots$, nên tổng chi phí tra cứu khoảng $O(n\sqrt{n})$, phù hợp với giới hạn đề bài.

<!-- thinking:end -->

Ta có thể dùng một hash table $\textit{vis}$ để ghi lại các segment đã xuất hiện. Sau đó, ta duyệt chuỗi $s$, xây dựng segment hiện tại $t$ từng ký tự một cho đến khi segment này chưa xuất hiện trước đó. Mỗi khi tạo được một segment mới, ta thêm nó vào danh sách kết quả và đánh dấu là đã gặp.

Sau khi duyệt xong, ta chỉ cần trả về danh sách kết quả.

Độ phức tạp thời gian là $O(n \times \sqrt{n})$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def partitionString(self, s: str) -> List[str]:
        vis = set()
        ans = []
        t = ""
        for c in s:
            t += c
            if t not in vis:
                vis.add(t)
                ans.append(t)
                t = ""
        return ans
```

#### Java

```java
class Solution {
    public List<String> partitionString(String s) {
        Set<String> vis = new HashSet<>();
        List<String> ans = new ArrayList<>();
        String t = "";
        for (char c : s.toCharArray()) {
            t += c;
            if (vis.add(t)) {
                ans.add(t);
                t = "";
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
    vector<string> partitionString(string s) {
        unordered_set<string> vis;
        vector<string> ans;
        string t = "";
        for (char c : s) {
            t += c;
            if (!vis.contains(t)) {
                vis.insert(t);
                ans.push_back(t);
                t = "";
            }
        }
        return ans;
    }
};
```

#### Go

```go
func partitionString(s string) (ans []string) {
	vis := make(map[string]bool)
	t := ""
	for _, c := range s {
		t += string(c)
		if !vis[t] {
			vis[t] = true
			ans = append(ans, t)
			t = ""
		}
	}
	return
}
```

#### TypeScript

```ts
function partitionString(s: string): string[] {
    const vis = new Set<string>();
    const ans: string[] = [];
    let t = '';
    for (const c of s) {
        t += c;
        if (!vis.has(t)) {
            vis.add(t);
            ans.push(t);
            t = '';
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Băm chuỗi + Bảng băm + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp trước dùng toàn bộ chuỗi con làm khóa của set, nên chi phí so sánh tăng theo độ dài segment. Hash chuỗi đa thức cho phép truy vấn chuỗi con trong $O(1)$ và giữ chi phí tra cứu trung bình là hằng số.
>
> Hai con trỏ đánh dấu segment hiện tại $[l,r]$; ta mở rộng $r$ và cắt khi hash là mới. Cách phân hoạch tham lam không thay đổi, nhưng tổng thời gian giảm xuống tuyến tính.

<!-- thinking:end -->

Ta có thể dùng kỹ thuật băm chuỗi để tăng tốc việc tra cứu các segment. Cụ thể, ta tính giá trị hash cho mỗi segment và lưu nó trong một hash table. Nhờ đó, ta có thể xác định trong thời gian hằng số xem một segment đã xuất hiện hay chưa.

Cụ thể, trước tiên ta tạo một lớp băm chuỗi $\textit{Hashing}$ dựa trên chuỗi $s$, hỗ trợ tính giá trị hash của bất kỳ chuỗi con nào. Sau đó, ta duyệt chuỗi $s$ bằng hai con trỏ $l$ và $r$ để biểu diễn vị trí bắt đầu và kết thúc của segment hiện tại (các chỉ số bắt đầu từ $1$). Mỗi khi mở rộng $r$, ta tính giá trị hash $x$ của segment hiện tại. Nếu giá trị hash này chưa có trong hash table, ta thêm segment vào danh sách kết quả và đánh dấu giá trị hash là đã gặp. Ngược lại, ta tiếp tục mở rộng $r$ cho đến khi tìm được một segment mới.

Sau khi duyệt xong, ta trả về danh sách kết quả.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Hashing:
    __slots__ = ["mod", "h", "p"]

    def __init__(
        self, s: Union[str, List[str]], base: int = 13331, mod: int = 998244353
    ):
        self.mod = mod
        self.h = [0] * (len(s) + 1)
        self.p = [1] * (len(s) + 1)
        for i in range(1, len(s) + 1):
            self.h[i] = (self.h[i - 1] * base + ord(s[i - 1])) % mod
            self.p[i] = (self.p[i - 1] * base) % mod

    def query(self, l: int, r: int) -> int:
        return (self.h[r] - self.h[l - 1] * self.p[r - l + 1]) % self.mod


class Solution:
    def partitionString(self, s: str) -> List[str]:
        hashing = Hashing(s)
        vis = set()
        l = 1
        ans = []
        for r, c in enumerate(s, 1):
            x = hashing.query(l, r)
            if x not in vis:
                vis.add(x)
                ans.append(s[l - 1 : r])
                l = r + 1
        return ans
```

#### Java

```java
class Hashing {
    private final long[] p;
    private final long[] h;
    private final long mod;

    public Hashing(String word) {
        this(word, 13331, 998244353);
    }

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
    public List<String> partitionString(String s) {
        Hashing hashing = new Hashing(s);
        Set<Long> vis = new HashSet<>();
        List<String> ans = new ArrayList<>();
        for (int l = 1, r = 1; r <= s.length(); ++r) {
            long x = hashing.query(l, r);
            if (vis.add(x)) {
                ans.add(s.substring(l - 1, r));
                l = r + 1;
            }
        }
        return ans;
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
    Hashing(const string& word, long long base = 13331, long long mod = 998244353) {
        int n = word.size();
        p.resize(n + 1);
        h.resize(n + 1);
        p[0] = 1;
        this->mod = mod;
        for (int i = 1; i <= n; i++) {
            p[i] = (p[i - 1] * base) % mod;
            h[i] = (h[i - 1] * base + word[i - 1]) % mod;
        }
    }

    long long query(int l, int r) const {
        return (h[r] - h[l - 1] * p[r - l + 1] % mod + mod) % mod;
    }
};

class Solution {
public:
    vector<string> partitionString(const string& s) {
        Hashing hashing(s);
        unordered_set<long long> vis;
        vector<string> ans;
        int l = 1;
        for (int r = 1; r <= (int) s.size(); ++r) {
            long long x = hashing.query(l, r);
            if (!vis.contains(x)) {
                vis.insert(x);
                ans.push_back(s.substr(l - 1, r - l + 1));
                l = r + 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
type Hashing struct {
	p, h []int64
	mod  int64
}

func NewHashing(s string, base, mod int64) *Hashing {
	n := len(s)
	p := make([]int64, n+1)
	h := make([]int64, n+1)
	p[0] = 1
	for i := 1; i <= n; i++ {
		p[i] = p[i-1] * base % mod
		h[i] = (h[i-1]*base + int64(s[i-1])) % mod
	}
	return &Hashing{p, h, mod}
}

func (hs *Hashing) Query(l, r int) int64 {
	return (hs.h[r] - hs.h[l-1]*hs.p[r-l+1]%hs.mod + hs.mod) % hs.mod
}

func partitionString(s string) (ans []string) {
	n := len(s)
	hashing := NewHashing(s, 13331, 998244353)
	vis := make(map[int64]bool)
	l := 1
	for r := 1; r <= n; r++ {
		x := hashing.Query(l, r)
		if !vis[x] {
			vis[x] = true
			ans = append(ans, s[l-1:r])
			l = r + 1
		}
	}
	return
}
```

#### TypeScript

```ts
class Hashing {
    private p: bigint[];
    private h: bigint[];
    private mod: bigint;

    constructor(s: string, base: bigint = 13331n, mod: bigint = 998244353n) {
        const n = s.length;
        this.mod = mod;
        this.p = new Array<bigint>(n + 1).fill(1n);
        this.h = new Array<bigint>(n + 1).fill(0n);
        for (let i = 1; i <= n; i++) {
            this.p[i] = (this.p[i - 1] * base) % mod;
            this.h[i] = (this.h[i - 1] * base + BigInt(s.charCodeAt(i - 1))) % mod;
        }
    }

    query(l: number, r: number): bigint {
        return (this.h[r] - ((this.h[l - 1] * this.p[r - l + 1]) % this.mod) + this.mod) % this.mod;
    }
}

function partitionString(s: string): string[] {
    const n = s.length;
    const hashing = new Hashing(s);
    const vis = new Set<string>();
    const ans: string[] = [];
    let l = 1;
    for (let r = 1; r <= n; r++) {
        const x = hashing.query(l, r).toString();
        if (!vis.has(x)) {
            vis.add(x);
            ans.push(s.slice(l - 1, r));
            l = r + 1;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
