---
comments: true
difficulty: Medium
rating: 1740
source: Weekly Contest 140 Q2
tags:
    - Hash Table
    - String
    - Backtracking
    - Counting
---

<!-- problem:start -->

# [1079. Letter Tile Possibilities](https://leetcode.com/problems/letter-tile-possibilities)

[中文文档](/solution/1000-1099/1079.Letter%20Tile%20Possibilities/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code>&nbsp;&nbsp;<code>tiles</code>, mỗi viên có in một chữ cái <code>tiles[i]</code>.</p>

<p>Trả về <em>số chuỗi chữ cái không rỗng có thể tạo được</em> bằng các chữ cái in trên những <code>tiles</code> đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> tiles = &quot;AAB&quot;
<strong>Output:</strong> 8
<strong>Giải thích: </strong>Các chuỗi có thể tạo được là &quot;A&quot;, &quot;B&quot;, &quot;AA&quot;, &quot;AB&quot;, &quot;BA&quot;, &quot;AAB&quot;, &quot;ABA&quot;, &quot;BAA&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> tiles = &quot;AAABBC&quot;
<strong>Output:</strong> 188
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> tiles = &quot;V&quot;
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= tiles.length &lt;= 7</code></li>
	<li><code>tiles</code> chỉ gồm các chữ cái tiếng Anh viết hoa.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các chuỗi khác nhau là những hoán vị của một multiset. Có thể duyệt vì $n\le 7$, nhưng nếu hoán vị các vị trí thì những chữ cái trùng nhau sẽ bị đếm lặp. Ta nên đệ quy dựa trên số lượng chữ cái còn lại.
>
> $\textit{dfs}(\textit{cnt})$ thử từng chữ cái còn lại, dùng một chữ cái đó và tính lựa chọn này là một chuỗi không rỗng trước khi tiếp tục đệ quy.
>
> Khi quay lui, khôi phục số lượng chữ cái; lời gọi ban đầu dùng tần suất các chữ cái trong bộ tiles.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numTilePossibilities(self, tiles: str) -> int:
        def dfs(cnt: Counter) -> int:
            ans = 0
            for i, x in cnt.items():
                if x > 0:
                    ans += 1
                    cnt[i] -= 1
                    ans += dfs(cnt)
                    cnt[i] += 1
            return ans

        cnt = Counter(tiles)
        return dfs(cnt)
```

#### Java

```java
class Solution {
    public int numTilePossibilities(String tiles) {
        int[] cnt = new int[26];
        for (char c : tiles.toCharArray()) {
            ++cnt[c - 'A'];
        }
        return dfs(cnt);
    }

    private int dfs(int[] cnt) {
        int res = 0;
        for (int i = 0; i < cnt.length; ++i) {
            if (cnt[i] > 0) {
                ++res;
                --cnt[i];
                res += dfs(cnt);
                ++cnt[i];
            }
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numTilePossibilities(string tiles) {
        int cnt[26]{};
        for (char c : tiles) {
            ++cnt[c - 'A'];
        }
        function<int(int* cnt)> dfs = [&](int* cnt) -> int {
            int res = 0;
            for (int i = 0; i < 26; ++i) {
                if (cnt[i] > 0) {
                    ++res;
                    --cnt[i];
                    res += dfs(cnt);
                    ++cnt[i];
                }
            }
            return res;
        };
        return dfs(cnt);
    }
};
```

#### Go

```go
func numTilePossibilities(tiles string) int {
	cnt := [26]int{}
	for _, c := range tiles {
		cnt[c-'A']++
	}
	var dfs func(cnt [26]int) int
	dfs = func(cnt [26]int) (res int) {
		for i, x := range cnt {
			if x > 0 {
				res++
				cnt[i]--
				res += dfs(cnt)
				cnt[i]++
			}
		}
		return
	}
	return dfs(cnt)
}
```

#### TypeScript

```ts
function numTilePossibilities(tiles: string): number {
    const cnt: number[] = new Array(26).fill(0);
    for (const c of tiles) {
        ++cnt[c.charCodeAt(0) - 'A'.charCodeAt(0)];
    }
    const dfs = (cnt: number[]): number => {
        let res = 0;
        for (let i = 0; i < 26; ++i) {
            if (cnt[i] > 0) {
                ++res;
                --cnt[i];
                res += dfs(cnt);
                ++cnt[i];
            }
        }
        return res;
    };
    return dfs(cnt);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
