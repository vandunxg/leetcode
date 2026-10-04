---
comments: true
difficulty: Hard
rating: 2608
source: Weekly Contest 444 Q4
tags:
    - Array
    - Hash Table
    - Linked List
    - Doubly-Linked List
    - Ordered Set
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3510. Minimum Pair Removal to Sort Array II](https://leetcode.com/problems/minimum-pair-removal-to-sort-array-ii)

[中文文档](/solution/3500-3599/3510.Minimum%20Pair%20Removal%20to%20Sort%20Array%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code>, bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý:</p>

<ul>
	<li>Chọn cặp phần tử <strong>liền kề</strong> có tổng <strong>nhỏ nhất</strong> trong <code>nums</code>. Nếu có nhiều cặp như vậy, chọn cặp nằm ngoài cùng bên trái.</li>
	<li>Thay cặp phần tử bằng tổng của chúng.</li>
</ul>

<p>Trả về <strong>số thao tác ít nhất</strong> cần thực hiện để mảng trở thành <strong>không giảm</strong>.</p>

<p>Một mảng được gọi là <strong>không giảm</strong> nếu mỗi phần tử lớn hơn hoặc bằng phần tử đứng trước nó (nếu có).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,2,3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Cặp <code>(3,1)</code> có tổng nhỏ nhất là 4. Sau khi thay thế, <code>nums = [5,2,4]</code>.</li>
	<li>Cặp <code>(2,4)</code> có tổng nhỏ nhất là 6. Sau khi thay thế, <code>nums = [5,6]</code>.</li>
</ul>

<p>Mảng <code>nums</code> trở thành không giảm sau hai thao tác.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng <code>nums</code> đã được sắp xếp.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorted Set

<!-- thinking:start -->

> **Tư duy**
>
> Cách mô phỏng $O(n^2)$ của bài trước không đáp ứng được với $n \le 10^5$. Ta vẫn gộp cặp phần tử liền kề có tổng nhỏ nhất, nhưng việc quét toàn bộ mảng ở mỗi bước quá chậm.
>
> Một sorted set dùng khóa $(\textit{sum}, i)$ sẽ cho phép lấy cặp nhỏ nhất; một sorted set khác lưu các chỉ số còn tồn tại để tìm phần tử liền trước và liền sau trong $O(\log n)$. Biến $\textit{inv}$ theo dõi số cặp liền kề giảm dần và chỉ được cập nhật tại hai hoặc ba vị trí bị ảnh hưởng, dừng lại khi $\textit{inv}=0$.

<!-- thinking:end -->

Ta định nghĩa một sorted set $\textit{sl}$ để lưu các tuple $(\textit{s}, i)$ gồm tổng của mỗi cặp phần tử liền kề và chỉ số bên trái của cặp đó, định nghĩa một sorted set khác $\textit{idx}$ để lưu các chỉ số của những phần tử còn lại trong mảng hiện tại, đồng thời dùng biến $\textit{inv}$ để ghi nhận số nghịch thế trong mảng hiện tại. Ban đầu, ta duyệt mảng $\textit{nums}$, thêm các tuple gồm tổng của mọi cặp phần tử liền kề và chỉ số bên trái của chúng vào sorted set $\textit{sl}$, đồng thời tính số nghịch thế $\textit{inv}$.

Trong mỗi thao tác, ta lấy phần tử $(\textit{s}, i)$ có tổng nhỏ nhất từ sorted set $\textit{sl}$. Khi đó, cặp phần tử tương ứng với các chỉ số $i$ và $j$ (trong đó $j$ là chỉ số tiếp theo sau $i$ trong sorted set $\textit{idx}$) chính là cặp phần tử liền kề có tổng nhỏ nhất trong mảng hiện tại. Nếu $nums[i] > nums[j]$, cặp này là một nghịch thế, và sau khi gộp rồi thay thế, số nghịch thế $\textit{inv}$ giảm đi một.

Tiếp theo, ta cần cập nhật các cặp phần tử liên quan đến các chỉ số $i$ và $j$:

1. Nếu chỉ số $i$ có chỉ số đứng trước là $h$ trong sorted set $\textit{idx}$, ta cần cập nhật cặp phần tử $(h, i)$. Nếu $nums[h] > nums[i]$, cặp này là một nghịch thế, và sau khi gộp rồi thay thế, số nghịch thế $\textit{inv}$ giảm đi một. Sau đó, ta xóa cặp phần tử $(h, i)$ khỏi sorted set $\textit{sl}$ và thêm cặp phần tử mới $(h, s)$ vào sorted set $\textit{sl}$. Nếu $nums[h] > s$, cặp phần tử mới là một nghịch thế, và sau khi gộp rồi thay thế, số nghịch thế $\textit{inv}$ tăng thêm một.
2. Nếu chỉ số $j$ có chỉ số đứng sau là $k$ trong sorted set $\textit{idx}$, ta cần cập nhật cặp phần tử $(j, k)$. Nếu $nums[j] > nums[k]$, cặp này là một nghịch thế, và sau khi gộp rồi thay thế, số nghịch thế $\textit{inv}$ giảm đi một. Sau đó, ta xóa cặp phần tử $(j, k)$ khỏi sorted set $\textit{sl}$ và thêm cặp phần tử mới $(s, k)$ vào sorted set $\textit{sl}$. Nếu $s > nums[k]$, cặp phần tử mới là một nghịch thế, và sau khi gộp rồi thay thế, số nghịch thế $\textit{inv}$ tăng thêm một.

Tiếp theo, ta thay phần tử tại chỉ số $i$ bằng $\textit{s}$ và xóa chỉ số $j$ khỏi sorted set $\textit{idx}$. Ta lặp lại quy trình trên cho đến khi số nghịch thế $\textit{inv}$ bằng không. Cuối cùng, số thao tác là số thao tác ít nhất cần thực hiện để mảng trở thành không giảm.

Độ phức tạp thời gian là $O(n \log n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumPairRemoval(self, nums: List[int]) -> int:
        n = len(nums)
        sl = SortedList()
        idx = SortedList(range(n))
        inv = 0
        for i in range(n - 1):
            sl.add((nums[i] + nums[i + 1], i))
            if nums[i] > nums[i + 1]:
                inv += 1
        ans = 0
        while inv:
            ans += 1
            s, i = sl.pop(0)
            pos = idx.index(i)
            j = idx[pos + 1]
            if nums[i] > nums[j]:
                inv -= 1
            if pos > 0:
                h = idx[pos - 1]
                if nums[h] > nums[i]:
                    inv -= 1
                sl.remove((nums[h] + nums[i], h))
                if nums[h] > s:
                    inv += 1
                sl.add((nums[h] + s, h))
            if pos + 2 < len(idx):
                k = idx[pos + 2]
                if nums[j] > nums[k]:
                    inv -= 1
                sl.remove((nums[j] + nums[k], j))
                if s > nums[k]:
                    inv += 1
                sl.add((s + nums[k], i))

            nums[i] = s
            idx.remove(j)
        return ans
```

#### Java

```java
class Solution {
    record Pair(long s, int i) implements Comparable<Pair> {
        @Override
        public int compareTo(Pair other) {
            int compareS = Long.compare(this.s, other.s);
            return compareS != 0 ? compareS : Integer.compare(this.i, other.i);
        }
    }

    public int minimumPairRemoval(int[] nums) {
        int n = nums.length;
        int inv = 0;
        TreeSet<Pair> sl = new TreeSet<>();
        for (int i = 0; i < n - 1; ++i) {
            if (nums[i] > nums[i + 1]) {
                ++inv;
            }
            sl.add(new Pair(nums[i] + nums[i + 1], i));
        }
        TreeSet<Integer> idx = new TreeSet<>();
        long[] arr = new long[n];
        for (int i = 0; i < n; ++i) {
            idx.add(i);
            arr[i] = nums[i];
        }

        int ans = 0;
        while (inv > 0) {
            ++ans;
            var p = sl.pollFirst();
            long s = p.s;
            int i = p.i;
            int j = idx.higher(i);
            if (arr[i] > arr[j]) {
                --inv;
            }
            Integer h = idx.lower(i);
            if (h != null) {
                if (arr[h] > arr[i]) {
                    --inv;
                }
                sl.remove(new Pair(arr[h] + arr[i], h));
                if (arr[h] > s) {
                    ++inv;
                }
                sl.add(new Pair(arr[h] + s, h));
            }
            Integer k = idx.higher(j);
            if (k != null) {
                if (arr[j] > arr[k]) {
                    --inv;
                }
                sl.remove(new Pair(arr[j] + arr[k], j));
                if (s > arr[k]) {
                    ++inv;
                }
                sl.add(new Pair(s + arr[k], i));
            }
            arr[i] = s;
            idx.remove(j);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumPairRemoval(vector<int>& nums) {
        int n = nums.size();
        int inv = 0;

        set<pair<long long, int>> sl;
        set<int> idx;
        vector<long long> arr(nums.begin(), nums.end());

        for (int i = 0; i < n; ++i) idx.insert(i);

        for (int i = 0; i < n - 1; ++i) {
            if (nums[i] > nums[i + 1]) {
                ++inv;
            }
            sl.insert({(long long) nums[i] + nums[i + 1], i});
        }

        int ans = 0;
        while (inv > 0) {
            ++ans;

            auto it = sl.begin();
            long long s = it->first;
            int i = it->second;
            sl.erase(it);

            auto j_it = idx.upper_bound(i);
            int j = *j_it;

            if (arr[i] > arr[j]) {
                --inv;
            }

            auto i_it = idx.find(i);
            if (i_it != idx.begin()) {
                auto h_it = prev(i_it);
                int h = *h_it;

                if (arr[h] > arr[i]) {
                    --inv;
                }
                sl.erase({arr[h] + arr[i], h});

                if (arr[h] > s) {
                    ++inv;
                }
                sl.insert({arr[h] + s, h});
            }

            auto k_it = next(j_it);
            if (k_it != idx.end()) {
                int k = *k_it;

                if (arr[j] > arr[k]) {
                    --inv;
                }
                sl.erase({arr[j] + arr[k], j});

                if (s > arr[k]) {
                    ++inv;
                }
                sl.insert({s + arr[k], i});
            }

            arr[i] = s;
            idx.erase(j);
        }

        return ans;
    }
};
```

#### Go

```go
func minimumPairRemoval(nums []int) (ans int) {
	type pair struct{ s, i int }

	n := len(nums)
	inv := 0

	sl := redblacktree.NewWith[pair, struct{}](func(a, b pair) int { return cmp.Or(a.s-b.s, a.i-b.i) })
	idx := redblacktree.New[int, struct{}]()
	for i := 0; i < n; i++ {
		idx.Put(i, struct{}{})
	}

	for i := 0; i < n-1; i++ {
		if nums[i] > nums[i+1] {
			inv++
		}
		sl.Put(pair{nums[i] + nums[i+1], i}, struct{}{})
	}

	for inv > 0 {
		ans++

		it := sl.Iterator()
		it.First()
		p := it.Key()
		sl.Remove(p)

		s, i := p.s, p.i

		jNode, _ := idx.Ceiling(i + 1)
		j := jNode.Key

		if nums[i] > nums[j] {
			inv--
		}

		if hNode, ok := idx.Floor(i - 1); ok {
			h := hNode.Key

			if nums[h] > nums[i] {
				inv--
			}
			sl.Remove(pair{nums[h] + nums[i], h})

			if nums[h] > s {
				inv++
			}
			sl.Put(pair{nums[h] + s, h}, struct{}{})
		}

		if kNode, ok := idx.Ceiling(j + 1); ok {
			k := kNode.Key

			if nums[j] > nums[k] {
				inv--
			}
			sl.Remove(pair{nums[j] + nums[k], j})

			if s > nums[k] {
				inv++
			}
			sl.Put(pair{s + nums[k], i}, struct{}{})
		}

		nums[i] = s
		idx.Remove(j)
	}

	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
