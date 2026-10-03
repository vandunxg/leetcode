---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Brainteaser
    - Array
    - Math
    - Dynamic Programming
    - Game Theory
    - Nim Game
    - 'Sprague–Grundy '
    - Impartial Game
---

<!-- problem:start -->

# [1908. Game of Nim 🔒](https://leetcode.com/problems/game-of-nim)

[中文文档](/solution/1900-1999/1908.Game%20of%20Nim/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob lần lượt chơi một trò chơi, trong đó <strong>Alice đi trước</strong>.</p>

<p>Trong trò chơi này có <code>n</code> đống đá. Ở mỗi lượt, người chơi phải lấy đi <strong>một hoặc nhiều</strong> viên đá từ một đống không rỗng <strong>do mình chọn</strong>. Người đầu tiên không thể thực hiện nước đi sẽ thua, người còn lại sẽ thắng.</p>

<p>Cho một mảng số nguyên <code>piles</code>, trong đó <code>piles[i]</code> là số viên đá trong đống thứ <code>i<sup>th</sup></code>, hãy trả về <code>true</code><em> nếu Alice thắng hoặc </em><code>false</code><em> nếu Bob thắng</em>.</p>

<p>Cả Alice và Bob đều chơi <strong>tối ưu</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> piles = [1]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Chỉ có một kịch bản có thể xảy ra:
- Ở lượt đầu tiên, Alice lấy một viên đá từ đống thứ nhất. piles = [0].
- Ở lượt thứ hai, Bob không còn viên đá nào để lấy. Alice thắng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> piles = [1,1]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Có thể chứng minh Bob luôn thắng. Một kịch bản có thể xảy ra là:
- Ở lượt đầu tiên, Alice lấy một viên đá từ đống thứ nhất. piles = [0,1].
- Ở lượt thứ hai, Bob lấy một viên đá từ đống thứ hai. piles = [0,0].
- Ở lượt thứ ba, Alice không còn viên đá nào để lấy. Bob thắng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> piles = [1,2,3]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Có thể chứng minh Bob luôn thắng. Một kịch bản có thể xảy ra là:
- Ở lượt đầu tiên, Alice lấy ba viên đá từ đống thứ ba. piles = [1,2,0].
- Ở lượt thứ hai, Bob lấy một viên đá từ đống thứ hai. piles = [1,1,0].
- Ở lượt thứ ba, Alice lấy một viên đá từ đống thứ nhất. piles = [0,1,0].
- Ở lượt thứ tư, Bob lấy một viên đá từ đống thứ hai. piles = [0,0,0].
- Ở lượt thứ năm, Alice không còn viên đá nào để lấy. Bob thắng.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == piles.length</code></li>
	<li><code>1 &lt;= n &lt;= 7</code></li>
	<li><code>1 &lt;= piles[i] &lt;= 7</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể tìm lời giải chạy trong thời gian tuyến tính không? Dù lời giải thời gian tuyến tính có thể nằm ngoài phạm vi của một buổi phỏng vấn, đây vẫn là một điều thú vị để tìm hiểu.</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Nim kinh điển dùng phép xor, nhưng ở đây có nhiều nhất $7$ đống, mỗi đống có kích thước không quá $7$, nên chỉ có $7^7$ trạng thái. Tìm kiếm trên đồ thị trạng thái là đủ.
>
> Một trạng thái là trạng thái thắng khi có một nước đi đưa nó đến trạng thái thua. Ta ghi nhớ kết quả theo tuple kích thước các đống và thử trừ từ $1\ldots x$ viên ở một đống.
>
> Nếu mọi trạng thái kế tiếp đều là trạng thái thắng, trạng thái hiện tại là trạng thái thua. Đáp án chính là việc kiểm tra tính chất đó trên tuple đầu vào.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nimGame(self, piles: List[int]) -> bool:
        @cache
        def dfs(st):
            lst = list(st)
            for i, x in enumerate(lst):
                for j in range(1, x + 1):
                    lst[i] -= j
                    if not dfs(tuple(lst)):
                        return True
                    lst[i] += j
            return False

        return dfs(tuple(piles))
```

#### Java

```java
class Solution {
    private Map<Integer, Boolean> memo = new HashMap<>();
    private int[] p = new int[8];

    public Solution() {
        p[0] = 1;
        for (int i = 1; i < 8; ++i) {
            p[i] = p[i - 1] * 8;
        }
    }

    public boolean nimGame(int[] piles) {
        return dfs(piles);
    }

    private boolean dfs(int[] piles) {
        int st = f(piles);
        if (memo.containsKey(st)) {
            return memo.get(st);
        }
        for (int i = 0; i < piles.length; ++i) {
            for (int j = 1; j <= piles[i]; ++j) {
                piles[i] -= j;
                if (!dfs(piles)) {
                    piles[i] += j;
                    memo.put(st, true);
                    return true;
                }
                piles[i] += j;
            }
        }
        memo.put(st, false);
        return false;
    }

    private int f(int[] piles) {
        int st = 0;
        for (int i = 0; i < piles.length; ++i) {
            st += piles[i] * p[i];
        }
        return st;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool nimGame(vector<int>& piles) {
        unordered_map<int, int> memo;
        int p[8] = {1};
        for (int i = 1; i < 8; ++i) {
            p[i] = p[i - 1] * 8;
        }
        auto f = [&](vector<int>& piles) {
            int st = 0;
            for (int i = 0; i < piles.size(); ++i) {
                st += piles[i] * p[i];
            }
            return st;
        };
        function<bool(vector<int>&)> dfs = [&](vector<int>& piles) {
            int st = f(piles);
            if (memo.count(st)) {
                return memo[st];
            }
            for (int i = 0; i < piles.size(); ++i) {
                for (int j = 1; j <= piles[i]; ++j) {
                    piles[i] -= j;
                    if (!dfs(piles)) {
                        piles[i] += j;
                        return memo[st] = true;
                    }
                    piles[i] += j;
                }
            }
            return memo[st] = false;
        };
        return dfs(piles);
    }
};
```

#### Go

```go
func nimGame(piles []int) bool {
	memo := map[int]bool{}
	p := make([]int, 8)
	p[0] = 1
	for i := 1; i < 8; i++ {
		p[i] = p[i-1] * 8
	}
	f := func(piles []int) int {
		st := 0
		for i, x := range piles {
			st += x * p[i]
		}
		return st
	}
	var dfs func(piles []int) bool
	dfs = func(piles []int) bool {
		st := f(piles)
		if v, ok := memo[st]; ok {
			return v
		}
		for i, x := range piles {
			for j := 1; j <= x; j++ {
				piles[i] -= j
				if !dfs(piles) {
					piles[i] += j
					memo[st] = true
					return true
				}
				piles[i] += j
			}
		}
		memo[st] = false
		return false
	}
	return dfs(piles)
}
```

#### TypeScript

```ts
function nimGame(piles: number[]): boolean {
    const p: number[] = Array(8).fill(1);
    for (let i = 1; i < 8; ++i) {
        p[i] = p[i - 1] * 8;
    }
    const f = (piles: number[]): number => {
        let st = 0;
        for (let i = 0; i < piles.length; ++i) {
            st += piles[i] * p[i];
        }
        return st;
    };
    const memo: Map<number, boolean> = new Map();
    const dfs = (piles: number[]): boolean => {
        const st = f(piles);
        if (memo.has(st)) {
            return memo.get(st)!;
        }
        for (let i = 0; i < piles.length; ++i) {
            for (let j = 1; j <= piles[i]; ++j) {
                piles[i] -= j;
                if (!dfs(piles)) {
                    piles[i] += j;
                    memo.set(st, true);
                    return true;
                }
                piles[i] += j;
            }
        }
        memo.set(st, false);
        return false;
    };
    return dfs(piles);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
