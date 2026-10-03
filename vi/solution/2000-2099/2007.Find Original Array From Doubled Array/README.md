---
comments: true
difficulty: Medium
rating: 1557
source: Biweekly Contest 61 Q2
tags:
    - Greedy
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [2007. Find Original Array From Doubled Array](https://leetcode.com/problems/find-original-array-from-doubled-array)

[中文文档](/solution/2000-2099/2007.Find%20Original%20Array%20From%20Doubled%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Một mảng số nguyên <code>original</code> được biến đổi thành mảng <strong>được nhân đôi</strong> <code>changed</code> bằng cách nối thêm <strong>giá trị gấp đôi</strong> của mỗi phần tử trong <code>original</code>, sau đó <strong>xáo trộn</strong> ngẫu nhiên mảng thu được.</p>

<p>Cho mảng <code>changed</code>, hãy trả về <code>original</code><em> nếu </em><code>changed</code><em> là một mảng <strong>được nhân đôi</strong>. Nếu </em><code>changed</code><em> không phải là một mảng <strong>được nhân đôi</strong>, hãy trả về một mảng rỗng. Các phần tử trong</em> <code>original</code> <em>có thể được trả về theo <strong>bất kỳ</strong> thứ tự nào</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> changed = [1,3,4,2,6,8]
<strong>Đầu ra:</strong> [1,3,4]
<strong>Giải thích:</strong> Một mảng original có thể là [1,3,4]:
- Giá trị gấp đôi của 1 là 1 * 2 = 2.
- Giá trị gấp đôi của 3 là 3 * 2 = 6.
- Giá trị gấp đôi của 4 là 4 * 2 = 8.
Các mảng original khác có thể là [4,3,1] hoặc [3,1,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> changed = [6,3,0,1]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> changed không phải là một mảng được nhân đôi.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> changed = [1]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> changed không phải là một mảng được nhân đôi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= changed.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= changed[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 10^5$, ta không thể tìm kiếm tuyến tính giá trị gấp đôi cho từng phần tử. Nếu `changed` là một mảng được nhân đôi, phần tử nhỏ nhất của nó chắc chắn thuộc về mảng gốc: không tồn tại nửa nào nhỏ hơn nó.
>
> Sau khi sắp xếp, ta duyệt từ nhỏ đến lớn, ghép mỗi $x$ với một giá trị $2x$ còn lại. Một bộ đếm theo dõi các phần tử chưa dùng và bỏ qua những $x$ đã được sử dụng.
>
> Nếu thiếu $2x$, việc khôi phục thất bại. Các số 0 và phần tử trùng lặp được xử lý bằng chính các bộ đếm này.

<!-- thinking:end -->

Ta nhận thấy rằng nếu mảng `changed` là một mảng được nhân đôi, phần tử nhỏ nhất trong mảng `changed` cũng phải là một phần tử trong mảng gốc. Vì vậy, trước tiên ta sắp xếp mảng `changed`, sau đó bắt đầu từ phần tử đầu tiên và duyệt mảng `changed` theo thứ tự tăng dần.

Ta dùng một hash table hoặc mảng $cnt$ để đếm số lần xuất hiện của mỗi phần tử trong mảng `changed`. Với mỗi phần tử $x$ trong mảng `changed`, trước tiên ta kiểm tra xem $x$ có tồn tại trong $cnt$ hay không. Nếu không tồn tại, ta bỏ qua phần tử này. Ngược lại, ta giảm $cnt[x]$ đi một, rồi kiểm tra xem $x \times 2$ có tồn tại trong $cnt$ hay không. Nếu không tồn tại, ta trả về ngay một mảng rỗng. Nếu tồn tại, ta giảm $cnt[x \times 2]$ đi một và thêm $x$ vào mảng kết quả.

Sau khi duyệt xong, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng `changed`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findOriginalArray(self, changed: List[int]) -> List[int]:
        changed.sort()
        cnt = Counter(changed)
        ans = []
        for x in changed:
            if cnt[x] == 0:
                continue
            cnt[x] -= 1
            if cnt[x << 1] <= 0:
                return []
            cnt[x << 1] -= 1
            ans.append(x)
        return ans
```

#### Java

```java
class Solution {
    public int[] findOriginalArray(int[] changed) {
        int n = changed.length;
        Arrays.sort(changed);
        int[] cnt = new int[changed[n - 1] + 1];
        for (int x : changed) {
            ++cnt[x];
        }
        int[] ans = new int[n >> 1];
        int i = 0;
        for (int x : changed) {
            if (cnt[x] == 0) {
                continue;
            }
            --cnt[x];
            int y = x << 1;
            if (y >= cnt.length || cnt[y] <= 0) {
                return new int[0];
            }
            --cnt[y];
            ans[i++] = x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findOriginalArray(vector<int>& changed) {
        sort(changed.begin(), changed.end());
        vector<int> cnt(changed.back() + 1);
        for (int x : changed) {
            ++cnt[x];
        }
        vector<int> ans;
        for (int x : changed) {
            if (cnt[x] == 0) {
                continue;
            }
            --cnt[x];
            int y = x << 1;
            if (y >= cnt.size() || cnt[y] <= 0) {
                return {};
            }
            --cnt[y];
            ans.push_back(x);
        }
        return ans;
    }
};
```

#### Go

```go
func findOriginalArray(changed []int) (ans []int) {
	sort.Ints(changed)
	cnt := make([]int, changed[len(changed)-1]+1)
	for _, x := range changed {
		cnt[x]++
	}
	for _, x := range changed {
		if cnt[x] == 0 {
			continue
		}
		cnt[x]--
		y := x << 1
		if y >= len(cnt) || cnt[y] <= 0 {
			return []int{}
		}
		cnt[y]--
		ans = append(ans, x)
	}
	return
}
```

#### TypeScript

```ts
function findOriginalArray(changed: number[]): number[] {
    changed.sort((a, b) => a - b);
    const cnt: number[] = Array(changed.at(-1)! + 1).fill(0);
    for (const x of changed) {
        ++cnt[x];
    }
    const ans: number[] = [];
    for (const x of changed) {
        if (cnt[x] === 0) {
            continue;
        }
        cnt[x]--;
        const y = x << 1;
        if (y >= cnt.length || cnt[y] <= 0) {
            return [];
        }
        cnt[y]--;
        ans.push(x);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
