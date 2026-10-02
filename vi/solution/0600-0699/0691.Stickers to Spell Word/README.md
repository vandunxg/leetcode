---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Memoization
    - Array
    - Hash Table
    - String
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [691. Stickers to Spell Word](https://leetcode.com/problems/stickers-to-spell-word)

[中文文档](/solution/0600-0699/0691.Stickers%20to%20Spell%20Word/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>n</code> loại <code>stickers</code> khác nhau. Mỗi sticker có in một từ tiếng Anh viết thường.</p>

<p>Bạn muốn tạo chuỗi <code>target</code> bằng cách cắt từng chữ cái từ các sticker mình có rồi sắp xếp lại. Bạn có thể dùng mỗi sticker nhiều lần tùy ý và có số lượng vô hạn cho từng loại.</p>

<p>Hãy trả về <em>số sticker ít nhất cần dùng để tạo thành </em><code>target</code>. Nếu không thể thực hiện, trả về <code>-1</code>.</p>

<p><strong>Lưu ý:</strong> Trong tất cả test case, các từ được chọn ngẫu nhiên từ <code>1000</code> từ tiếng Anh Mỹ phổ biến nhất, còn <code>target</code> được tạo bằng cách nối hai từ ngẫu nhiên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stickers = [&quot;with&quot;,&quot;example&quot;,&quot;science&quot;], target = &quot;thehat&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Ta có thể dùng 2 sticker &quot;with&quot; và 1 sticker &quot;example&quot;.
Sau khi cắt và sắp xếp lại các chữ cái trên những sticker đó, ta tạo được target &quot;thehat&quot;.
Đây cũng là số sticker ít nhất cần dùng để tạo target.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stickers = [&quot;notice&quot;,&quot;possible&quot;], target = &quot;basicbasic&quot;
<strong>Đầu ra:</strong> -1
Giải thích:
Không thể tạo target &quot;basicbasic&quot; bằng cách cắt chữ cái từ các sticker đã cho.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == stickers.length</code></li>
	<li><code>1 &lt;= n &lt;= 50</code></li>
	<li><code>1 &lt;= stickers[i].length &lt;= 10</code></li>
	<li><code>1 &lt;= target.length &lt;= 15</code></li>
	<li><code>stickers[i]</code> và <code>target</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS + Nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> Độ dài $\textit{target}$ tối đa là $15$ và có thể dùng lại sticker. Nếu tìm kiếm theo số lượng sticker, ta sẽ gặp lại những tập ký tự đã được ghép giống nhau.
>
> Dùng bit mask để đánh dấu các chữ cái trong $\textit{target}$ đã ghép được. BFS bắt đầu từ trạng thái $0$; mỗi sticker sẽ bật thêm nhiều bit còn thiếu nhất có thể. Lần đầu đạt mask đầy đủ chính là số sticker ít nhất.

<!-- thinking:end -->

Ta nhận thấy độ dài chuỗi `target` không vượt quá 15. Có thể dùng một số nhị phân dài 15 bit để biểu diễn trạng thái từng ký tự trong `target`. Nếu bit thứ $i$ là 1 thì ký tự thứ $i$ đã được ghép; ngược lại, ký tự đó chưa được ghép.

Ta định nghĩa trạng thái ban đầu là 0, nghĩa là chưa ghép được ký tự nào. Sau đó, dùng Breadth-First Search (BFS) bắt đầu từ trạng thái này. Ở mỗi bước, ta duyệt tất cả sticker. Với mỗi sticker, thử ghép từng ký tự của `target`. Nếu ghép được một ký tự, đặt bit thứ $i$ của số nhị phân tương ứng thành 1 để đánh dấu ký tự đó đã được ghép. Tiếp tục tìm kiếm cho đến khi ghép được toàn bộ ký tự trong `target`.

Độ phức tạp thời gian là $O(2^n \times m \times (l + n))$, còn độ phức tạp không gian là $O(2^n)$. Trong đó, $n$ là độ dài chuỗi `target`, còn $m$ và $l$ lần lượt là số lượng sticker và độ dài trung bình của mỗi sticker.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minStickers(self, stickers: List[str], target: str) -> int:
        n = len(target)
        q = deque([0])
        vis = [False] * (1 << n)
        vis[0] = True
        ans = 0
        while q:
            for _ in range(len(q)):
                cur = q.popleft()
                if cur == (1 << n) - 1:
                    return ans
                for s in stickers:
                    cnt = Counter(s)
                    nxt = cur
                    for i, c in enumerate(target):
                        if (cur >> i & 1) == 0 and cnt[c] > 0:
                            cnt[c] -= 1
                            nxt |= 1 << i
                    if not vis[nxt]:
                        vis[nxt] = True
                        q.append(nxt)
            ans += 1
        return -1
```

#### Java

```java
class Solution {
    public int minStickers(String[] stickers, String target) {
        int n = target.length();
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        boolean[] vis = new boolean[1 << n];
        vis[0] = true;
        for (int ans = 0; !q.isEmpty(); ++ans) {
            for (int m = q.size(); m > 0; --m) {
                int cur = q.poll();
                if (cur == (1 << n) - 1) {
                    return ans;
                }
                for (String s : stickers) {
                    int[] cnt = new int[26];
                    int nxt = cur;
                    for (char c : s.toCharArray()) {
                        ++cnt[c - 'a'];
                    }
                    for (int i = 0; i < n; ++i) {
                        int j = target.charAt(i) - 'a';
                        if ((cur >> i & 1) == 0 && cnt[j] > 0) {
                            --cnt[j];
                            nxt |= 1 << i;
                        }
                    }
                    if (!vis[nxt]) {
                        vis[nxt] = true;
                        q.offer(nxt);
                    }
                }
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minStickers(vector<string>& stickers, string target) {
        int n = target.size();
        queue<int> q{{0}};
        vector<bool> vis(1 << n);
        vis[0] = true;
        for (int ans = 0; q.size(); ++ans) {
            for (int m = q.size(); m; --m) {
                int cur = q.front();
                q.pop();
                if (cur == (1 << n) - 1) {
                    return ans;
                }
                for (auto& s : stickers) {
                    int cnt[26]{};
                    int nxt = cur;
                    for (char& c : s) {
                        ++cnt[c - 'a'];
                    }
                    for (int i = 0; i < n; ++i) {
                        int j = target[i] - 'a';
                        if ((cur >> i & 1) == 0 && cnt[j] > 0) {
                            nxt |= 1 << i;
                            --cnt[j];
                        }
                    }
                    if (!vis[nxt]) {
                        vis[nxt] = true;
                        q.push(nxt);
                    }
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minStickers(stickers []string, target string) (ans int) {
	n := len(target)
	q := []int{0}
	vis := make([]bool, 1<<n)
	vis[0] = true
	for ; len(q) > 0; ans++ {
		for m := len(q); m > 0; m-- {
			cur := q[0]
			q = q[1:]
			if cur == 1<<n-1 {
				return
			}
			for _, s := range stickers {
				cnt := [26]int{}
				for _, c := range s {
					cnt[c-'a']++
				}
				nxt := cur
				for i, c := range target {
					if cur>>i&1 == 0 && cnt[c-'a'] > 0 {
						nxt |= 1 << i
						cnt[c-'a']--
					}
				}
				if !vis[nxt] {
					vis[nxt] = true
					q = append(q, nxt)
				}
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minStickers(stickers: string[], target: string): number {
    const n = target.length;
    const q: number[] = [0];
    const vis: boolean[] = Array(1 << n).fill(false);
    vis[0] = true;
    for (let ans = 0; q.length; ++ans) {
        const qq: number[] = [];
        for (const cur of q) {
            if (cur === (1 << n) - 1) {
                return ans;
            }
            for (const s of stickers) {
                const cnt: number[] = Array(26).fill(0);
                for (const c of s) {
                    cnt[c.charCodeAt(0) - 97]++;
                }
                let nxt = cur;
                for (let i = 0; i < n; ++i) {
                    const j = target.charCodeAt(i) - 97;
                    if (((cur >> i) & 1) === 0 && cnt[j]) {
                        nxt |= 1 << i;
                        cnt[j]--;
                    }
                }
                if (!vis[nxt]) {
                    vis[nxt] = true;
                    qq.push(nxt);
                }
            }
        }
        q.splice(0, q.length, ...qq);
    }
    return -1;
}
```

#### Rust

```rust
use std::collections::{HashSet, VecDeque};

impl Solution {
    pub fn min_stickers(stickers: Vec<String>, target: String) -> i32 {
        let mut q = VecDeque::new();
        q.push_back(0);
        let mut ans = 0;
        let n = target.len();
        let mut vis = HashSet::new();
        vis.insert(0);
        while !q.is_empty() {
            for _ in 0..q.len() {
                let state = q.pop_front().unwrap();
                if state == (1 << n) - 1 {
                    return ans;
                }
                for s in &stickers {
                    let mut nxt = state;
                    let mut cnt = [0; 26];
                    for &c in s.as_bytes() {
                        cnt[(c - b'a') as usize] += 1;
                    }
                    for (i, &c) in target.as_bytes().iter().enumerate() {
                        let idx = (c - b'a') as usize;
                        if (nxt & (1 << i)) == 0 && cnt[idx] > 0 {
                            nxt |= 1 << i;
                            cnt[idx] -= 1;
                        }
                    }
                    if !vis.contains(&nxt) {
                        q.push_back(nxt);
                        vis.insert(nxt);
                    }
                }
            }
            ans += 1;
        }
        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
