---
comments: true
difficulty: Medium
rating: 1655
source: Weekly Contest 456 Q2
tags:
    - Array
    - String
---

<!-- problem:start -->

# [3598. Longest Common Prefix Between Adjacent Strings After Removals](https://leetcode.com/problems/longest-common-prefix-between-adjacent-strings-after-removals)

[中文文档](/solution/3500-3599/3598.Longest%20Common%20Prefix%20Between%20Adjacent%20Strings%20After%20Removals/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng chuỗi <code>words</code>. Với mỗi chỉ số <code>i</code> trong phạm vi <code>[0, words.length - 1]</code>, hãy thực hiện các bước sau:</p>

<ul>
	<li>Xóa phần tử tại chỉ số <code>i</code> khỏi mảng <code>words</code>.</li>
	<li>Tính <strong>độ dài</strong> của <strong><span data-keyword="string-prefix">tiền tố</span> chung dài nhất</strong> giữa tất cả các cặp <strong>kề nhau</strong> trong mảng sau khi thay đổi.</li>
</ul>

<p>Trả về một mảng <code>answer</code>, trong đó <code>answer[i]</code> là độ dài của tiền tố chung dài nhất giữa các cặp kề nhau sau khi xóa phần tử tại chỉ số <code>i</code>. Nếu <strong>không còn cặp kề nhau nào</strong> hoặc <strong>không có cặp nào có tiền tố chung</strong>, thì <code>answer[i]</code> phải bằng 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;jump&quot;,&quot;run&quot;,&quot;run&quot;,&quot;jump&quot;,&quot;run&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,0,0,3,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Xóa chỉ số 0:
	<ul>
		<li><code>words</code> trở thành <code>[&quot;run&quot;, &quot;run&quot;, &quot;jump&quot;, &quot;run&quot;]</code></li>
		<li>Cặp kề nhau có tiền tố chung dài nhất là <code>[&quot;run&quot;, &quot;run&quot;]</code>, với tiền tố chung là <code>&quot;run&quot;</code> (độ dài 3)</li>
	</ul>
	</li>
	<li>Xóa chỉ số 1:
	<ul>
		<li><code>words</code> trở thành <code>[&quot;jump&quot;, &quot;run&quot;, &quot;jump&quot;, &quot;run&quot;]</code></li>
		<li>Không có cặp kề nhau nào có tiền tố chung (độ dài 0)</li>
	</ul>
	</li>
	<li>Xóa chỉ số 2:
	<ul>
		<li><code>words</code> trở thành <code>[&quot;jump&quot;, &quot;run&quot;, &quot;jump&quot;, &quot;run&quot;]</code></li>
		<li>Không có cặp kề nhau nào có tiền tố chung (độ dài 0)</li>
	</ul>
	</li>
	<li>Xóa chỉ số 3:
	<ul>
		<li><code>words</code> trở thành <code>[&quot;jump&quot;, &quot;run&quot;, &quot;run&quot;, &quot;run&quot;]</code></li>
		<li>Cặp kề nhau có tiền tố chung dài nhất là <code>[&quot;run&quot;, &quot;run&quot;]</code>, với tiền tố chung là <code>&quot;run&quot;</code> (độ dài 3)</li>
	</ul>
	</li>
	<li>Xóa chỉ số 4:
	<ul>
		<li>words trở thành <code>[&quot;jump&quot;, &quot;run&quot;, &quot;run&quot;, &quot;jump&quot;]</code></li>
		<li>Cặp kề nhau có tiền tố chung dài nhất là <code>[&quot;run&quot;, &quot;run&quot;]</code>, với tiền tố chung là <code>&quot;run&quot;</code> (độ dài 3)</li>
	</ul>
	</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;dog&quot;,&quot;racer&quot;,&quot;car&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,0,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Xóa tại bất kỳ chỉ số nào cũng cho kết quả bằng 0.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10<sup>4</sup></code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tổng độ dài của <code>words[i].length</code> nhỏ hơn hoặc bằng <code>10<sup>5</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Xóa $words[i]$ sẽ loại bỏ các cặp $(i-1,i)$ và $(i,i+1)$, đồng thời có thể thêm cặp nối $(i-1,i+1)$. Ta có thể lưu giá trị LCP lớn nhất trên toàn mảng bằng một multiset có thứ tự.
>
> Trước hết, tính trước LCP của mọi cặp kề nhau. Với mỗi $i$, xóa các cặp bị ảnh hưởng, thêm cặp nối, lấy giá trị lớn nhất rồi khôi phục lại. Bỏ qua các chỉ số nằm ngoài phạm vi ở hai đầu mảng.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{calc}(s, t)$ để tính độ dài tiền tố chung dài nhất giữa hai chuỗi $s$ và $t$. Ta có thể sử dụng một ordered set để duy trì độ dài tiền tố chung dài nhất của tất cả các cặp chuỗi kề nhau.

Định nghĩa hàm $\textit{add}(i, j)$ để thêm độ dài tiền tố chung dài nhất của cặp chuỗi tại các chỉ số $i$ và $j$ vào ordered set. Định nghĩa hàm $\textit{remove}(i, j)$ để xóa độ dài tiền tố chung dài nhất của cặp chuỗi tại các chỉ số $i$ và $j$ khỏi ordered set.

Trước tiên, ta tính độ dài tiền tố chung dài nhất của mọi cặp chuỗi kề nhau và lưu chúng vào ordered set. Sau đó, với mỗi chỉ số $i$, ta thực hiện các bước sau:

1. Xóa độ dài tiền tố chung dài nhất của cặp chuỗi tại các chỉ số $i$ và $i + 1$.
2. Xóa độ dài tiền tố chung dài nhất của cặp chuỗi tại các chỉ số $i - 1$ và $i$.
3. Thêm độ dài tiền tố chung dài nhất của cặp chuỗi tại các chỉ số $i - 1$ và $i + 1$.
4. Thêm giá trị lớn nhất hiện tại trong ordered set (nếu tồn tại và lớn hơn 0) vào đáp án.
5. Xóa độ dài tiền tố chung dài nhất của cặp chuỗi tại các chỉ số $i - 1$ và $i + 1$.
6. Thêm độ dài tiền tố chung dài nhất của cặp chuỗi tại các chỉ số $i - 1$ và $i$.
7. Thêm độ dài tiền tố chung dài nhất của cặp chuỗi tại các chỉ số $i$ và $i + 1$.

Nhờ đó, sau khi xóa từng chuỗi, ta có thể nhanh chóng tính độ dài tiền tố chung dài nhất giữa các cặp chuỗi kề nhau.

Độ phức tạp thời gian là $O(L + n \times \log n)$, và độ phức tạp không gian là $O(n)$, trong đó $L$ là tổng độ dài của tất cả các chuỗi và $n$ là số lượng chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestCommonPrefix(self, words: List[str]) -> List[int]:
        @cache
        def calc(s: str, t: str) -> int:
            k = 0
            for a, b in zip(s, t):
                if a != b:
                    break
                k += 1
            return k

        def add(i: int, j: int):
            if 0 <= i < n and 0 <= j < n:
                sl.add(calc(words[i], words[j]))

        def remove(i: int, j: int):
            if 0 <= i < n and 0 <= j < n:
                sl.remove(calc(words[i], words[j]))

        n = len(words)
        sl = SortedList(calc(a, b) for a, b in pairwise(words))
        ans = []
        for i in range(n):
            remove(i, i + 1)
            remove(i - 1, i)
            add(i - 1, i + 1)
            ans.append(sl[-1] if sl and sl[-1] > 0 else 0)
            remove(i - 1, i + 1)
            add(i - 1, i)
            add(i, i + 1)
        return ans
```

#### Java

```java
class Solution {
    private final TreeMap<Integer, Integer> tm = new TreeMap<>();
    private String[] words;
    private int n;

    public int[] longestCommonPrefix(String[] words) {
        n = words.length;
        this.words = words;
        for (int i = 0; i + 1 < n; ++i) {
            tm.merge(calc(words[i], words[i + 1]), 1, Integer::sum);
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            remove(i, i + 1);
            remove(i - 1, i);
            add(i - 1, i + 1);
            ans[i] = !tm.isEmpty() && tm.lastKey() > 0 ? tm.lastKey() : 0;
            remove(i - 1, i + 1);
            add(i - 1, i);
            add(i, i + 1);
        }
        return ans;
    }

    private void add(int i, int j) {
        if (i >= 0 && i < n && j >= 0 && j < n) {
            tm.merge(calc(words[i], words[j]), 1, Integer::sum);
        }
    }

    private void remove(int i, int j) {
        if (i >= 0 && i < n && j >= 0 && j < n) {
            int x = calc(words[i], words[j]);
            if (tm.merge(x, -1, Integer::sum) == 0) {
                tm.remove(x);
            }
        }
    }

    private int calc(String s, String t) {
        int m = Math.min(s.length(), t.length());
        for (int k = 0; k < m; ++k) {
            if (s.charAt(k) != t.charAt(k)) {
                return k;
            }
        }
        return m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> longestCommonPrefix(vector<string>& words) {
        multiset<int> ms;
        int n = words.size();
        auto calc = [&](const string& s, const string& t) {
            int m = min(s.size(), t.size());
            for (int k = 0; k < m; ++k) {
                if (s[k] != t[k]) {
                    return k;
                }
            }
            return m;
        };
        for (int i = 0; i + 1 < n; ++i) {
            ms.insert(calc(words[i], words[i + 1]));
        }
        vector<int> ans(n);
        auto add = [&](int i, int j) {
            if (i >= 0 && i < n && j >= 0 && j < n) {
                ms.insert(calc(words[i], words[j]));
            }
        };
        auto remove = [&](int i, int j) {
            if (i >= 0 && i < n && j >= 0 && j < n) {
                int x = calc(words[i], words[j]);
                auto it = ms.find(x);
                if (it != ms.end()) {
                    ms.erase(it);
                }
            }
        };
        for (int i = 0; i < n; ++i) {
            remove(i, i + 1);
            remove(i - 1, i);
            add(i - 1, i + 1);
            ans[i] = ms.empty() ? 0 : *ms.rbegin();
            remove(i - 1, i + 1);
            add(i - 1, i);
            add(i, i + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func longestCommonPrefix(words []string) []int {
	n := len(words)
	tm := treemap.NewWithIntComparator()

	calc := func(s, t string) int {
		m := min(len(s), len(t))
		for k := 0; k < m; k++ {
			if s[k] != t[k] {
				return k
			}
		}
		return m
	}

	add := func(i, j int) {
		if i >= 0 && i < n && j >= 0 && j < n {
			x := calc(words[i], words[j])
			if v, ok := tm.Get(x); ok {
				tm.Put(x, v.(int)+1)
			} else {
				tm.Put(x, 1)
			}
		}
	}

	remove := func(i, j int) {
		if i >= 0 && i < n && j >= 0 && j < n {
			x := calc(words[i], words[j])
			if v, ok := tm.Get(x); ok {
				if v.(int) == 1 {
					tm.Remove(x)
				} else {
					tm.Put(x, v.(int)-1)
				}
			}
		}
	}

	for i := 0; i+1 < n; i++ {
		add(i, i+1)
	}

	ans := make([]int, n)
	for i := 0; i < n; i++ {
		remove(i, i+1)
		remove(i-1, i)
		add(i-1, i+1)

		if !tm.Empty() {
			if maxKey, _ := tm.Max(); maxKey.(int) > 0 {
				ans[i] = maxKey.(int)
			}
		}

		remove(i-1, i+1)
		add(i-1, i)
		add(i, i+1)
	}

	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
