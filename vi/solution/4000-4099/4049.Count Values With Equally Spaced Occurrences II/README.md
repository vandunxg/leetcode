---
comments: true
difficulty: Medium
rating: 1405
source: Biweekly Contest 191 Q2
---

<!-- problem:start -->

# [4049. Count Values With Equally Spaced Occurrences II](https://leetcode.com/problems/count-values-with-equally-spaced-occurrences-ii)

[中文文档](/solution/4000-4099/4049.Count%20Values%20With%20Equally%20Spaced%20Occurrences%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một số nguyên <code>x</code> được gọi là <strong>đặc biệt</strong> nếu:</p>

<ul>
	<li><code>x</code> xuất hiện <strong>ít nhất ba lần</strong> trong <code>nums</code>.</li>
	<li><strong>Mọi</strong> lần xuất hiện của <code>x</code> đều <strong>cách đều nhau</strong> trong <code>nums</code>. Nói cách khác, nếu mọi lần xuất hiện của <code>x</code> nằm tại các chỉ số <code>i<sub>1</sub> &lt; i<sub>2</sub> &lt; ... &lt; i<sub>m</sub></code>, thì <code>i<sub>2</sub> - i<sub>1</sub> = i<sub>3</sub> - i<sub>2</sub> = ... = i<sub>m</sub> - i<sub>m-1</sub></code>.</li>
</ul>

<p>Hãy trả về số lượng số nguyên đặc biệt <strong>phân biệt</strong> trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,8,1,5,1,5,8,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>1 là số đặc biệt vì xuất hiện tại các chỉ số cách đều 0, 2 và 4.</li>
	<li>5 là số đặc biệt vì xuất hiện tại các chỉ số cách đều 3, 5 và 7.</li>
	<li>8 không phải số đặc biệt vì chỉ xuất hiện hai lần.</li>
</ul>

<p>Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [8,8,8,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>8 là số đặc biệt vì xuất hiện tại các chỉ số cách đều 0, 1, 2 và 3. Do đó, đáp án là 1.</p>
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
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Bài trước chỉ xử lý đúng ba lần xuất hiện. Ở đây một giá trị phải xuất hiện ít nhất ba lần, và mọi lần xuất hiện phải nằm trên cùng một cấp số cộng. Với $n = 10^5$, ta không thể quét lại mảng ban đầu cho từng giá trị.
>
> Sau khi nhóm các chỉ số theo giá trị, tổng độ dài các danh sách vẫn là $n$. Nếu mọi khoảng cách giữa hai chỉ số kề nhau đều bằng khoảng cách đầu tiên, toàn bộ dãy là một cấp số cộng.
>
> Chỉ cần nhóm các chỉ số rồi quét tuyến tính từng danh sách.

<!-- thinking:end -->

Ta dùng một bảng băm để ghi lại tất cả các chỉ số mà mỗi số nguyên xuất hiện. Duyệt qua $\textit{nums}$ và thêm chỉ số $i$ vào danh sách của $\textit{nums}[i]$.

Sau đó, duyệt qua từng danh sách chỉ số $\textit{pos}$ trong bảng băm. Bỏ qua danh sách nếu độ dài nhỏ hơn $3$. Ngược lại, đặt $d = \textit{pos}[1] - \textit{pos}[0]$ và kiểm tra xem mọi khoảng cách giữa hai phần tử kề nhau có bằng $d$ hay không. Nếu có, số nguyên đó là số đặc biệt và ta tăng đáp án thêm $1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSpecialIntegers(self, nums: list[int]) -> int:
        g = defaultdict(list)
        for i, x in enumerate(nums):
            g[x].append(i)
        ans = 0
        for pos in g.values():
            if len(pos) < 3:
                continue
            d = pos[1] - pos[0]
            if all(j - i == d for i, j in pairwise(pos)):
                ans += 1
        return ans
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
            if (pos.size() < 3) {
                continue;
            }

            int d = pos.get(1) - pos.get(0);
            boolean ok = true;
            for (int i = 1; i < pos.size(); i++) {
                if (pos.get(i) - pos.get(i - 1) != d) {
                    ok = false;
                    break;
                }
            }

            if (ok) {
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
            if (pos.size() < 3) {
                continue;
            }

            int d = pos[1] - pos[0];
            bool ok = true;
            for (int i = 1; i < pos.size(); i++) {
                if (pos[i] - pos[i - 1] != d) {
                    ok = false;
                    break;
                }
            }

            if (ok) {
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
		if len(pos) < 3 {
			continue
		}

		d := pos[1] - pos[0]
		ok := true
		for i := 1; i < len(pos); i++ {
			if pos[i]-pos[i-1] != d {
				ok = false
				break
			}
		}

		if ok {
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
        if (pos.length < 3) {
            continue;
        }

        const d = pos[1] - pos[0];
        let ok = true;
        for (let i = 1; i < pos.length; i++) {
            if (pos[i] - pos[i - 1] !== d) {
                ok = false;
                break;
            }
        }

        if (ok) {
            ans++;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
