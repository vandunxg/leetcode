---
comments: true
difficulty: Medium
rating: 2134
source: Weekly Contest 497 Q3
tags:
    - Hash Table
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [3900. Longest Balanced Substring After One Swap](https://leetcode.com/problems/longest-balanced-substring-after-one-swap)

[中文文档](/solution/3900-3999/3900.Longest%20Balanced%20Substring%20After%20One%20Swap/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi nhị phân <code>s</code> chỉ gồm các ký tự <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</p>

<p>Một chuỗi được gọi là <strong>cân bằng</strong> nếu chứa số lượng <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code> <strong>bằng nhau</strong>.</p>

<p>Bạn có thể thực hiện <strong>nhiều nhất một</strong> phép đổi chỗ giữa hai ký tự bất kỳ trong <code>s</code>. Sau đó, chọn một <span data-keyword="substring">chuỗi con</span> <strong>cân bằng</strong> từ <code>s</code>.</p>

<p>Hãy trả về một số nguyên biểu thị độ dài <strong>lớn nhất</strong> của chuỗi con <strong>cân bằng</strong> mà bạn có thể chọn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;100001&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đổi chỗ <code>&quot;10<u><strong>0</strong></u>00<u><strong>1</strong></u>&quot;</code>. Chuỗi trở thành <code>&quot;101000&quot;</code>.</li>
	<li>Chọn chuỗi con <code>&quot;<u><strong>1010</strong></u>00&quot;</code>, chuỗi này cân bằng vì có hai ký tự <code>&#39;0&#39;</code> và hai ký tự <code>&#39;1&#39;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;111&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn không thực hiện phép đổi chỗ nào.</li>
	<li>Chọn chuỗi con rỗng, chuỗi này cân bằng vì không có ký tự <code>&#39;0&#39;</code> và không có ký tự <code>&#39;1&#39;</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các ký tự <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê một phép đổi chỗ rồi duyệt mọi chuỗi con sẽ có độ phức tạp $O(n^3)$, không thể thực hiện được khi $n\le 10^5$. Ngay cả việc dùng hai con trỏ để duyệt các đầu mút vẫn phải xử lý ảnh hưởng toàn cục của phép đổi chỗ.
>
> Một chuỗi con cân bằng có số lượng $0$ và $1$ bằng nhau, tức là hiệu prefix (tính $1$ là $+1$ và $0$ là $-1$) không đổi. Một phép đổi chỗ có thể thay đổi hiệu này đi $2$, vì vậy ngoài các cặp prefix bằng nhau, ta còn phải xét các prefix chênh nhau $\pm 2$, với điều kiện ký tự bổ sung tương ứng vẫn tồn tại bên ngoài đoạn.
>
> Một hash map lưu tất cả vị trí của mỗi prefix là đủ: lần xuất hiện sớm nhất cho ứng viên dài nhất, còn nếu đoạn đó không thể lấy ký tự cần thiết từ bên ngoài thì ta chuyển sang chỉ số xuất hiện sớm tiếp theo.

<!-- thinking:end -->

Gọi prefix sum $\textit{pre}$ là số lượng `1` trừ số lượng `0` trong prefix hiện tại. Khi một chuỗi con có số lượng `0` và `1` bằng nhau, hiệu prefix sum tương ứng của nó bằng $0$.

Do đó, nếu prefix sum tại vị trí $i$ là $x$, và một vị trí trước đó cũng có prefix sum bằng $x$, thì chuỗi con giữa hai vị trí này là chuỗi cân bằng, và ta có thể dùng trực tiếp để cập nhật đáp án.

Trong bài toán này, ta được phép thực hiện nhiều nhất một phép đổi chỗ giữa hai ký tự bất kỳ. Một phép đổi chỗ chỉ có thể giảm chênh lệch giữa số lượng `1` và `0` trong chuỗi con đi $2$. Vì vậy, ngoài trường hợp hiệu prefix sum bằng $0$, ta còn cần xét:

- Hiệu prefix sum bằng $2$, nghĩa là chuỗi con chứa nhiều hơn `0` đúng 2 ký tự `1`. Trong trường hợp này, nếu vẫn còn ít nhất một `0` bên ngoài chuỗi con, ta có thể làm nó cân bằng bằng một phép đổi chỗ.
- Hiệu prefix sum bằng $-2$, nghĩa là chuỗi con chứa nhiều hơn `1` đúng 2 ký tự `0`. Tương tự, nếu vẫn còn ít nhất một `1` bên ngoài chuỗi con, ta có thể làm nó cân bằng bằng một phép đổi chỗ.

Để làm điều này, trước tiên ta đếm tổng số `0` và `1` trong cả chuỗi, lần lượt ký hiệu là $\textit{cnt0}$ và $\textit{cnt1}$. Sau đó, ta dùng một bảng băm để ghi lại tất cả vị trí mà mỗi prefix sum xuất hiện.

Trong khi duyệt chuỗi đến vị trí $i$, gọi prefix sum hiện tại là $\textit{pre}$:

- Dùng lần xuất hiện sớm nhất của $\textit{pre}$ để cập nhật độ dài chuỗi con cân bằng dài nhất mà không cần đổi chỗ.
- Nếu tồn tại prefix sum $\textit{pre} - 2$, ta có thể thử tạo chuỗi con có nhiều hơn `0` đúng 2 ký tự `1`. Giả sử độ dài của nó là $L$. Khi đó, số lượng `0` bên trong là $(L - 2) / 2$. Chỉ khi giá trị này nhỏ hơn nghiêm ngặt $\textit{cnt0}$ thì ta mới biết có ít nhất một `0` bên ngoài chuỗi con để đổi vào.
- Nếu tồn tại prefix sum $\textit{pre} + 2$, ta có thể tương tự thử tạo chuỗi con có nhiều hơn `1` đúng 2 ký tự `0`. Trong trường hợp này, số lượng `1` bên trong phải nhỏ hơn nghiêm ngặt $\textit{cnt1}$.

Vì một lần xuất hiện sớm hơn của cùng một prefix sum tạo ra chuỗi con dài hơn, ta luôn thử vị trí sớm nhất trước. Nếu vị trí đó không thỏa mãn điều kiện vẫn còn một ký tự bên ngoài chuỗi con để đổi vào, ta thử vị trí xuất hiện sớm thứ hai.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestBalanced(self, s: str) -> int:
        cnt0 = s.count("0")
        cnt1 = len(s) - cnt0
        pos = {0: [-1]}
        ans = pre = 0
        for i, c in enumerate(s):
            pre += 1 if c == "1" else -1
            pos.setdefault(pre, []).append(i)

            ans = max(ans, i - pos[pre][0])
            if pre - 2 in pos:
                p = pos[pre - 2]
                if (i - p[0] - 2) // 2 < cnt0:
                    ans = max(ans, i - p[0])
                elif len(p) > 1:
                    ans = max(ans, i - p[1])

            if pre + 2 in pos:
                p = pos[pre + 2]
                if (i - p[0] - 2) // 2 < cnt1:
                    ans = max(ans, i - p[0])
                elif len(p) > 1:
                    ans = max(ans, i - p[1])
        return ans
```

#### Java

```java
class Solution {
    public int longestBalanced(String s) {
        int cnt0 = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == '0') {
                ++cnt0;
            }
        }
        int cnt1 = s.length() - cnt0;
        Map<Integer, List<Integer>> pos = new HashMap<>();
        pos.put(0, new ArrayList<>(List.of(-1)));
        int ans = 0;
        int pre = 0;
        for (int i = 0; i < s.length(); ++i) {
            pre += s.charAt(i) == '1' ? 1 : -1;
            pos.computeIfAbsent(pre, k -> new ArrayList<>()).add(i);

            ans = Math.max(ans, i - pos.get(pre).get(0));
            if (pos.containsKey(pre - 2)) {
                List<Integer> p = pos.get(pre - 2);
                if ((i - p.get(0) - 2) / 2 < cnt0) {
                    ans = Math.max(ans, i - p.get(0));
                } else if (p.size() > 1) {
                    ans = Math.max(ans, i - p.get(1));
                }
            }

            if (pos.containsKey(pre + 2)) {
                List<Integer> p = pos.get(pre + 2);
                if ((i - p.get(0) - 2) / 2 < cnt1) {
                    ans = Math.max(ans, i - p.get(0));
                } else if (p.size() > 1) {
                    ans = Math.max(ans, i - p.get(1));
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
    int longestBalanced(string s) {
        int cnt0 = count(s.begin(), s.end(), '0');
        int cnt1 = s.size() - cnt0;
        unordered_map<int, vector<int>> pos;
        pos[0] = {-1};
        int ans = 0, pre = 0;
        for (int i = 0; i < s.size(); ++i) {
            pre += s[i] == '1' ? 1 : -1;
            pos[pre].push_back(i);

            ans = max(ans, i - pos[pre][0]);
            if (pos.contains(pre - 2)) {
                auto& p = pos[pre - 2];
                if ((i - p[0] - 2) / 2 < cnt0) {
                    ans = max(ans, i - p[0]);
                } else if (p.size() > 1) {
                    ans = max(ans, i - p[1]);
                }
            }

            if (pos.contains(pre + 2)) {
                auto& p = pos[pre + 2];
                if ((i - p[0] - 2) / 2 < cnt1) {
                    ans = max(ans, i - p[0]);
                } else if (p.size() > 1) {
                    ans = max(ans, i - p[1]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestBalanced(s string) int {
	cnt0 := strings.Count(s, "0")
	cnt1 := len(s) - cnt0
	pos := map[int][]int{0: {-1}}
	ans, pre := 0, 0
	for i, c := range s {
		if c == '1' {
			pre++
		} else {
			pre--
		}
		pos[pre] = append(pos[pre], i)

		ans = max(ans, i-pos[pre][0])
		if p, ok := pos[pre-2]; ok {
			if (i-p[0]-2)/2 < cnt0 {
				ans = max(ans, i-p[0])
			} else if len(p) > 1 {
				ans = max(ans, i-p[1])
			}
		}

		if p, ok := pos[pre+2]; ok {
			if (i-p[0]-2)/2 < cnt1 {
				ans = max(ans, i-p[0])
			} else if len(p) > 1 {
				ans = max(ans, i-p[1])
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function longestBalanced(s: string): number {
    const cnt0 = [...s].filter(c => c === '0').length;
    const cnt1 = s.length - cnt0;
    const pos = new Map<number, number[]>();
    pos.set(0, [-1]);
    let ans = 0;
    let pre = 0;
    for (let i = 0; i < s.length; ++i) {
        pre += s[i] === '1' ? 1 : -1;
        if (!pos.has(pre)) {
            pos.set(pre, []);
        }
        pos.get(pre)!.push(i);

        ans = Math.max(ans, i - pos.get(pre)![0]);
        if (pos.has(pre - 2)) {
            const p = pos.get(pre - 2)!;
            if ((i - p[0] - 2) >> 1 < cnt0) {
                ans = Math.max(ans, i - p[0]);
            } else if (p.length > 1) {
                ans = Math.max(ans, i - p[1]);
            }
        }

        if (pos.has(pre + 2)) {
            const p = pos.get(pre + 2)!;
            if ((i - p[0] - 2) >> 1 < cnt1) {
                ans = Math.max(ans, i - p[0]);
            } else if (p.length > 1) {
                ans = Math.max(ans, i - p[1]);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
