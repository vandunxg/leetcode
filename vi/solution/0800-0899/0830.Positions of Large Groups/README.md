---
comments: true
difficulty: Easy
tags:
    - String
---

<!-- problem:start -->

# [830. Positions of Large Groups](https://leetcode.com/problems/positions-of-large-groups)

[中文文档](/solution/0800-0899/0830.Positions%20of%20Large%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Trong chuỗi <code><font face="monospace">s</font></code>&nbsp;gồm các chữ cái viết thường, các chữ cái giống nhau tạo thành những nhóm liên tiếp.</p>

<p>Ví dụ, chuỗi <code>s = &quot;abbxxxxzyy&quot;</code> gồm các nhóm <code>&quot;a&quot;</code>, <code>&quot;bb&quot;</code>, <code>&quot;xxxx&quot;</code>, <code>&quot;z&quot;</code> và&nbsp;<code>&quot;yy&quot;</code>.</p>

<p>Mỗi nhóm được biểu diễn bằng khoảng&nbsp;<code>[start, end]</code>, trong đó&nbsp;<code>start</code>&nbsp;và&nbsp;<code>end</code>&nbsp;lần lượt là chỉ số bắt đầu và kết thúc&nbsp;(bao gồm cả hai đầu mút) của nhóm. Trong ví dụ trên,&nbsp;<code>&quot;xxxx&quot;</code>&nbsp;có khoảng&nbsp;<code>[3,6]</code>.</p>

<p>Một nhóm được xem là&nbsp;<strong>lớn</strong>&nbsp;nếu có từ 3 ký tự trở lên.</p>

<p>Hãy trả về&nbsp;<em>các khoảng của mọi nhóm <strong>lớn</strong>, được sắp xếp theo thứ tự <strong>tăng dần của chỉ số bắt đầu</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abbxxxxzzy&quot;
<strong>Đầu ra:</strong> [[3,6]]
<strong>Giải thích:</strong> <code>&quot;xxxx&quot; is the only </code>nhóm lớn duy nhất có chỉ số bắt đầu là 3 và chỉ số kết thúc là 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abc&quot;
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Ta có các nhóm &quot;a&quot;, &quot;b&quot; và &quot;c&quot;, không nhóm nào đủ lớn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcdddeeeeaabbbcd&quot;
<strong>Đầu ra:</strong> [[3,5],[6,9],[12,14]]
<strong>Giải thích:</strong> Các nhóm lớn là &quot;ddd&quot;, &quot;eeee&quot; và &quot;bbb&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Nhóm lớn là một đoạn gồm cùng một chữ cái lặp liên tiếp ít nhất $3$ lần. Chỉ cần duyệt chuỗi chữ thường một lượt để tìm mọi khoảng như vậy.
>
> Dùng hai con trỏ để xác định từng đoạn; nếu độ dài đoạn ít nhất $3$, ghi lại hai đầu mút rồi chuyển đến đoạn tiếp theo.

<!-- thinking:end -->

Ta dùng hai con trỏ $i$ và $j$ để tìm vị trí bắt đầu và kết thúc của từng nhóm, sau đó kiểm tra độ dài nhóm có lớn hơn hoặc bằng $3$ hay không. Nếu có, ta thêm nhóm đó vào mảng kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largeGroupPositions(self, s: str) -> List[List[int]]:
        i, n = 0, len(s)
        ans = []
        while i < n:
            j = i
            while j < n and s[j] == s[i]:
                j += 1
            if j - i >= 3:
                ans.append([i, j - 1])
            i = j
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> largeGroupPositions(String s) {
        int n = s.length();
        int i = 0;
        List<List<Integer>> ans = new ArrayList<>();
        while (i < n) {
            int j = i;
            while (j < n && s.charAt(j) == s.charAt(i)) {
                ++j;
            }
            if (j - i >= 3) {
                ans.add(List.of(i, j - 1));
            }
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> largeGroupPositions(string s) {
        int n = s.size();
        int i = 0;
        vector<vector<int>> ans;
        while (i < n) {
            int j = i;
            while (j < n && s[j] == s[i]) {
                ++j;
            }
            if (j - i >= 3) {
                ans.push_back({i, j - 1});
            }
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func largeGroupPositions(s string) [][]int {
	i, n := 0, len(s)
	ans := [][]int{}
	for i < n {
		j := i
		for j < n && s[j] == s[i] {
			j++
		}
		if j-i >= 3 {
			ans = append(ans, []int{i, j - 1})
		}
		i = j
	}
	return ans
}
```

#### TypeScript

```ts
function largeGroupPositions(s: string): number[][] {
    const n = s.length;
    const ans: number[][] = [];

    for (let i = 0; i < n;) {
        let j = i;
        while (j < n && s[j] === s[i]) {
            ++j;
        }
        if (j - i >= 3) {
            ans.push([i, j - 1]);
        }
        i = j;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
