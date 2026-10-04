---
comments: true
difficulty: Medium
rating: 1917
source: Biweekly Contest 130 Q3
tags:
    - Hash Table
    - String
    - Dynamic Programming
    - Counting
---

<!-- problem:start -->

# [3144. Minimum Substring Partition of Equal Character Frequency](https://leetcode.com/problems/minimum-substring-partition-of-equal-character-frequency)

[Tài liệu tiếng Trung](/solution/3100-3199/3144.Minimum%20Substring%20Partition%20of%20Equal%20Character%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, bạn cần phân hoạch chuỗi thành một hoặc nhiều <strong>cân bằng</strong> <span data-keyword="substring">chuỗi con</span>. Ví dụ, nếu <code>s == &quot;ababcc&quot;</code> thì <code>(&quot;abab&quot;, &quot;c&quot;, &quot;c&quot;)</code>, <code>(&quot;ab&quot;, &quot;abc&quot;, &quot;c&quot;)</code> và <code>(&quot;ababcc&quot;)</code> đều là các phân hoạch hợp lệ, nhưng <code>(&quot;a&quot;, <strong>&quot;bab&quot;</strong>, &quot;cc&quot;)</code>, <code>(<strong>&quot;aba&quot;</strong>, &quot;bc&quot;, &quot;c&quot;)</code> và <code>(&quot;ab&quot;, <strong>&quot;abcc&quot;</strong>)</code> thì không. Các chuỗi con không cân bằng được in đậm.</p>

<p>Trả về <strong>số lượng nhỏ nhất</strong> chuỗi con mà bạn có thể phân hoạch <code>s</code> thành.</p>

<p><strong>Lưu ý:</strong> Một chuỗi <strong>cân bằng</strong> là chuỗi trong đó mỗi ký tự xuất hiện cùng số lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;fabccddg&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể phân hoạch chuỗi <code>s</code> thành 3 chuỗi con theo một trong các cách sau: <code>(&quot;fab, &quot;ccdd&quot;, &quot;g&quot;)</code> hoặc <code>(&quot;fabc&quot;, &quot;cd&quot;, &quot;dg&quot;)</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abababaccddb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể phân hoạch chuỗi <code>s</code> thành 2 chuỗi con như sau: <code>(&quot;abab&quot;, &quot;abaccddb&quot;)</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi $s$ phải được chia thành ít mảnh cân bằng nhất có thể, trong đó tần suất của các ký tự trong mỗi mảnh bằng nhau. Duyệt mọi phân hoạch có số lượng tăng theo cấp số mũ; với $n\le 1000$, ta có thể dùng quy hoạch động bậc hai.
>
> Từ chỉ số $i$, khi mở rộng đến $j$, ta có thể kiểm tra tính cân bằng bằng các map tần suất: một mảnh hợp lệ khi chỉ còn một tần suất xuất hiện. Đáp án tối ưu chỉ phụ thuộc vào chỉ số bắt đầu.
>
> Ghi nhớ $dfs(i)$: duy trì $cnt$ và $freq$, khi $freq$ có kích thước $1$ thì chọn $1+dfs(j+1)$. Hậu tố rỗng trả về $0$.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}(i)$, biểu diễn số lượng chuỗi con nhỏ nhất bắt đầu từ $s[i]$. Đáp án là $\textit{dfs}(0)$.

Quá trình tính hàm $\textit{dfs}(i)$ như sau:

Nếu $i \geq n$, nghĩa là đã xử lý tất cả ký tự, ta trả về $0$.

Ngược lại, ta duy trì một hash table $\textit{cnt}$ để biểu diễn tần suất của mỗi ký tự trong chuỗi con hiện tại. Đồng thời, ta duy trì một hash table $\textit{freq}$ để biểu diễn số lượng ký tự có cùng số lần xuất hiện.

Sau đó, ta duyệt $j$ từ $i$ đến $n-1$, biểu diễn vị trí kết thúc của chuỗi con hiện tại. Với mỗi $j$, ta cập nhật $\textit{cnt}$ và $\textit{freq}$, rồi kiểm tra xem kích thước của $\textit{freq}$ có bằng $1$ hay không. Nếu có, ta có thể tách tại $j+1$, và đáp án là $1 + \textit{dfs}(j+1)$. Ta lấy đáp án nhỏ nhất trên mọi $j$ làm giá trị trả về của hàm.

Để tránh tính toán lặp lại, ta sử dụng tìm kiếm có ghi nhớ.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n \times |\Sigma|)$. Trong đó, $n$ là độ dài của chuỗi $s$, còn $|\Sigma|$ là kích thước của tập ký tự, bằng $26$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSubstringsInPartition(self, s: str) -> int:
        @cache
        def dfs(i: int) -> int:
            if i >= n:
                return 0
            cnt = defaultdict(int)
            freq = defaultdict(int)
            ans = n - i
            for j in range(i, n):
                if cnt[s[j]]:
                    freq[cnt[s[j]]] -= 1
                    if not freq[cnt[s[j]]]:
                        freq.pop(cnt[s[j]])
                cnt[s[j]] += 1
                freq[cnt[s[j]]] += 1
                if len(freq) == 1 and (t := 1 + dfs(j + 1)) < ans:
                    ans = t
            return ans

        n = len(s)
        return dfs(0)
```

#### Java

```java
class Solution {
    private int n;
    private char[] s;
    private Integer[] f;

    public int minimumSubstringsInPartition(String s) {
        n = s.length();
        f = new Integer[n];
        this.s = s.toCharArray();
        return dfs(0);
    }

    private int dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        int[] cnt = new int[26];
        Map<Integer, Integer> freq = new HashMap<>(26);
        int ans = n - i;
        for (int j = i; j < n; ++j) {
            int k = s[j] - 'a';
            if (cnt[k] > 0) {
                if (freq.merge(cnt[k], -1, Integer::sum) == 0) {
                    freq.remove(cnt[k]);
                }
            }
            ++cnt[k];
            freq.merge(cnt[k], 1, Integer::sum);
            if (freq.size() == 1) {
                ans = Math.min(ans, 1 + dfs(j + 1));
            }
        }
        return f[i] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSubstringsInPartition(string s) {
        int n = s.size();
        int f[n];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i) -> int {
            if (i >= n) {
                return 0;
            }
            if (f[i] != -1) {
                return f[i];
            }
            f[i] = n - i;
            int cnt[26]{};
            unordered_map<int, int> freq;
            for (int j = i; j < n; ++j) {
                int k = s[j] - 'a';
                if (cnt[k]) {
                    freq[cnt[k]]--;
                    if (freq[cnt[k]] == 0) {
                        freq.erase(cnt[k]);
                    }
                }
                ++cnt[k];
                ++freq[cnt[k]];
                if (freq.size() == 1) {
                    f[i] = min(f[i], 1 + dfs(j + 1));
                }
            }
            return f[i];
        };
        return dfs(0);
    }
};
```

#### Go

```go
func minimumSubstringsInPartition(s string) int {
	n := len(s)
	f := make([]int, n)
	for i := range f {
		f[i] = -1
	}
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if f[i] != -1 {
			return f[i]
		}
		cnt := [26]int{}
		freq := map[int]int{}
		f[i] = n - i
		for j := i; j < n; j++ {
			k := int(s[j] - 'a')
			if cnt[k] > 0 {
				freq[cnt[k]]--
				if freq[cnt[k]] == 0 {
					delete(freq, cnt[k])
				}
			}
			cnt[k]++
			freq[cnt[k]]++
			if len(freq) == 1 {
				f[i] = min(f[i], 1+dfs(j+1))
			}
		}
		return f[i]
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function minimumSubstringsInPartition(s: string): number {
    const n = s.length;
    const f: number[] = Array(n).fill(-1);
    const dfs = (i: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i] !== -1) {
            return f[i];
        }
        const cnt: Map<number, number> = new Map();
        const freq: Map<number, number> = new Map();
        f[i] = n - i;
        for (let j = i; j < n; ++j) {
            const k = s.charCodeAt(j) - 97;
            if (freq.has(cnt.get(k)!)) {
                freq.set(cnt.get(k)!, freq.get(cnt.get(k)!)! - 1);
                if (freq.get(cnt.get(k)!) === 0) {
                    freq.delete(cnt.get(k)!);
                }
            }
            cnt.set(k, (cnt.get(k) || 0) + 1);
            freq.set(cnt.get(k)!, (freq.get(cnt.get(k)!) || 0) + 1);
            if (freq.size === 1) {
                f[i] = Math.min(f[i], 1 + dfs(j + 1));
            }
        }
        return f[i];
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tìm kiếm có ghi nhớ (Tối ưu hóa)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duy trì thêm một map về số lần xuất hiện của các tần suất, làm tăng hằng số và khiến việc cập nhật trở nên rườm rà.
>
> Tính cân bằng tương đương với việc độ dài bằng (tần suất lớn nhất) nhân với (số lượng chữ cái phân biệt). Chỉ cần $cnt$ và giá trị lớn nhất $m$.
>
> Khi mở rộng $j$, cập nhật $m$ và đệ quy khi $j-i+1=m\cdot|cnt|$. Trạng thái vẫn là chỉ số bắt đầu, với vòng lặp bên trong ngắn gọn hơn.

<!-- thinking:end -->

Ta có thể tối ưu Lời giải 1 bằng cách không duy trì hash table $\textit{freq}$. Thay vào đó, ta chỉ cần duy trì hash table $\textit{cnt}$, biểu diễn tần suất của mỗi ký tự trong chuỗi con hiện tại. Ngoài ra, ta duy trì hai biến $k$ và $m$ lần lượt biểu diễn số lượng ký tự phân biệt trong chuỗi con hiện tại và tần suất lớn nhất của một ký tự. Với chuỗi con $s[i..j]$, nếu $j-i+1 = m \times k$ thì chuỗi con này là chuỗi con cân bằng.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n \times |\Sigma|)$. Trong đó, $n$ là độ dài của chuỗi $s$, còn $|\Sigma|$ là kích thước của tập ký tự, bằng $26$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSubstringsInPartition(self, s: str) -> int:
        @cache
        def dfs(i: int) -> int:
            if i >= n:
                return 0
            cnt = defaultdict(int)
            m = 0
            ans = n - i
            for j in range(i, n):
                cnt[s[j]] += 1
                m = max(m, cnt[s[j]])
                if j - i + 1 == m * len(cnt):
                    ans = min(ans, 1 + dfs(j + 1))
            return ans

        n = len(s)
        ans = dfs(0)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private char[] s;
    private Integer[] f;

    public int minimumSubstringsInPartition(String s) {
        n = s.length();
        f = new Integer[n];
        this.s = s.toCharArray();
        return dfs(0);
    }

    private int dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        int[] cnt = new int[26];
        int ans = n - i;
        int k = 0, m = 0;
        for (int j = i; j < n; ++j) {
            k += ++cnt[s[j] - 'a'] == 1 ? 1 : 0;
            m = Math.max(m, cnt[s[j] - 'a']);
            if (j - i + 1 == k * m) {
                ans = Math.min(ans, 1 + dfs(j + 1));
            }
        }
        return f[i] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSubstringsInPartition(string s) {
        int n = s.size();
        int f[n];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i) -> int {
            if (i >= n) {
                return 0;
            }
            if (f[i] != -1) {
                return f[i];
            }
            f[i] = n - i;
            int cnt[26]{};
            int k = 0, m = 0;
            for (int j = i; j < n; ++j) {
                k += ++cnt[s[j] - 'a'] == 1 ? 1 : 0;
                m = max(m, cnt[s[j] - 'a']);
                if (j - i + 1 == k * m) {
                    f[i] = min(f[i], 1 + dfs(j + 1));
                }
            }
            return f[i];
        };
        return dfs(0);
    }
};
```

#### Go

```go
func minimumSubstringsInPartition(s string) int {
	n := len(s)
	f := make([]int, n)
	for i := range f {
		f[i] = -1
	}
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if f[i] != -1 {
			return f[i]
		}
		cnt := [26]int{}
		f[i] = n - i
		k, m := 0, 0
		for j := i; j < n; j++ {
			x := int(s[j] - 'a')
			cnt[x]++
			if cnt[x] == 1 {
				k++
			}
			m = max(m, cnt[x])
			if j-i+1 == k*m {
				f[i] = min(f[i], 1+dfs(j+1))
			}
		}
		return f[i]
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function minimumSubstringsInPartition(s: string): number {
    const n = s.length;
    const f: number[] = Array(n).fill(-1);
    const dfs = (i: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i] !== -1) {
            return f[i];
        }
        const cnt: number[] = Array(26).fill(0);
        f[i] = n - i;
        let [k, m] = [0, 0];
        for (let j = i; j < n; ++j) {
            const x = s.charCodeAt(j) - 97;
            k += ++cnt[x] === 1 ? 1 : 0;
            m = Math.max(m, cnt[x]);
            if (j - i + 1 === k * m) {
                f[i] = Math.min(f[i], 1 + dfs(j + 1));
            }
        }
        return f[i];
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm có ghi nhớ vẫn phải chịu chi phí đệ quy và cache. Chuyển trạng thái tương tự không có hiệu ứng phụ thuộc về sau.
>
> Gọi $f[i]$ là số mảnh ít nhất đối với tiền tố có độ dài $i$. Với mỗi điểm kết thúc bên phải $i$, mở rộng sang trái đến $j$ và cập nhật $f[i+1]$ bằng $f[j]+1$ khi chuỗi con đó cân bằng.
>
> Bảng này cho kết quả $f[n]$, chỉ cần bộ nhớ quy hoạch động tuyến tính cùng một map đếm.

<!-- thinking:end -->

Ta có thể chuyển tìm kiếm có ghi nhớ thành quy hoạch động. Định nghĩa trạng thái $f[i]$ là số lượng chuỗi con nhỏ nhất cần dùng để phân hoạch $i$ ký tự đầu tiên. Ban đầu, $f[0] = 0$, còn các trạng thái còn lại là $f[i] = +\infty$ hoặc $f[i] = n$.

Tiếp theo, ta duyệt $i$ từ $0$ đến $n-1$. Với mỗi $i$, ta duy trì một hash table $\textit{cnt}$ để biểu diễn tần suất của mỗi ký tự trong chuỗi con hiện tại. Ngoài ra, ta duy trì hai biến $k$ và $m$ lần lượt biểu diễn số lượng ký tự phân biệt trong chuỗi con hiện tại và tần suất lớn nhất của một ký tự. Với chuỗi con $s[j..i]$, nếu $i-j+1 = m \times k$ thì chuỗi con này là chuỗi con cân bằng. Khi đó, ta có thể phân hoạch tại $j$, nên $f[i+1] = \min(f[i+1], f[j] + 1)$.

Đáp án cuối cùng là $f[n]$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n + |\Sigma|)$. Trong đó, $n$ là độ dài của chuỗi $s$, còn $|\Sigma|$ là kích thước của tập ký tự, bằng $26$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSubstringsInPartition(self, s: str) -> int:
        n = len(s)
        f = [inf] * (n + 1)
        f[0] = 0
        for i in range(n):
            cnt = defaultdict(int)
            m = 0
            for j in range(i, -1, -1):
                cnt[s[j]] += 1
                m = max(m, cnt[s[j]])
                if i - j + 1 == len(cnt) * m:
                    f[i + 1] = min(f[i + 1], f[j] + 1)
        return f[n]
```

#### Java

```java
class Solution {
    public int minimumSubstringsInPartition(String s) {
        int n = s.length();
        char[] cs = s.toCharArray();
        int[] f = new int[n + 1];
        Arrays.fill(f, n);
        f[0] = 0;
        for (int i = 0; i < n; ++i) {
            int[] cnt = new int[26];
            int k = 0, m = 0;
            for (int j = i; j >= 0; --j) {
                k += ++cnt[cs[j] - 'a'] == 1 ? 1 : 0;
                m = Math.max(m, cnt[cs[j] - 'a']);
                if (i - j + 1 == k * m) {
                    f[i + 1] = Math.min(f[i + 1], 1 + f[j]);
                }
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSubstringsInPartition(string s) {
        int n = s.size();
        vector<int> f(n + 1, n);
        f[0] = 0;
        for (int i = 0; i < n; ++i) {
            int cnt[26]{};
            int k = 0, m = 0;
            for (int j = i; ~j; --j) {
                k += ++cnt[s[j] - 'a'] == 1;
                m = max(m, cnt[s[j] - 'a']);
                if (i - j + 1 == k * m) {
                    f[i + 1] = min(f[i + 1], f[j] + 1);
                }
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func minimumSubstringsInPartition(s string) int {
	n := len(s)
	f := make([]int, n+1)
	for i := range f {
		f[i] = n
	}
	f[0] = 0
	for i := 0; i < n; i++ {
		cnt := [26]int{}
		k, m := 0, 0
		for j := i; j >= 0; j-- {
			x := int(s[j] - 'a')
			cnt[x]++
			if cnt[x] == 1 {
				k++
			}
			m = max(m, cnt[x])
			if i-j+1 == k*m {
				f[i+1] = min(f[i+1], 1+f[j])
			}
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
function minimumSubstringsInPartition(s: string): number {
    const n = s.length;
    const f: number[] = Array(n + 1).fill(n);
    f[0] = 0;
    for (let i = 0; i < n; ++i) {
        const cnt: number[] = Array(26).fill(0);
        let [k, m] = [0, 0];
        for (let j = i; ~j; --j) {
            const x = s.charCodeAt(j) - 97;
            k += ++cnt[x] === 1 ? 1 : 0;
            m = Math.max(m, cnt[x]);
            if (i - j + 1 === k * m) {
                f[i + 1] = Math.min(f[i + 1], 1 + f[j]);
            }
        }
    }
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
