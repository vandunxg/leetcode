---
comments: true
difficulty: Easy
rating: 1191
source: Biweekly Contest 191 Q1
---

<!-- problem:start -->

# [4048. Count Values With Equally Spaced Occurrences I](https://leetcode.com/problems/count-values-with-equally-spaced-occurrences-i)

[Tài liệu tiếng Trung](/solution/4000-4099/4048.Count%20Values%20With%20Equally%20Spaced%20Occurrences%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Một số nguyên <code>x</code> được gọi là <strong>đặc biệt</strong> nếu:</p>

<ul>
	<li><code>x</code> xuất hiện <strong>đúng ba</strong> lần trong <code>nums</code>.</li>
	<li><strong>Cả ba</strong> lần xuất hiện của <code>x</code> đều <strong>cách đều</strong> trong <code>nums</code>. Nói cách khác, nếu tất cả các lần xuất hiện của <code>x</code> nằm tại các chỉ số <code>i<sub>1</sub> &lt; i<sub>2</sub> &lt; i<sub>3</sub></code>, thì <code>i<sub>2</sub> - i<sub>1</sub> = i<sub>3</sub> - i<sub>2</sub></code>.</li>
</ul>

<p>Trả về số lượng số nguyên đặc biệt <strong>phân biệt</strong> trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,8,1,5,1,5,8,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>1 là số đặc biệt vì xuất hiện đúng ba lần tại các chỉ số cách đều 0, 2 và 4.</li>
	<li>5 là số đặc biệt vì xuất hiện đúng ba lần tại các chỉ số cách đều 3, 5 và 7.</li>
	<li>8 không phải số đặc biệt vì chỉ xuất hiện hai lần.</li>
</ul>

<p>Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [8,8,8,8]</span></p>

<p><strong>Đầu ra:</strong>&nbsp;0</p>

<p><strong>Giải thích:</strong></p>

<p>8 không phải số đặc biệt vì không xuất hiện đúng ba lần. Do đó, đáp án là 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [8,6,6,8,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>8 xuất hiện tại các chỉ số 0, 3 và 4, không cách đều nhau. 6 chỉ xuất hiện hai lần. Do đó, không có số nguyên nào là số đặc biệt.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 100$, nên ngay cả khi duyệt mảng một lần cho mỗi giá trị phân biệt thì vẫn đáp ứng được giới hạn. Một số nguyên đặc biệt phải xuất hiện đúng ba lần, và ba chỉ số đó phải lập thành một cấp số cộng.
>
> Sau khi thu thập các chỉ số của từng giá trị, việc kiểm tra được rút gọn còn hai điều: danh sách có độ dài $3$, và tổng của chỉ số đầu tiên với chỉ số cuối cùng bằng hai lần chỉ số ở giữa.
>
> Một hash table nhóm các chỉ số trong một lượt duyệt.

<!-- thinking:end -->

Sử dụng một hash table để ghi lại tất cả các chỉ số mà mỗi số nguyên xuất hiện. Duyệt qua $\textit{nums}$ và nối chỉ số $i$ vào danh sách của $\textit{nums}[i]$.

Sau đó, duyệt qua từng danh sách chỉ số $\textit{pos}$ trong hash table. Nếu $\textit{pos}$ có độ dài $3$ và $\textit{pos}[0] + \textit{pos}[2] = 2 \times \textit{pos}[1]$ (ba lần xuất hiện cách đều nhau), thì số nguyên đó là số đặc biệt và tăng đáp án thêm $1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSpecialIntegers(self, nums: list[int]) -> int:
        g = defaultdict(list)
        for i, x in enumerate(nums):
            g[x].append(i)
        return sum(
            len(pos) == 3 and pos[0] + pos[2] == pos[1] * 2 for pos in g.values()
        )
```

#### Java

```java
class Solution {
    public int countSpecialIntegers(int[] nums) {
        Map<Integer, List<Integer>> g = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            g.computeIfAbsent(nums[i], k -> new ArrayList<>()).add(i);
        }

        int ans = 0;
        for (List<Integer> pos : g.values()) {
            if (pos.size() == 3 && pos.get(0) + pos.get(2) == pos.get(1) * 2) {
                ans++;
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
    int countSpecialIntegers(vector<int>& nums) {
        unordered_map<int, vector<int>> g;
        for (int i = 0; i < nums.size(); i++) {
            g[nums[i]].push_back(i);
        }

        int ans = 0;
        for (auto& [x, pos] : g) {
            if (pos.size() == 3 && pos[0] + pos[2] == pos[1] * 2) {
                ans++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSpecialIntegers(nums []int) int {
	g := make(map[int][]int)
	for i, x := range nums {
		g[x] = append(g[x], i)
	}

	ans := 0
	for _, pos := range g {
		if len(pos) == 3 && pos[0]+pos[2] == pos[1]*2 {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countSpecialIntegers(nums: number[]): number {
    const g = new Map<number, number[]>();

    for (let i = 0; i < nums.length; i++) {
        if (!g.has(nums[i])) {
            g.set(nums[i], []);
        }
        g.get(nums[i])!.push(i);
    }

    let ans = 0;
    for (const pos of g.values()) {
        if (pos.length === 3 && pos[0] + pos[2] === pos[1] * 2) {
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
