---
comments: true
difficulty: Medium
rating: 1316
source: Weekly Contest 287 Q2
tags:
    - Array
    - Hash Table
    - Counting
    - Sorting
---

<!-- problem:start -->

# [2225. Find Players With Zero or One Losses](https://leetcode.com/problems/find-players-with-zero-or-one-losses)

[中文文档](/solution/2200-2299/2225.Find%20Players%20With%20Zero%20or%20One%20Losses/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>matches</code>, trong đó <code>matches[i] = [winner<sub>i</sub>, loser<sub>i</sub>]</code> cho biết người chơi <code>winner<sub>i</sub></code> đã đánh bại người chơi <code>loser<sub>i</sub></code> trong một trận đấu.</p>

<p>Trả về <em>một danh sách </em><code>answer</code><em> có kích thước </em><code>2</code><em>, trong đó:</em></p>

<ul>
	<li><code>answer[0]</code> là danh sách tất cả người chơi <strong>chưa</strong> từng thua trận nào.</li>
	<li><code>answer[1]</code> là danh sách tất cả người chơi đã thua đúng <strong>một</strong> trận.</li>
</ul>

<p>Các giá trị trong hai danh sách phải được trả về theo thứ tự <strong>tăng dần</strong>.</p>

<p><strong>Ghi chú:</strong></p>

<ul>
	<li>Chỉ xét những người chơi đã tham gia <strong>ít nhất một</strong> trận đấu.</li>
	<li>Dữ liệu kiểm thử được tạo sao cho <strong>không</strong> có hai trận đấu nào có <strong>cùng</strong> kết quả.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> matches = [[1,3],[2,3],[3,6],[5,6],[5,7],[4,5],[4,8],[4,9],[10,4],[10,9]]
<strong>Đầu ra:</strong> [[1,2,10],[4,5,7,8]]
<strong>Giải thích:</strong>
Người chơi 1, 2 và 10 chưa từng thua trận nào.
Người chơi 4, 5, 7 và 8 mỗi người đã thua một trận.
Người chơi 3, 6 và 9 mỗi người đã thua hai trận.
Do đó, answer[0] = [1,2,10] và answer[1] = [4,5,7,8].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> matches = [[2,3],[1,3],[5,4],[6,4]]
<strong>Đầu ra:</strong> [[1,2,5,6],[]]
<strong>Giải thích:</strong>
Người chơi 1, 2, 5 và 6 chưa từng thua trận nào.
Người chơi 3 và 4 mỗi người đã thua hai trận.
Do đó, answer[0] = [1,2,5,6] và answer[1] = [].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= matches.length &lt;= 10<sup>5</sup></code></li>
	<li><code>matches[i].length == 2</code></li>
	<li><code>1 &lt;= winner<sub>i</sub>, loser<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>winner<sub>i</sub> != loser<sub>i</sub></code></li>
	<li>Tất cả <code>matches[i]</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Cần liệt kê những người chơi chưa từng thua và những người chỉ thua đúng một lần, đồng thời sắp xếp mỗi danh sách theo thứ tự tăng dần. Có tối đa $10^5$ trận đấu, nên việc duyệt qua mọi mã người chơi là lãng phí. Chỉ những người chơi xuất hiện trong các trận đấu mới đáng quan tâm, và thông tin duy nhất cần biết là số trận đã thua.
>
> Một hash map $\textit{cnt}$ lưu số trận thua: người thắng được thêm vào với giá trị $0$ nếu chưa xuất hiện, còn số trận thua của người thua được tăng lên. Sau khi sắp xếp theo mã người chơi, những người có $0$ hoặc $1$ trận thua được đưa vào hai danh sách kết quả.

<!-- thinking:end -->

Ta sử dụng một hash table `cnt` để ghi nhận số trận mà mỗi người chơi đã thua.

Sau đó, ta duyệt qua hash table, đưa những người chơi thua 0 trận vào `ans[0]`, và đưa những người chơi thua 1 trận vào `ans[1]`.

Cuối cùng, ta sắp xếp `ans[0]` và `ans[1]` theo thứ tự tăng dần rồi trả về kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số trận đấu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findWinners(self, matches: List[List[int]]) -> List[List[int]]:
        cnt = Counter()
        for winner, loser in matches:
            if winner not in cnt:
                cnt[winner] = 0
            cnt[loser] += 1
        ans = [[], []]
        for x, v in sorted(cnt.items()):
            if v < 2:
                ans[v].append(x)
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> findWinners(int[][] matches) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (var e : matches) {
            cnt.putIfAbsent(e[0], 0);
            cnt.merge(e[1], 1, Integer::sum);
        }
        List<List<Integer>> ans = List.of(new ArrayList<>(), new ArrayList<>());
        for (var e : cnt.entrySet()) {
            if (e.getValue() < 2) {
                ans.get(e.getValue()).add(e.getKey());
            }
        }
        Collections.sort(ans.get(0));
        Collections.sort(ans.get(1));
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> findWinners(vector<vector<int>>& matches) {
        map<int, int> cnt;
        for (auto& e : matches) {
            if (!cnt.contains(e[0])) {
                cnt[e[0]] = 0;
            }
            ++cnt[e[1]];
        }
        vector<vector<int>> ans(2);
        for (auto& [x, v] : cnt) {
            if (v < 2) {
                ans[v].push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findWinners(matches [][]int) [][]int {
	cnt := map[int]int{}
	for _, e := range matches {
		if _, ok := cnt[e[0]]; !ok {
			cnt[e[0]] = 0
		}
		cnt[e[1]]++
	}
	ans := make([][]int, 2)
	for x, v := range cnt {
		if v < 2 {
			ans[v] = append(ans[v], x)
		}
	}
	sort.Ints(ans[0])
	sort.Ints(ans[1])
	return ans
}
```

#### TypeScript

```ts
function findWinners(matches: number[][]): number[][] {
    const cnt: Map<number, number> = new Map();
    for (const [winner, loser] of matches) {
        if (!cnt.has(winner)) {
            cnt.set(winner, 0);
        }
        cnt.set(loser, (cnt.get(loser) || 0) + 1);
    }
    const ans: number[][] = [[], []];
    for (const [x, v] of cnt) {
        if (v < 2) {
            ans[v].push(x);
        }
    }
    ans[0].sort((a, b) => a - b);
    ans[1].sort((a, b) => a - b);
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} matches
 * @return {number[][]}
 */
var findWinners = function (matches) {
    const cnt = new Map();
    for (const [winner, loser] of matches) {
        if (!cnt.has(winner)) {
            cnt.set(winner, 0);
        }
        cnt.set(loser, (cnt.get(loser) || 0) + 1);
    }
    const ans = [[], []];
    for (const [x, v] of cnt) {
        if (v < 2) {
            ans[v].push(x);
        }
    }
    ans[0].sort((a, b) => a - b);
    ans[1].sort((a, b) => a - b);
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
