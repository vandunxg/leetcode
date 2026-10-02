---
comments: true
difficulty: Medium
rating: 1562
source: Weekly Contest 189 Q3
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [1452. People Whose List of Favorite Companies Is Not a Subset of Another List](https://leetcode.com/problems/people-whose-list-of-favorite-companies-is-not-a-subset-of-another-list)

[中文文档](/solution/1400-1499/1452.People%20Whose%20List%20of%20Favorite%20Companies%20Is%20Not%20a%20Subset%20of%20Another%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>favoriteCompanies</code>, trong đó <code>favoriteCompanies[i]</code> là danh sách các công ty yêu thích của người thứ <code>i</code> (<strong>được đánh chỉ số từ 0</strong>).</p>

<p><em>Trả về chỉ số của những người có danh sách các công ty yêu thích không phải là <strong>tập con</strong> của bất kỳ danh sách các công ty yêu thích nào khác</em>. Bạn phải trả về các chỉ số theo thứ tự tăng dần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> favoriteCompanies = [[&quot;leetcode&quot;,&quot;google&quot;,&quot;facebook&quot;],[&quot;google&quot;,&quot;microsoft&quot;],[&quot;google&quot;,&quot;facebook&quot;],[&quot;google&quot;],[&quot;amazon&quot;]]
<strong>Đầu ra:</strong> [0,1,4]
<strong>Giải thích:</strong>
Người có index=2 có favoriteCompanies[2]=[&quot;google&quot;,&quot;facebook&quot;], là tập con của favoriteCompanies[0]=[&quot;leetcode&quot;,&quot;google&quot;,&quot;facebook&quot;], tương ứng với người có index 0.
Người có index=3 có favoriteCompanies[3]=[&quot;google&quot;], là tập con của favoriteCompanies[0]=[&quot;leetcode&quot;,&quot;google&quot;,&quot;facebook&quot;] và favoriteCompanies[1]=[&quot;google&quot;,&quot;microsoft&quot;].
Các danh sách công ty yêu thích khác không phải là tập con của danh sách nào khác, do đó đáp án là [0,1,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> favoriteCompanies = [[&quot;leetcode&quot;,&quot;google&quot;,&quot;facebook&quot;],[&quot;leetcode&quot;,&quot;amazon&quot;],[&quot;facebook&quot;,&quot;google&quot;]]
<strong>Đầu ra:</strong> [0,1]
<strong>Giải thích:</strong> Trong trường hợp này, favoriteCompanies[2]=[&quot;facebook&quot;,&quot;google&quot;] là tập con của favoriteCompanies[0]=[&quot;leetcode&quot;,&quot;google&quot;,&quot;facebook&quot;], do đó đáp án là [0,1].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> favoriteCompanies = [[&quot;leetcode&quot;],[&quot;google&quot;],[&quot;facebook&quot;],[&quot;amazon&quot;]]
<strong>Đầu ra:</strong> [0,1,2,3]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= favoriteCompanies.length &lt;= 100</code></li>
	<li><code>1 &lt;= favoriteCompanies[i].length &lt;= 500</code></li>
	<li><code>1 &lt;= favoriteCompanies[i][j].length &lt;= 20</code></li>
	<li>Tất cả các chuỗi trong <code>favoriteCompanies[i]</code> đều <strong>khác nhau</strong>.</li>
	<li>Tất cả các danh sách công ty yêu thích đều <strong>khác nhau</strong>, nghĩa là nếu sắp xếp mỗi danh sách theo thứ tự alphabet thì <code>favoriteCompanies[i] != favoriteCompanies[j]</code>.</li>
	<li>Tất cả các chuỗi chỉ bao gồm chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 100$ và mỗi danh sách có nhiều nhất $500$ tên. Ánh xạ tên công ty sang các số nguyên, lưu mỗi danh sách dưới dạng một set, rồi kiểm tra quan hệ tập con bằng một vòng lặp lồng nhau.
>
> Các danh sách là duy nhất, nên người $i$ được giữ lại nếu không tồn tại $j\neq i$ nào thỏa mãn $\textit{nums}[i]\subseteq\textit{nums}[j]$.

<!-- thinking:end -->

Ta có thể ánh xạ mỗi công ty với một số nguyên duy nhất. Sau đó, với mỗi người, ta chuyển các công ty yêu thích của họ thành một set các số nguyên. Cuối cùng, ta kiểm tra xem các công ty yêu thích của một người có phải là tập con của các công ty yêu thích của người khác hay không.

Độ phức tạp thời gian là $(n \times m \times k + n^2 \times m)$, và độ phức tạp không gian là $O(n \times m)$. Trong đó, $n$ và $m$ lần lượt là độ dài của `favoriteCompanies` và độ dài trung bình danh sách của mỗi công ty, còn $k$ là độ dài trung bình của mỗi công ty.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def peopleIndexes(self, favoriteCompanies: List[List[str]]) -> List[int]:
        idx = 0
        d = {}
        n = len(favoriteCompanies)
        nums = [set() for _ in range(n)]
        for i, ss in enumerate(favoriteCompanies):
            for s in ss:
                if s not in d:
                    d[s] = idx
                    idx += 1
                nums[i].add(d[s])
        ans = []
        for i in range(n):
            if not any(i != j and (nums[i] & nums[j]) == nums[i] for j in range(n)):
                ans.append(i)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> peopleIndexes(List<List<String>> favoriteCompanies) {
        int n = favoriteCompanies.size();
        Map<String, Integer> d = new HashMap<>();
        int idx = 0;
        Set<Integer>[] nums = new Set[n];
        Arrays.setAll(nums, i -> new HashSet<>());
        for (int i = 0; i < n; ++i) {
            var ss = favoriteCompanies.get(i);
            for (var s : ss) {
                if (!d.containsKey(s)) {
                    d.put(s, idx++);
                }
                nums[i].add(d.get(s));
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            boolean ok = true;
            for (int j = 0; j < n && ok; ++j) {
                if (i != j && nums[j].containsAll(nums[i])) {
                    ok = false;
                }
            }
            if (ok) {
                ans.add(i);
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
    vector<int> peopleIndexes(vector<vector<string>>& favoriteCompanies) {
        int n = favoriteCompanies.size();
        unordered_map<string, int> d;
        int idx = 0;
        vector<unordered_set<int>> nums(n);

        for (int i = 0; i < n; ++i) {
            for (const auto& s : favoriteCompanies[i]) {
                if (!d.contains(s)) {
                    d[s] = idx++;
                }
                nums[i].insert(d[s]);
            }
        }

        auto check = [](const unordered_set<int>& a, const unordered_set<int>& b) {
            for (int x : a) {
                if (!b.contains(x)) {
                    return false;
                }
            }
            return true;
        };

        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            bool ok = true;
            for (int j = 0; j < n && ok; ++j) {
                if (i != j && check(nums[i], nums[j])) {
                    ok = false;
                }
            }
            if (ok) {
                ans.push_back(i);
            }
        }

        return ans;
    }
};
```

#### Go

```go
func peopleIndexes(favoriteCompanies [][]string) (ans []int) {
	n := len(favoriteCompanies)
	d := make(map[string]int)
	idx := 0
	nums := make([]map[int]struct{}, n)

	for i := 0; i < n; i++ {
		nums[i] = make(map[int]struct{})
		for _, s := range favoriteCompanies[i] {
			if _, ok := d[s]; !ok {
				d[s] = idx
				idx++
			}
			nums[i][d[s]] = struct{}{}
		}
	}

	check := func(a, b map[int]struct{}) bool {
		for x := range a {
			if _, ok := b[x]; !ok {
				return false
			}
		}
		return true
	}
	for i := 0; i < n; i++ {
		ok := true
		for j := 0; j < n && ok; j++ {
			if i != j && check(nums[i], nums[j]) {
				ok = false
			}
		}
		if ok {
			ans = append(ans, i)
		}
	}

	return
}
```

#### TypeScript

```ts
function peopleIndexes(favoriteCompanies: string[][]): number[] {
    const n = favoriteCompanies.length;
    const d: Map<string, number> = new Map();
    let idx = 0;
    const nums: Set<number>[] = Array.from({ length: n }, () => new Set<number>());

    for (let i = 0; i < n; i++) {
        for (const s of favoriteCompanies[i]) {
            if (!d.has(s)) {
                d.set(s, idx++);
            }
            nums[i].add(d.get(s)!);
        }
    }

    const check = (a: Set<number>, b: Set<number>): boolean => {
        for (const x of a) {
            if (!b.has(x)) {
                return false;
            }
        }
        return true;
    };

    const ans: number[] = [];
    for (let i = 0; i < n; i++) {
        let ok = true;
        for (let j = 0; j < n && ok; j++) {
            if (i !== j && check(nums[i], nums[j])) {
                ok = false;
            }
        }
        if (ok) {
            ans.push(i);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
