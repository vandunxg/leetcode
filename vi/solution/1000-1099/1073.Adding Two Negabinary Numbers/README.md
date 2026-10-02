---
comments: true
difficulty: Medium
rating: 1806
source: Weekly Contest 139 Q3
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [1073. Adding Two Negabinary Numbers](https://leetcode.com/problems/adding-two-negabinary-numbers)

[中文文档](/solution/1000-1099/1073.Adding%20Two%20Negabinary%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số <code>arr1</code> và <code>arr2</code> trong hệ cơ số <strong>-2</strong>, hãy trả về kết quả tổng của chúng.</p>

<p>Mỗi số được cho ở <em>dạng mảng</em>: mảng gồm các bit 0 và 1, theo thứ tự từ bit có trọng số lớn nhất đến bit có trọng số nhỏ nhất. Ví dụ, <code>arr = [1,1,0,1]</code> biểu diễn số <code>(-2)^3 + (-2)^2 + (-2)^0 = -3</code>. Mảng <code>arr</code> cũng được đảm bảo không có số 0 ở đầu: hoặc <code>arr == [0]</code>, hoặc <code>arr[0] == 1</code>.</p>

<p>Trả về tổng của <code>arr1</code> và <code>arr2</code> theo cùng định dạng: mảng gồm các bit 0 và 1, không có số 0 ở đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [1,1,1,1,1], arr2 = [1,0,1]
<strong>Đầu ra:</strong> [1,0,0,0,0]
<strong>Giải thích: </strong>arr1 biểu diễn 11, arr2 biểu diễn 5, kết quả biểu diễn 16.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [0], arr2 = [0]
<strong>Đầu ra:</strong> [0]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [0], arr2 = [1]
<strong>Đầu ra:</strong> [1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr1.length,&nbsp;arr2.length &lt;= 1000</code></li>
	<li><code>arr1[i]</code> và <code>arr2[i]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>arr1</code> và <code>arr2</code> không có số 0 ở đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cộng trong hệ negabinary vẫn duyệt từ bit thấp nhất, nhưng cơ số $-2$ làm thay đổi phần nhớ: $2\times(-2)^i=-(-2)^{i+1}$ và $-(-2)^i=(-2)^i+(-2)^{i+1}$. Độ dài tối đa $\le 1000$ nên có thể mô phỏng từng bit.
>
> Cộng $a+b+c$ từ phải sang trái. Nếu tổng ít nhất là $2$, trừ đi $2$ và nhớ $-1$; nếu tổng bằng $-1$, ghi $1$ và nhớ $1$.
>
> Các bit được thu thập từ thấp lên cao; bỏ các số 0 thừa ở đầu rồi đảo ngược kết quả.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def addNegabinary(self, arr1: List[int], arr2: List[int]) -> List[int]:
        i, j = len(arr1) - 1, len(arr2) - 1
        c = 0
        ans = []
        while i >= 0 or j >= 0 or c:
            a = 0 if i < 0 else arr1[i]
            b = 0 if j < 0 else arr2[j]
            x = a + b + c
            c = 0
            if x >= 2:
                x -= 2
                c -= 1
            elif x == -1:
                x = 1
                c += 1
            ans.append(x)
            i, j = i - 1, j - 1
        while len(ans) > 1 and ans[-1] == 0:
            ans.pop()
        return ans[::-1]
```

#### Java

```java
class Solution {
    public int[] addNegabinary(int[] arr1, int[] arr2) {
        int i = arr1.length - 1, j = arr2.length - 1;
        List<Integer> ans = new ArrayList<>();
        for (int c = 0; i >= 0 || j >= 0 || c != 0; --i, --j) {
            int a = i < 0 ? 0 : arr1[i];
            int b = j < 0 ? 0 : arr2[j];
            int x = a + b + c;
            c = 0;
            if (x >= 2) {
                x -= 2;
                c -= 1;
            } else if (x == -1) {
                x = 1;
                c += 1;
            }
            ans.add(x);
        }
        while (ans.size() > 1 && ans.get(ans.size() - 1) == 0) {
            ans.remove(ans.size() - 1);
        }
        Collections.reverse(ans);
        return ans.stream().mapToInt(x -> x).toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> addNegabinary(vector<int>& arr1, vector<int>& arr2) {
        int i = arr1.size() - 1, j = arr2.size() - 1;
        vector<int> ans;
        for (int c = 0; i >= 0 || j >= 0 || c; --i, --j) {
            int a = i < 0 ? 0 : arr1[i];
            int b = j < 0 ? 0 : arr2[j];
            int x = a + b + c;
            c = 0;
            if (x >= 2) {
                x -= 2;
                c -= 1;
            } else if (x == -1) {
                x = 1;
                c += 1;
            }
            ans.push_back(x);
        }
        while (ans.size() > 1 && ans.back() == 0) {
            ans.pop_back();
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func addNegabinary(arr1 []int, arr2 []int) (ans []int) {
	i, j := len(arr1)-1, len(arr2)-1
	for c := 0; i >= 0 || j >= 0 || c != 0; i, j = i-1, j-1 {
		x := c
		if i >= 0 {
			x += arr1[i]
		}
		if j >= 0 {
			x += arr2[j]
		}
		c = 0
		if x >= 2 {
			x -= 2
			c -= 1
		} else if x == -1 {
			x = 1
			c += 1
		}
		ans = append(ans, x)
	}
	for len(ans) > 1 && ans[len(ans)-1] == 0 {
		ans = ans[:len(ans)-1]
	}
	for i, j = 0, len(ans)-1; i < j; i, j = i+1, j-1 {
		ans[i], ans[j] = ans[j], ans[i]
	}
	return ans
}
```

#### TypeScript

```ts
function addNegabinary(arr1: number[], arr2: number[]): number[] {
    let i = arr1.length - 1,
        j = arr2.length - 1;
    const ans: number[] = [];
    for (let c = 0; i >= 0 || j >= 0 || c; --i, --j) {
        const a = i < 0 ? 0 : arr1[i];
        const b = j < 0 ? 0 : arr2[j];
        let x = a + b + c;
        c = 0;
        if (x >= 2) {
            x -= 2;
            c -= 1;
        } else if (x === -1) {
            x = 1;
            c += 1;
        }
        ans.push(x);
    }
    while (ans.length > 1 && ans[ans.length - 1] === 0) {
        ans.pop();
    }
    return ans.reverse();
}
```

#### C#

```cs
public class Solution {
    public int[] AddNegabinary(int[] arr1, int[] arr2) {
        int i = arr1.Length - 1, j = arr2.Length - 1;
        List<int> ans = new List<int>();
        for (int c = 0; i >= 0 || j >= 0 || c != 0; --i, --j) {
            int a = i < 0 ? 0 : arr1[i];
            int b = j < 0 ? 0 : arr2[j];
            int x = a + b + c;
            c = 0;
            if (x >= 2) {
                x -= 2;
                c -= 1;
            } else if (x == -1) {
                x = 1;
                c = 1;
            }
            ans.Add(x);
        }
        while (ans.Count > 1 && ans[ans.Count - 1] == 0) {
            ans.RemoveAt(ans.Count - 1);
        }
        ans.Reverse();
        return ans.ToArray();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
