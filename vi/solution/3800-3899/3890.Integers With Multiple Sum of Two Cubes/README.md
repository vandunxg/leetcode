---
comments: true
difficulty: Medium
rating: 1534
source: Weekly Contest 496 Q2
tags:
    - Hash Table
    - Counting
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [3890. Integers With Multiple Sum of Two Cubes](https://leetcode.com/problems/integers-with-multiple-sum-of-two-cubes)

[中文文档](/solution/3800-3899/3890.Integers%20With%20Multiple%20Sum%20of%20Two%20Cubes/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>.</p>

<p>Một số nguyên <code>x</code> được gọi là <strong>tốt</strong> nếu tồn tại <strong>ít nhất</strong> hai cặp <code>(a, b)</code> <strong>khác nhau</strong> sao cho:</p>

<ul>
	<li><code>a</code> và <code>b</code> là các số nguyên dương.</li>
	<li><code>a &lt;= b</code></li>
	<li><code>x = a<sup>3</sup> + b<sup>3</sup></code></li>
</ul>

<p>Hãy trả về một mảng chứa tất cả các số nguyên tốt <strong>nhỏ hơn hoặc bằng</strong> <code>n</code>, được sắp xếp theo thứ tự tăng dần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4104</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1729,4104]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong các số nguyên nhỏ hơn hoặc bằng 4104, các số nguyên tốt là:</p>

<ul>
	<li>1729: <code>1<sup>3</sup> + 12<sup>3</sup> = 1729</code> và <code>9<sup>3</sup> + 10<sup>3</sup> = 1729</code>.</li>
	<li>4104: <code>2<sup>3</sup> + 16<sup>3</sup> = 4104</code> và <code>9<sup>3</sup> + 15<sup>3</sup> = 4104</code>.</li>
</ul>

<p>Do đó, đáp án là <code>[1729, 4104]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 578</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số nguyên tốt nào nhỏ hơn hoặc bằng 578, nên đáp án là một mảng rỗng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Một số nguyên tốt có ít nhất hai cách biểu diễn dưới dạng $a^3+b^3$ với $1 \le a \le b$. Ta cần tìm tất cả các giá trị $\le n$ như vậy, với $n \le 10^9$.
>
> Nếu $a$ hoặc $b$ lớn hơn $1000$ thì tổng đã vượt quá $10^9$, nên phạm vi tìm kiếm là hữu hạn và có thể được tiền xử lý.
>
> Một vòng lặp kép sẽ đếm số lần xuất hiện của mỗi tổng lập phương; các giá trị có nhiều hơn một cách biểu diễn sẽ được sắp xếp.
>
> Với mỗi truy vấn, ta dùng tìm kiếm nhị phân để tìm cận trên rồi trả về phần đầu tương ứng của mảng.

<!-- thinking:end -->

Ta nhận thấy khi $a$ hoặc $b$ lớn hơn $1000$, biểu thức $a^3 + b^3 > 10^9$. Do đó, ta chỉ cần liệt kê $1 \leq a \leq b \leq 1000$ và đếm số lần xuất hiện của mỗi số nguyên $x = a^3 + b^3$. Cuối cùng, ta lọc các số nguyên xuất hiện nhiều hơn một lần và sắp xếp chúng theo thứ tự tăng dần để thu được tất cả các số nguyên tốt.

Ta tiền xử lý tất cả các số nguyên tốt và lưu chúng trong một mảng $\textit{GOOD}$. Với mỗi truy vấn, ta dùng tìm kiếm nhị phân để tìm chỉ số $idx$ của số nguyên đầu tiên trong $\textit{GOOD}$ lớn hơn $n$, sau đó trả về $idx$ phần tử đầu tiên trong $\textit{GOOD}$.

Độ phức tạp thời gian là $O(m^2 + k \log k)$, trong đó $m = 1000$ là phạm vi liệt kê và $k$ là số lượng số nguyên tốt. Độ phức tạp không gian là $O(k)$.

<!-- tabs:start -->

#### Python3

```python
LIMIT = 10**9

cnt = defaultdict(int)
cubes = [i**3 for i in range(1001)]

for a in range(1, 1001):
    for b in range(a, 1001):
        x = cubes[a] + cubes[b]
        if x > LIMIT:
            break
        cnt[x] += 1

GOOD = sorted(x for x, v in cnt.items() if v > 1)


class Solution:
    def findGoodIntegers(self, n: int) -> list[int]:
        idx = bisect_right(GOOD, n)
        return GOOD[:idx]
```

#### Java

```java
class Solution {
    private static final int LIMIT = (int) 1e9;
    private static final List<Integer> GOOD = new ArrayList<>();

    static {
        Map<Integer, Integer> cnt = new HashMap<>();
        int[] cubes = new int[1001];
        for (int i = 0; i <= 1000; i++) {
            cubes[i] = i * i * i;
        }
        for (int a = 1; a <= 1000; a++) {
            for (int b = a; b <= 1000; b++) {
                int x = cubes[a] + cubes[b];
                if (x > LIMIT) {
                    break;
                }
                cnt.merge(x, 1, Integer::sum);
            }
        }
        for (Map.Entry<Integer, Integer> e : cnt.entrySet()) {
            if (e.getValue() > 1) {
                GOOD.add(e.getKey());
            }
        }

        Collections.sort(GOOD);
    }

    public List<Integer> findGoodIntegers(int n) {
        int idx = Collections.binarySearch(GOOD, n + 1);
        if (idx < 0) {
            idx = -idx - 1;
        }
        return GOOD.subList(0, idx);
    }
}
```

#### C++

```cpp
vector<int> GOOD;

auto init = [] {
    const int LIMIT = 1e9;

    unordered_map<int, int> cnt;
    vector<int> cubes(1001);

    for (int i = 0; i <= 1000; ++i) {
        cubes[i] = i * i * i;
    }

    for (int a = 1; a <= 1000; ++a) {
        for (int b = a; b <= 1000; ++b) {
            int x = cubes[a] + cubes[b];
            if (x > LIMIT) break;
            cnt[x]++;
        }
    }

    for (auto& [x, v] : cnt) {
        if (v > 1) {
            GOOD.push_back(x);
        }
    }

    sort(GOOD.begin(), GOOD.end());

    return 0;
}();

class Solution {
public:
    vector<int> findGoodIntegers(int n) {
        int idx = upper_bound(GOOD.begin(), GOOD.end(), n) - GOOD.begin();
        return vector<int>(GOOD.begin(), GOOD.begin() + idx);
    }
};
```

#### Go

```go
var GOOD []int

func init() {
	const LIMIT = 1e9

	cnt := make(map[int]int)
	cubes := make([]int, 1001)

	for i := 0; i <= 1000; i++ {
		cubes[i] = i * i * i
	}

	for a := 1; a <= 1000; a++ {
		for b := a; b <= 1000; b++ {
			x := cubes[a] + cubes[b]
			if x > LIMIT {
				break
			}
			cnt[x]++
		}
	}

	for x, v := range cnt {
		if v > 1 {
			GOOD = append(GOOD, x)
		}
	}

	sort.Ints(GOOD)
}

func findGoodIntegers(n int) []int {
	idx := sort.Search(len(GOOD), func(i int) bool {
		return GOOD[i] > n
	})
	return GOOD[:idx]
}
```

#### TypeScript

```ts
const LIMIT = 1e9;

const GOOD: number[] = (() => {
    const cnt = new Map<number, number>();
    const cubes: number[] = Array.from({ length: 1001 }, (_, i) => i * i * i);

    for (let a = 1; a <= 1000; a++) {
        for (let b = a; b <= 1000; b++) {
            const x = cubes[a] + cubes[b];
            if (x > LIMIT) break;
            cnt.set(x, (cnt.get(x) ?? 0) + 1);
        }
    }

    const res: number[] = [];
    for (const [x, v] of cnt.entries()) {
        if (v > 1) res.push(x);
    }

    res.sort((a, b) => a - b);
    return res;
})();

function findGoodIntegers(n: number): number[] {
    const idx = _.sortedLastIndex(GOOD, n);
    return GOOD.slice(0, idx);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
