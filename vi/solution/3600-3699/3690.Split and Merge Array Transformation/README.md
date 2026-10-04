---
comments: true
difficulty: Medium
rating: 1982
source: Weekly Contest 468 Q3
---

<!-- problem:start -->

# [3690. Split and Merge Array Transformation](https://leetcode.com/problems/split-and-merge-array-transformation)

[中文文档](/solution/3600-3699/3690.Split%20and%20Merge%20Array%20Transformation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>, mỗi mảng có độ dài <code>n</code>. Bạn có thể thực hiện <strong>phép tách và gộp</strong> sau đây trên <code>nums1</code> một số lần bất kỳ:</p>

<ol>
	<li>Chọn một mảng con <code>nums1[L..R]</code>.</li>
	<li>Xóa mảng con đó, giữ lại tiền tố <code>nums1[0..L-1]</code> (rỗng nếu <code>L = 0</code>) và hậu tố <code>nums1[R+1..n-1]</code> (rỗng nếu <code>R = n - 1</code>).</li>
	<li>Chèn lại mảng con đã xóa (theo thứ tự ban đầu) vào <strong>bất kỳ</strong> vị trí nào trong mảng còn lại (tức là giữa hai phần tử bất kỳ, ở đầu hoặc ở cuối mảng).</li>
</ol>

<p>Trả về số <strong>phép tách và gộp</strong> <strong>ít nhất</strong> cần thực hiện để biến đổi <code>nums1</code> thành <code>nums2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [3,1,2], nums2 = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tách mảng con <code>[3]</code> (<code>L = 0</code>, <code>R = 0</code>); mảng còn lại là <code>[1,2]</code>.</li>
	<li>Chèn <code>[3]</code> vào cuối; mảng trở thành <code>[1,2,3]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào: </strong><span class="example-io">nums1 = </span>[1,1,2,3,4,5]<span class="example-io">, nums2 = </span>[5,4,3,2,1,1]</p>

<p><strong>Đầu ra: </strong>3</p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Xóa <code>[1,1,2]</code> tại các chỉ số <code>0 - 2</code>; mảng còn lại là <code>[3,4,5]</code>; chèn <code>[1,1,2]</code> vào vị trí <code>2</code>, thu được <code>[3,4,1,1,2,5]</code>.</li>
	<li>Xóa <code>[4,1,1]</code> tại các chỉ số <code>1 - 3</code>; mảng còn lại là <code>[3,2,5]</code>; chèn <code>[4,1,1]</code> vào vị trí <code>3</code>, thu được <code>[3,2,5,4,1,1]</code>.</li>
	<li>Xóa <code>[3,2]</code> tại các chỉ số <code>0 - 1</code>; mảng còn lại là <code>[5,4,1,1]</code>; chèn <code>[3,2]</code> vào vị trí <code>2</code>, thu được <code>[5,4,3,2,1,1]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums1.length == nums2.length &lt;= 6</code></li>
	<li><code>-10<sup>5</sup> &lt;= nums1[i], nums2[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>nums2</code> là một <strong>hoán vị</strong> của <code>nums1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước sẽ cắt một đoạn rồi chèn nó vào vị trí khác. Vì $n\le 6$, có nhiều nhất $n!$ trạng thái, nên BFS sẽ tìm được chuỗi ngắn nhất.
>
> Lưu các mảng dưới dạng tuple trong một tập visited. Từ mỗi trạng thái, liệt kê mọi đoạn cắt $[l,r]$ và mọi vị trí chèn.
>
> Lớp BFS đầu tiên gặp $\textit{nums2}$ chính là đáp án. Các trạng thái là những hoán vị của cùng một multiset.

<!-- thinking:end -->

Ta có thể sử dụng Breadth-First Search (BFS) để giải bài toán này. Vì độ dài mảng không vượt quá 6, ta có thể liệt kê tất cả các phép tách và gộp có thể để tìm số phép toán ít nhất.

Trước tiên, ta định nghĩa một queue $\textit{q}$ để lưu các trạng thái mảng hiện tại, đồng thời dùng một tập $\textit{vis}$ để ghi nhận các trạng thái mảng đã duyệt nhằm tránh tính toán trùng lặp. Ban đầu, queue chỉ chứa mảng $\textit{nums1}$.

Sau đó, ta thực hiện các bước sau:

1. Lấy trạng thái mảng hiện tại $\textit{cur}$ khỏi queue.
2. Nếu $\textit{cur}$ bằng mảng đích $\textit{nums2}$, trả về số phép toán hiện tại.
3. Nếu không, liệt kê mọi vị trí tách $(l, r)$, xóa mảng con $\textit{cur}[l..r]$ để thu được mảng còn lại $\textit{remain}$ và mảng con $\textit{sub}$.
4. Chèn mảng con $\textit{sub}$ vào mọi vị trí có thể của mảng còn lại $\textit{remain}$ để tạo các trạng thái mảng mới $\textit{nxt}$.
5. Nếu trạng thái mảng mới $\textit{nxt}$ chưa được duyệt, thêm nó vào queue và tập các trạng thái đã duyệt.
6. Lặp lại các bước trên cho đến khi tìm thấy mảng đích hoặc queue rỗng.

Độ phức tạp thời gian là $O(n! \times n^4)$, và độ phức tạp không gian là $O(n! \times n)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSplitMerge(self, nums1: List[int], nums2: List[int]) -> int:
        n = len(nums1)
        target = tuple(nums2)
        start = tuple(nums1)

        q = [start]
        vis = set()
        vis.add(start)

        for ans in count(0):
            t = q
            q = []
            for cur in t:
                if cur == target:
                    return ans
                for l in range(n):
                    for r in range(l, n):
                        remain = list(cur[:l]) + list(cur[r + 1 :])
                        sub = cur[l : r + 1]
                        for i in range(len(remain) + 1):
                            nxt = tuple(remain[:i] + list(sub) + remain[i:])
                            if nxt not in vis:
                                vis.add(nxt)
                                q.append(nxt)
```

#### Java

```java
class Solution {
    public int minSplitMerge(int[] nums1, int[] nums2) {
        int n = nums1.length;
        List<Integer> target = toList(nums2);
        List<Integer> start = toList(nums1);
        List<List<Integer>> q = List.of(start);
        Set<List<Integer>> vis = new HashSet<>();
        vis.add(start);
        for (int ans = 0;; ++ans) {
            var t = q;
            q = new ArrayList<>();
            for (var cur : t) {
                if (cur.equals(target)) {
                    return ans;
                }
                for (int l = 0; l < n; ++l) {
                    for (int r = l; r < n; ++r) {
                        List<Integer> remain = new ArrayList<>();
                        for (int i = 0; i < l; ++i) {
                            remain.add(cur.get(i));
                        }
                        for (int i = r + 1; i < n; ++i) {
                            remain.add(cur.get(i));
                        }
                        List<Integer> sub = cur.subList(l, r + 1);
                        for (int i = 0; i <= remain.size(); ++i) {
                            List<Integer> nxt = new ArrayList<>();
                            for (int j = 0; j < i; ++j) {
                                nxt.add(remain.get(j));
                            }
                            for (int x : sub) {
                                nxt.add(x);
                            }
                            for (int j = i; j < remain.size(); ++j) {
                                nxt.add(remain.get(j));
                            }
                            if (vis.add(nxt)) {
                                q.add(nxt);
                            }
                        }
                    }
                }
            }
        }
    }

    private List<Integer> toList(int[] arr) {
        List<Integer> res = new ArrayList<>(arr.length);
        for (int x : arr) {
            res.add(x);
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSplitMerge(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        vector<int> target = nums2;
        vector<vector<int>> q{nums1};
        set<vector<int>> vis;
        vis.insert(nums1);

        for (int ans = 0;; ++ans) {
            vector<vector<int>> t = q;
            q.clear();
            for (auto& cur : t) {
                if (cur == target) {
                    return ans;
                }
                for (int l = 0; l < n; ++l) {
                    for (int r = l; r < n; ++r) {
                        vector<int> remain;
                        remain.insert(remain.end(), cur.begin(), cur.begin() + l);
                        remain.insert(remain.end(), cur.begin() + r + 1, cur.end());
                        vector<int> sub(cur.begin() + l, cur.begin() + r + 1);
                        for (int i = 0; i <= (int) remain.size(); ++i) {
                            vector<int> nxt;
                            nxt.insert(nxt.end(), remain.begin(), remain.begin() + i);
                            nxt.insert(nxt.end(), sub.begin(), sub.end());
                            nxt.insert(nxt.end(), remain.begin() + i, remain.end());

                            if (!vis.count(nxt)) {
                                vis.insert(nxt);
                                q.push_back(nxt);
                            }
                        }
                    }
                }
            }
        }
    }
};
```

#### Go

```go
func minSplitMerge(nums1 []int, nums2 []int) int {
	n := len(nums1)

	toArr := func(nums []int) [6]int {
		var t [6]int
		for i, x := range nums {
			t[i] = x
		}
		return t
	}

	start := toArr(nums1)
	target := toArr(nums2)

	vis := map[[6]int]bool{start: true}
	q := [][6]int{start}

	for ans := 0; ; ans++ {
		nq := [][6]int{}
		for _, cur := range q {
			if cur == target {
				return ans
			}
			for l := 0; l < n; l++ {
				for r := l; r < n; r++ {
					remain := []int{}
					for i := 0; i < l; i++ {
						remain = append(remain, cur[i])
					}
					for i := r + 1; i < n; i++ {
						remain = append(remain, cur[i])
					}

					sub := []int{}
					for i := l; i <= r; i++ {
						sub = append(sub, cur[i])
					}

					for pos := 0; pos <= len(remain); pos++ {
						nxtSlice := []int{}
						nxtSlice = append(nxtSlice, remain[:pos]...)
						nxtSlice = append(nxtSlice, sub...)
						nxtSlice = append(nxtSlice, remain[pos:]...)

						nxt := toArr(nxtSlice)
						if !vis[nxt] {
							vis[nxt] = true
							nq = append(nq, nxt)
						}
					}
				}
			}
		}
		q = nq
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
