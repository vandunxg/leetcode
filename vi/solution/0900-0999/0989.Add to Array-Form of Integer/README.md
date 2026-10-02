---
comments: true
difficulty: Easy
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [989. Add to Array-Form of Integer](https://leetcode.com/problems/add-to-array-form-of-integer)

[中文文档](/solution/0900-0999/0989.Add%20to%20Array-Form%20of%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Dạng mảng</strong> của số nguyên <code>num</code> là một mảng biểu diễn các chữ số theo thứ tự từ trái sang phải.</p>

<ul>
	<li>Ví dụ, với <code>num = 1321</code>, dạng mảng là <code>[1,3,2,1]</code>.</li>
</ul>

<p>Cho <code>num</code> ở <strong>dạng mảng</strong> và số nguyên <code>k</code>, hãy trả về <em><strong>dạng mảng</strong> của số nguyên</em> <code>num + k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = [1,2,0,0], k = 34
<strong>Đầu ra:</strong> [1,2,3,4]
<strong>Giải thích:</strong> 1200 + 34 = 1234
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = [2,7,4], k = 181
<strong>Đầu ra:</strong> [4,5,5]
<strong>Giải thích:</strong> 274 + 181 = 455
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = [2,1,5], k = 806
<strong>Đầu ra:</strong> [1,0,2,1]
<strong>Giải thích:</strong> 215 + 806 = 1021
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= num[i] &lt;= 9</code></li>
	<li><code>num</code> không có chữ số 0 ở đầu, trừ trường hợp chính số đó là 0.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Cộng $k$ vào số nguyên được lưu dưới dạng mảng chữ số, đồng thời xử lý phần nhớ. Bắt đầu từ chữ số thấp nhất, cộng $k$ vào hàng hiện tại, lấy thương làm phần nhớ rồi tiếp tục cho đến khi đã duyệt hết mảng và $k$ bằng 0. Các chữ số được tạo ra từ thấp đến cao nên cuối cùng cần đảo ngược kết quả.

<!-- thinking:end -->

Ta có thể bắt đầu từ chữ số cuối của mảng và cộng từng chữ số vào $k$. Sau đó, chia $k$ cho $10$, lấy số dư làm giá trị chữ số hiện tại và thương làm phần nhớ. Tiếp tục quá trình này cho đến khi duyệt hết mảng và $k = 0$. Cuối cùng, đảo ngược mảng kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của $\textit{num}$. Nếu không tính phần bộ nhớ dùng cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def addToArrayForm(self, num: List[int], k: int) -> List[int]:
        ans = []
        i = len(num) - 1
        while i >= 0 or k:
            k += 0 if i < 0 else num[i]
            k, x = divmod(k, 10)
            ans.append(x)
            i -= 1
        return ans[::-1]
```

#### Java

```java
class Solution {
    public List<Integer> addToArrayForm(int[] num, int k) {
        List<Integer> ans = new ArrayList<>();
        for (int i = num.length - 1; i >= 0 || k > 0; --i) {
            k += (i >= 0 ? num[i] : 0);
            ans.add(k % 10);
            k /= 10;
        }
        Collections.reverse(ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> addToArrayForm(vector<int>& num, int k) {
        vector<int> ans;
        for (int i = num.size() - 1; i >= 0 || k > 0; --i) {
            k += (i >= 0 ? num[i] : 0);
            ans.push_back(k % 10);
            k /= 10;
        }
        ranges::reverse(ans);
        return ans;
    }
};
```

#### Go

```go
func addToArrayForm(num []int, k int) (ans []int) {
	for i := len(num) - 1; i >= 0 || k > 0; i-- {
		if i >= 0 {
			k += num[i]
		}
		ans = append(ans, k%10)
		k /= 10
	}
	slices.Reverse(ans)
	return
}
```

#### TypeScript

```ts
function addToArrayForm(num: number[], k: number): number[] {
    const ans: number[] = [];
    for (let i = num.length - 1; i >= 0 || k > 0; --i) {
        k += i >= 0 ? num[i] : 0;
        ans.push(k % 10);
        k = Math.floor(k / 10);
    }
    return ans.reverse();
}
```

#### Rust

```rust
impl Solution {
    pub fn add_to_array_form(num: Vec<i32>, k: i32) -> Vec<i32> {
        let mut ans = Vec::new();
        let mut k = k;
        let mut i = num.len() as i32 - 1;

        while i >= 0 || k > 0 {
            if i >= 0 {
                k += num[i as usize];
            }
            ans.push(k % 10);
            k /= 10;
            i -= 1;
        }

        ans.reverse();
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
