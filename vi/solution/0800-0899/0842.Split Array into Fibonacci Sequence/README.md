---
comments: true
difficulty: Medium
tags:
    - String
    - Backtracking
---

<!-- problem:start -->

# [842. Split Array into Fibonacci Sequence](https://leetcode.com/problems/split-array-into-fibonacci-sequence)

[中文文档](/solution/0800-0899/0842.Split%20Array%20into%20Fibonacci%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi chữ số <code>num</code>, chẳng hạn <code>&quot;123456579&quot;</code>. Ta có thể chia chuỗi thành một dãy kiểu Fibonacci <code>[123, 456, 579]</code>.</p>

<p>Chính xác hơn, dãy <strong>kiểu Fibonacci</strong> là danh sách số nguyên không âm <code>f</code> thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= f[i] &lt; 2<sup>31</sup></code>, (nghĩa là mỗi số nguyên nằm trong phạm vi kiểu số nguyên có dấu <strong>32-bit</strong>),</li>
	<li><code>f.length &gt;= 3</code>, và</li>
	<li><code>f[i] + f[i + 1] == f[i + 2]</code> với mọi <code>0 &lt;= i &lt; f.length - 2</code>.</li>
</ul>

<p>Lưu ý, khi chia chuỗi thành các phần, mỗi phần không được có số 0 thừa ở đầu, trừ khi phần đó chính là số <code>0</code>.</p>

<p>Hãy trả về một dãy kiểu Fibonacci bất kỳ được tạo bằng cách chia <code>num</code>, hoặc trả về <code>[]</code> nếu không thể chia được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;1101111&quot;
<strong>Đầu ra:</strong> [11,0,11,11]
<strong>Giải thích:</strong> Kết quả [110, 1, 111] cũng được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;112358130&quot;
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không thể thực hiện yêu cầu này.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;0123&quot;
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không cho phép số 0 ở đầu, nên cách chia &quot;01&quot;, &quot;2&quot;, &quot;3&quot; không hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 200</code></li>
	<li><code>num</code> chỉ chứa các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta chia chuỗi chữ số thành dãy kiểu Fibonacci gồm ít nhất ba số nguyên 32-bit. Độ dài chuỗi $\le 200$; hai số đầu quyết định các số còn lại, nên backtracking là lựa chọn phù hợp.
>
> Thử từng vị trí kết thúc có thể có của số tiếp theo, không cho phép số 0 ở đầu, và cắt nhánh nếu giá trị vượt giới hạn 32-bit hoặc vượt tổng hai số trước đó. Khi đã chọn hai số, số tiếp theo phải bằng tổng của chúng. Thành công khi đi đến cuối chuỗi và đã có hơn hai số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def splitIntoFibonacci(self, num: str) -> List[int]:
        def dfs(i):
            if i == n:
                return len(ans) > 2
            x = 0
            for j in range(i, n):
                if j > i and num[i] == '0':
                    break
                x = x * 10 + int(num[j])
                if x > 2**31 - 1 or (len(ans) > 2 and x > ans[-2] + ans[-1]):
                    break
                if len(ans) < 2 or ans[-2] + ans[-1] == x:
                    ans.append(x)
                    if dfs(j + 1):
                        return True
                    ans.pop()
            return False

        n = len(num)
        ans = []
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer> ans = new ArrayList<>();
    private String num;

    public List<Integer> splitIntoFibonacci(String num) {
        this.num = num;
        dfs(0);
        return ans;
    }

    private boolean dfs(int i) {
        if (i == num.length()) {
            return ans.size() >= 3;
        }
        long x = 0;
        for (int j = i; j < num.length(); ++j) {
            if (j > i && num.charAt(i) == '0') {
                break;
            }
            x = x * 10 + num.charAt(j) - '0';
            if (x > Integer.MAX_VALUE
                || (ans.size() >= 2 && x > ans.get(ans.size() - 1) + ans.get(ans.size() - 2))) {
                break;
            }
            if (ans.size() < 2 || x == ans.get(ans.size() - 1) + ans.get(ans.size() - 2)) {
                ans.add((int) x);
                if (dfs(j + 1)) {
                    return true;
                }
                ans.remove(ans.size() - 1);
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> splitIntoFibonacci(string num) {
        int n = num.size();
        vector<int> ans;
        function<bool(int)> dfs = [&](int i) -> bool {
            if (i == n) {
                return ans.size() > 2;
            }
            long long x = 0;
            for (int j = i; j < n; ++j) {
                if (j > i && num[i] == '0') {
                    break;
                }
                x = x * 10 + num[j] - '0';
                if (x > INT_MAX || (ans.size() > 1 && x > (long long) ans[ans.size() - 1] + ans[ans.size() - 2])) {
                    break;
                }
                if (ans.size() < 2 || x == (long long) ans[ans.size() - 1] + ans[ans.size() - 2]) {
                    ans.push_back(x);
                    if (dfs(j + 1)) {
                        return true;
                    }
                    ans.pop_back();
                }
            }
            return false;
        };
        dfs(0);
        return ans;
    }
};
```

#### Go

```go
func splitIntoFibonacci(num string) []int {
	n := len(num)
	ans := []int{}
	var dfs func(int) bool
	dfs = func(i int) bool {
		if i == n {
			return len(ans) > 2
		}
		x := 0
		for j := i; j < n; j++ {
			if j > i && num[i] == '0' {
				break
			}
			x = x*10 + int(num[j]-'0')
			if x > math.MaxInt32 || (len(ans) > 1 && x > ans[len(ans)-1]+ans[len(ans)-2]) {
				break
			}
			if len(ans) < 2 || x == ans[len(ans)-1]+ans[len(ans)-2] {
				ans = append(ans, x)
				if dfs(j + 1) {
					return true
				}
				ans = ans[:len(ans)-1]
			}
		}
		return false
	}
	dfs(0)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
