---
comments: true
difficulty: Medium
rating: 1431
source: Weekly Contest 151 Q2
tags:
    - Array
    - Hash Table
    - String
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [1170. Compare Strings by Frequency of the Smallest Character](https://leetcode.com/problems/compare-strings-by-frequency-of-the-smallest-character)

[中文文档](/solution/1100-1199/1170.Compare%20Strings%20by%20Frequency%20of%20the%20Smallest%20Character/README.md)

## Mô tả

<!-- description:start -->

<p>Gọi hàm <code>f(s)</code> là <strong>số lần xuất hiện của ký tự nhỏ nhất theo thứ tự từ điển</strong> trong chuỗi không rỗng <code>s</code>. Ví dụ, nếu <code>s = &quot;dcce&quot;</code> thì <code>f(s) = 2</code> vì ký tự nhỏ nhất theo thứ tự từ điển là <code>&#39;c&#39;</code>, xuất hiện 2 lần.</p>

<p>Cho mảng chuỗi <code>words</code> và một mảng chuỗi truy vấn <code>queries</code>. Với mỗi truy vấn <code>queries[i]</code>, hãy đếm <strong>số từ</strong> <code>W</code> trong <code>words</code> sao cho <code>f(queries[i])</code> &lt; <code>f(W)</code>.</p>

<p>Trả về <em>mảng số nguyên </em><code>answer</code><em>, trong đó mỗi </em><code>answer[i]</code><em> là kết quả của truy vấn thứ </em><code>i<sup>th</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [&quot;cbd&quot;], words = [&quot;zaaaz&quot;]
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong> Với truy vấn đầu tiên, ta có f(&quot;cbd&quot;) = 1, f(&quot;zaaaz&quot;) = 3 nên f(&quot;cbd&quot;) &lt; f(&quot;zaaaz&quot;).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [&quot;bbb&quot;,&quot;cc&quot;], words = [&quot;a&quot;,&quot;aa&quot;,&quot;aaa&quot;,&quot;aaaa&quot;]
<strong>Đầu ra:</strong> [1,2]
<strong>Giải thích:</strong> Với truy vấn đầu tiên, chỉ có f(&quot;bbb&quot;) &lt; f(&quot;aaaa&quot;). Với truy vấn thứ hai, cả f(&quot;aaa&quot;) và f(&quot;aaaa&quot;) đều lớn hơn f(&quot;cc&quot;).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= queries.length &lt;= 2000</code></li>
	<li><code>1 &lt;= words.length &lt;= 2000</code></li>
	<li><code>1 &lt;= queries[i].length, words[i].length &lt;= 10</code></li>
	<li><code>queries[i][j]</code> và <code>words[i][j]</code> là các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi truy vấn, ta đếm số từ có giá trị $f$ lớn hơn nghiêm ngặt. Duyệt tất cả từ cho từng truy vấn tốn $O(qn)$. Hãy tính trước $f(w)$ cho mọi từ, sắp xếp các giá trị rồi tìm kiếm nhị phân vị trí đầu tiên có giá trị lớn hơn $f(q)$; số phần tử từ đó đến cuối là đáp án. Tính $f$ bằng cách đếm ký tự nhỏ nhất, thao tác này tuyến tính theo độ dài chuỗi ngắn.

<!-- thinking:end -->

Trước tiên, theo mô tả bài toán, ta cài đặt hàm $f(s)$ trả về số lần xuất hiện của chữ cái nhỏ nhất theo thứ tự từ điển trong chuỗi $s$.

Tiếp theo, ta tính $f(w)$ cho từng chuỗi $w$ trong $words$, sắp xếp các giá trị rồi lưu vào mảng $nums$.

Sau đó, ta duyệt từng chuỗi truy vấn $q$ trong $queries$ và tìm kiếm nhị phân trong $nums$ để tìm vị trí đầu tiên $i$ có giá trị lớn hơn $f(q)$. Tất cả phần tử từ chỉ số $i$ trở đi trong $nums$ đều thỏa mãn $f(q) < f(W)$, nên đáp án cho truy vấn hiện tại là $n - i$.

Độ phức tạp thời gian là $O((n + q) \times M)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $q$ lần lượt là số phần tử của mảng $words$ và $queries$, còn $M$ là độ dài lớn nhất của các chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numSmallerByFrequency(self, queries: List[str], words: List[str]) -> List[int]:
        def f(s: str) -> int:
            cnt = Counter(s)
            return next(cnt[c] for c in ascii_lowercase if cnt[c])

        n = len(words)
        nums = sorted(f(w) for w in words)
        return [n - bisect_right(nums, f(q)) for q in queries]
```

#### Java

```java
class Solution {
    public int[] numSmallerByFrequency(String[] queries, String[] words) {
        int n = words.length;
        int[] nums = new int[n];
        for (int i = 0; i < n; ++i) {
            nums[i] = f(words[i]);
        }
        Arrays.sort(nums);
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int x = f(queries[i]);
            int l = 0, r = n;
            while (l < r) {
                int mid = (l + r) >> 1;
                if (nums[mid] > x) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            ans[i] = n - l;
        }
        return ans;
    }

    private int f(String s) {
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        for (int x : cnt) {
            if (x > 0) {
                return x;
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> numSmallerByFrequency(vector<string>& queries, vector<string>& words) {
        auto f = [](string s) {
            int cnt[26] = {0};
            for (char c : s) {
                cnt[c - 'a']++;
            }
            for (int x : cnt) {
                if (x) {
                    return x;
                }
            }
            return 0;
        };
        int n = words.size();
        int nums[n];
        for (int i = 0; i < n; i++) {
            nums[i] = f(words[i]);
        }
        sort(nums, nums + n);
        vector<int> ans;
        for (auto& q : queries) {
            int x = f(q);
            ans.push_back(n - (upper_bound(nums, nums + n, x) - nums));
        }
        return ans;
    }
};
```

#### Go

```go
func numSmallerByFrequency(queries []string, words []string) (ans []int) {
	f := func(s string) int {
		cnt := [26]int{}
		for _, c := range s {
			cnt[c-'a']++
		}
		for _, x := range cnt {
			if x > 0 {
				return x
			}
		}
		return 0
	}
	n := len(words)
	nums := make([]int, n)
	for i, w := range words {
		nums[i] = f(w)
	}
	sort.Ints(nums)
	for _, q := range queries {
		x := f(q)
		ans = append(ans, n-sort.SearchInts(nums, x+1))
	}
	return
}
```

#### TypeScript

```ts
function numSmallerByFrequency(queries: string[], words: string[]): number[] {
    const f = (s: string): number => {
        const cnt = new Array(26).fill(0);
        for (const c of s) {
            cnt[c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
        }
        return cnt.find(x => x > 0);
    };
    const nums = words.map(f).sort((a, b) => a - b);
    const ans: number[] = [];
    for (const q of queries) {
        const x = f(q);
        let l = 0,
            r = nums.length;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] > x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        ans.push(nums.length - l);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
