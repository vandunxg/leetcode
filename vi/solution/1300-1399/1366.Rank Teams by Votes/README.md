---
comments: true
difficulty: Medium
rating: 1626
source: Weekly Contest 178 Q2
tags:
    - Array
    - Hash Table
    - String
    - Counting
    - Sorting
---

<!-- problem:start -->

# [1366. Rank Teams by Votes](https://leetcode.com/problems/rank-teams-by-votes)

[中文文档](/solution/1300-1399/1366.Rank%20Teams%20by%20Votes/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một hệ thống xếp hạng đặc biệt, mỗi cử tri xếp hạng tất cả các đội tham gia cuộc thi từ cao nhất đến thấp nhất.</p>

<p>Thứ tự các đội được quyết định dựa trên số phiếu ở vị trí thứ nhất. Nếu có từ hai đội trở lên hòa ở vị trí này, ta xét vị trí thứ hai để phân định; nếu vẫn hòa thì tiếp tục xét các vị trí tiếp theo cho đến khi xác định được thứ hạng. Nếu sau khi xét tất cả vị trí vẫn hòa, ta xếp các đội theo thứ tự alphabet của ký tự đại diện cho đội.</p>

<p>Cho mảng chuỗi <code>votes</code> chứa phiếu bầu của tất cả cử tri trong hệ thống xếp hạng. Hãy sắp xếp các đội theo quy tắc trên.</p>

<p>Trả về <em>chuỗi chứa tất cả các đội được <strong>sắp xếp</strong> theo hệ thống xếp hạng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> votes = [&quot;ABC&quot;,&quot;ACB&quot;,&quot;ABC&quot;,&quot;ACB&quot;,&quot;ACB&quot;]
<strong>Output:</strong> &quot;ACB&quot;
<strong>Giải thích:</strong> 
Đội A được 5 cử tri xếp ở vị trí thứ nhất. Không đội nào khác nhận phiếu ở vị trí thứ nhất, nên A đứng đầu.
Đội B được 2 cử tri xếp thứ hai và 3 cử tri xếp thứ ba.
Đội C được 3 cử tri xếp thứ hai và 2 cử tri xếp thứ ba.
Vì nhiều cử tri xếp C ở vị trí thứ hai hơn, đội C đứng thứ hai và đội B đứng thứ ba.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> votes = [&quot;WXYZ&quot;,&quot;XYZW&quot;]
<strong>Output:</strong> &quot;XWYZ&quot;
<strong>Giải thích:</strong>
X thắng nhờ quy tắc phân định hòa. X và W có cùng số phiếu ở vị trí thứ nhất, nhưng X có một phiếu ở vị trí thứ hai còn W không có phiếu nào ở vị trí này. 
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> votes = [&quot;ZMNAGUEDSJYLBOPHRQICWFXTVK&quot;]
<strong>Output:</strong> &quot;ZMNAGUEDSJYLBOPHRQICWFXTVK&quot;
<strong>Giải thích:</strong> Chỉ có một cử tri nên thứ hạng được lấy theo phiếu của người đó.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= votes.length &lt;= 1000</code></li>
	<li><code>1 &lt;= votes[i].length &lt;= 26</code></li>
	<li><code>votes[i].length == votes[j].length</code> for <code>0 &lt;= i, j &lt; votes.length</code>.</li>
	<li><code>votes[i][j]</code> là một chữ cái tiếng Anh <strong>viết hoa</strong>.</li>
	<li>Các ký tự trong <code>votes[i]</code> đều khác nhau.</li>
	<li>Tất cả ký tự xuất hiện trong <code>votes[0]</code> cũng <strong>xuất hiện</strong> trong <code>votes[j]</code> với <code>1 &lt;= j &lt; votes.length</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> Các đội được xếp hạng theo số phiếu ở từng vị trí, sau đó theo ký tự. So sánh từng cặp đội trên mọi phiếu tốn $O(m^2 n)$. Thay vào đó, ta đếm vector số phiếu theo vị trí của mỗi đội, rồi sắp xếp theo vector này và ký tự đảo dấu để vector lớn hơn đứng trước.

<!-- thinking:end -->

Với mỗi đội, ta đếm số phiếu nhận được ở từng thứ hạng, rồi lần lượt so sánh số phiếu ở các thứ hạng. Nếu số phiếu bằng nhau, ta so sánh ký tự đại diện cho đội.

Độ phức tạp thời gian là $O(n \times m + m^2 \times \log m)$ và độ phức tạp không gian là $O(m^2)$. Trong đó, $n$ là độ dài của $\textit{votes}$, còn $m$ là số đội, tức độ dài của $\textit{votes}[0]$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rankTeams(self, votes: List[str]) -> str:
        m = len(votes[0])
        cnt = defaultdict(lambda: [0] * m)
        for vote in votes:
            for i, c in enumerate(vote):
                cnt[c][i] += 1
        return "".join(sorted(cnt, key=lambda c: (cnt[c], -ord(c)), reverse=True))
```

#### Java

```java
class Solution {
    public String rankTeams(String[] votes) {
        int m = votes[0].length();
        int[][] cnt = new int[26][m + 1];
        for (var vote : votes) {
            for (int i = 0; i < m; ++i) {
                ++cnt[vote.charAt(i) - 'A'][i];
            }
        }
        Character[] s = new Character[m];
        for (int i = 0; i < m; ++i) {
            s[i] = votes[0].charAt(i);
        }
        Arrays.sort(s, (a, b) -> {
            int i = a - 'A', j = b - 'A';
            for (int k = 0; k < m; ++k) {
                if (cnt[i][k] != cnt[j][k]) {
                    return cnt[j][k] - cnt[i][k];
                }
            }
            return a - b;
        });
        StringBuilder ans = new StringBuilder();
        for (var c : s) {
            ans.append(c);
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string rankTeams(vector<string>& votes) {
        int m = votes[0].size();
        array<array<int, 27>, 26> cnt{};

        for (const auto& vote : votes) {
            for (int i = 0; i < m; ++i) {
                ++cnt[vote[i] - 'A'][i];
            }
        }
        string s = votes[0];
        ranges::sort(s, [&](char a, char b) {
            int i = a - 'A', j = b - 'A';
            for (int k = 0; k < m; ++k) {
                if (cnt[i][k] != cnt[j][k]) {
                    return cnt[i][k] > cnt[j][k];
                }
            }
            return a < b;
        });
        return string(s.begin(), s.end());
    }
};
```

#### Go

```go
func rankTeams(votes []string) string {
	m := len(votes[0])
	cnt := [26][27]int{}
	for _, vote := range votes {
		for i, ch := range vote {
			cnt[ch-'A'][i]++
		}
	}
	s := []rune(votes[0])
	sort.Slice(s, func(i, j int) bool {
		a, b := s[i]-'A', s[j]-'A'
		for k := 0; k < m; k++ {
			if cnt[a][k] != cnt[b][k] {
				return cnt[a][k] > cnt[b][k]
			}
		}
		return s[i] < s[j]
	})
	return string(s)
}
```

#### TypeScript

```ts
function rankTeams(votes: string[]): string {
    const m = votes[0].length;
    const cnt: number[][] = Array.from({ length: 26 }, () => Array(m + 1).fill(0));
    for (const vote of votes) {
        for (let i = 0; i < m; i++) {
            cnt[vote.charCodeAt(i) - 65][i]++;
        }
    }
    const s: string[] = votes[0].split('');
    s.sort((a, b) => {
        const i = a.charCodeAt(0) - 65;
        const j = b.charCodeAt(0) - 65;
        for (let k = 0; k < m; k++) {
            if (cnt[i][k] !== cnt[j][k]) {
                return cnt[j][k] - cnt[i][k];
            }
        }
        return a.localeCompare(b);
    });
    return s.join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn rank_teams(votes: Vec<String>) -> String {
        let m = votes[0].len();
        let mut cnt = vec![vec![0; m + 1]; 26];

        for vote in &votes {
            for (i, ch) in vote.chars().enumerate() {
                cnt[(ch as u8 - b'A') as usize][i] += 1;
            }
        }

        let mut s: Vec<char> = votes[0].chars().collect();

        s.sort_by(|&a, &b| {
            let i = (a as u8 - b'A') as usize;
            let j = (b as u8 - b'A') as usize;

            for k in 0..m {
                if cnt[i][k] != cnt[j][k] {
                    return cnt[j][k].cmp(&cnt[i][k]);
                }
            }
            a.cmp(&b)
        });

        s.into_iter().collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
