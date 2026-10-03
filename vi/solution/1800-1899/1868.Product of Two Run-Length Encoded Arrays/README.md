---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [1868. Product of Two Run-Length Encoded Arrays 🔒](https://leetcode.com/problems/product-of-two-run-length-encoded-arrays)

[中文文档](/solution/1800-1899/1868.Product%20of%20Two%20Run-Length%20Encoded%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Run-length encoding</strong> là một thuật toán nén cho phép biểu diễn một mảng số nguyên <code>nums</code> có nhiều đoạn gồm các số <strong>lặp lại liên tiếp</strong> bằng một mảng 2D <code>encoded</code> (thường nhỏ hơn). Mỗi <code>encoded[i] = [val<sub>i</sub>, freq<sub>i</sub>]</code> mô tả đoạn thứ <code>i<sup>th</sup></code> gồm các số lặp lại trong <code>nums</code>, trong đó <code>val<sub>i</sub></code> là giá trị được lặp lại <code>freq<sub>i</sub></code> lần.</p>

<ul>
	<li>Ví dụ, <code>nums = [1,1,1,2,2,2,2,2]</code> được biểu diễn bằng mảng <strong>run-length encoded</strong> <code>encoded = [[1,3],[2,5]]</code>. Có thể đọc cách khác là “ba số <code>1</code> theo sau bởi năm số <code>2</code>”.</li>
</ul>

<p><strong>Tích</strong> của hai mảng run-length encoded <code>encoded1</code> và <code>encoded2</code> có thể được tính theo các bước sau:</p>

<ol>
	<li><strong>Mở rộng</strong> cả <code>encoded1</code> và <code>encoded2</code> thành các mảng đầy đủ <code>nums1</code> và <code>nums2</code> tương ứng.</li>
	<li>Tạo một mảng mới <code>prodNums</code> có độ dài <code>nums1.length</code> và đặt <code>prodNums[i] = nums1[i] * nums2[i]</code>.</li>
	<li><strong>Nén</strong> <code>prodNums</code> thành một mảng run-length encoded rồi trả về mảng đó.</li>
</ol>

<p>Cho hai mảng <strong>run-length encoded</strong> <code>encoded1</code> và <code>encoded2</code> lần lượt biểu diễn các mảng đầy đủ <code>nums1</code> và <code>nums2</code>. <code>nums1</code> và <code>nums2</code> có <strong>cùng độ dài</strong>. Mỗi <code>encoded1[i] = [val<sub>i</sub>, freq<sub>i</sub>]</code> mô tả đoạn thứ <code>i<sup>th</sup></code> của <code>nums1</code>, còn mỗi <code>encoded2[j] = [val<sub>j</sub>, freq<sub>j</sub>]</code> mô tả đoạn thứ <code>j<sup>th</sup></code> của <code>nums2</code>.</p>

<p>Trả về <i><strong>tích</strong> của </i><code>encoded1</code><em> và </em><code>encoded2</code>.</p>

<p><strong>Lưu ý:</strong> Phải nén sao cho mảng run-length encoded có độ dài <strong>nhỏ nhất</strong> có thể.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> encoded1 = [[1,3],[2,3]], encoded2 = [[6,3],[3,3]]
<strong>Đầu ra:</strong> [[6,6]]
<strong>Giải thích:</strong> encoded1 mở rộng thành [1,1,1,2,2,2] và encoded2 mở rộng thành [6,6,6,3,3,3].
prodNums = [6,6,6,6,6,6], được nén thành mảng run-length encoded [[6,6]].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> encoded1 = [[1,3],[2,1],[3,2]], encoded2 = [[2,3],[3,3]]
<strong>Đầu ra:</strong> [[2,3],[6,1],[9,2]]
<strong>Giải thích:</strong> encoded1 mở rộng thành [1,1,1,2,3,3] và encoded2 mở rộng thành [2,2,2,3,3,3].
prodNums = [2,2,2,6,9,9], được nén thành mảng run-length encoded [[2,3],[6,1],[9,2]].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= encoded1.length, encoded2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>encoded1[i].length == 2</code></li>
	<li><code>encoded2[j].length == 2</code></li>
	<li><code>1 &lt;= val<sub>i</sub>, freq<sub>i</sub> &lt;= 10<sup>4</sup></code> với mỗi <code>encoded1[i]</code>.</li>
	<li><code>1 &lt;= val<sub>j</sub>, freq<sub>j</sub> &lt;= 10<sup>4</sup></code> với mỗi <code>encoded2[j]</code>.</li>
	<li>Các mảng đầy đủ mà <code>encoded1</code> và <code>encoded2</code> biểu diễn có cùng độ dài.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai encoding run-length biểu diễn các mảng có cùng độ dài; ta cần encoding của tích. Nếu mở rộng chúng, có thể tạo ra tới $10^9$ phần tử.
>
> Hai con trỏ sẽ căn chỉnh các đoạn: lấy $f=\min$ của các tần suất còn lại, sinh giá trị tích $v$, rồi gộp vào đoạn cuối của đáp án nếu có thể. Giảm cả hai tần suất và tăng con trỏ khi một đoạn đã dùng hết. Chỉ dạng nén được duyệt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findRLEArray(
        self, encoded1: List[List[int]], encoded2: List[List[int]]
    ) -> List[List[int]]:
        ans = []
        j = 0
        for vi, fi in encoded1:
            while fi:
                f = min(fi, encoded2[j][1])
                v = vi * encoded2[j][0]
                if ans and ans[-1][0] == v:
                    ans[-1][1] += f
                else:
                    ans.append([v, f])
                fi -= f
                encoded2[j][1] -= f
                if encoded2[j][1] == 0:
                    j += 1
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> findRLEArray(int[][] encoded1, int[][] encoded2) {
        List<List<Integer>> ans = new ArrayList<>();
        int j = 0;
        for (var e : encoded1) {
            int vi = e[0], fi = e[1];
            while (fi > 0) {
                int f = Math.min(fi, encoded2[j][1]);
                int v = vi * encoded2[j][0];
                int m = ans.size();
                if (m > 0 && ans.get(m - 1).get(0) == v) {
                    ans.get(m - 1).set(1, ans.get(m - 1).get(1) + f);
                } else {
                    ans.add(new ArrayList<>(List.of(v, f)));
                }
                fi -= f;
                encoded2[j][1] -= f;
                if (encoded2[j][1] == 0) {
                    ++j;
                }
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
    vector<vector<int>> findRLEArray(vector<vector<int>>& encoded1, vector<vector<int>>& encoded2) {
        vector<vector<int>> ans;
        int j = 0;
        for (auto& e : encoded1) {
            int vi = e[0], fi = e[1];
            while (fi) {
                int f = min(fi, encoded2[j][1]);
                int v = vi * encoded2[j][0];
                if (!ans.empty() && ans.back()[0] == v) {
                    ans.back()[1] += f;
                } else {
                    ans.push_back({v, f});
                }
                fi -= f;
                encoded2[j][1] -= f;
                if (encoded2[j][1] == 0) {
                    ++j;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findRLEArray(encoded1 [][]int, encoded2 [][]int) (ans [][]int) {
	j := 0
	for _, e := range encoded1 {
		vi, fi := e[0], e[1]
		for fi > 0 {
			f := min(fi, encoded2[j][1])
			v := vi * encoded2[j][0]
			if len(ans) > 0 && ans[len(ans)-1][0] == v {
				ans[len(ans)-1][1] += f
			} else {
				ans = append(ans, []int{v, f})
			}
			fi -= f
			encoded2[j][1] -= f
			if encoded2[j][1] == 0 {
				j++
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
