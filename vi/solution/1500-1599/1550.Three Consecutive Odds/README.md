---
comments: true
difficulty: Easy
rating: 1221
source: Weekly Contest 202 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1550. Three Consecutive Odds](https://leetcode.com/problems/three-consecutive-odds)

[中文文档](/solution/1500-1599/1550.Three%20Consecutive%20Odds/README.md)

## Mô tả

<!-- description:start -->

Cho một mảng số nguyên <code>arr</code>, hãy trả về <code>true</code>&nbsp;nếu mảng chứa ba số lẻ liên tiếp. Nếu không, trả về&nbsp;<code>false</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,6,4,1]
<strong>Đầu ra:</strong> false
<b>Giải thích:</b> Không có ba số lẻ liên tiếp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,34,3,4,5,7,23,12]
<strong>Đầu ra:</strong> true
<b>Giải thích:</b> [5,7,23] là ba số lẻ liên tiếp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Xác định xem có tồn tại ba số lẻ liên tiếp hay không. $n$ đủ nhỏ để chỉ cần duyệt một lần; không cần cấu trúc chỉ số.
>
> Một biến đếm theo dõi dãy số lẻ hiện tại: tăng khi gặp số lẻ và trả về kết quả khi đạt $3$; đặt lại về không khi gặp số chẵn. Vì chỉ cần quan tâm tính liên tiếp nên bộ nhớ phụ là hằng số.

<!-- thinking:end -->

Ta dùng biến $\textit{cnt}$ để ghi nhận số lượng số lẻ liên tiếp hiện tại.

Tiếp theo, ta duyệt qua mảng. Nếu phần tử hiện tại là số lẻ, tăng $\textit{cnt}$ lên một. Nếu $\textit{cnt}$ bằng 3 thì trả về $\textit{True}$. Nếu phần tử hiện tại là số chẵn, đặt lại $\textit{cnt}$ về không.

Sau khi duyệt xong, nếu không tìm thấy ba số lẻ liên tiếp thì trả về $\textit{False}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{arr}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def threeConsecutiveOdds(self, arr: List[int]) -> bool:
        cnt = 0
        for x in arr:
            if x & 1:
                cnt += 1
                if cnt == 3:
                    return True
            else:
                cnt = 0
        return False
```

#### Java

```java
class Solution {
    public boolean threeConsecutiveOdds(int[] arr) {
        int cnt = 0;
        for (int x : arr) {
            if (x % 2 == 1) {
                if (++cnt == 3) {
                    return true;
                }
            } else {
                cnt = 0;
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
    bool threeConsecutiveOdds(vector<int>& arr) {
        int cnt = 0;
        for (int x : arr) {
            if (x & 1) {
                if (++cnt == 3) {
                    return true;
                }
            } else {
                cnt = 0;
            }
        }
        return false;
    }
};
```

#### Go

```go
func threeConsecutiveOdds(arr []int) bool {
	cnt := 0
	for _, x := range arr {
		if x&1 == 1 {
			cnt++
			if cnt == 3 {
				return true
			}
		} else {
			cnt = 0
		}
	}
	return false
}
```

#### TypeScript

```ts
function threeConsecutiveOdds(arr: number[]): boolean {
    let cnt = 0;
    for (const x of arr) {
        if (x & 1) {
            if (++cnt == 3) {
                return true;
            }
        } else {
            cnt = 0;
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt + Phép toán bit

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 kiểm tra tính chẵn lẻ từng phần tử. Ba số đều lẻ khi và chỉ khi phép AND bit của chúng có bit thấp nhất bằng một. Thực hiện AND trên từng cửa sổ ba phần tử giúp thay thế biến đếm tường minh.

<!-- thinking:end -->

Dựa trên tính chất của các phép toán bit, kết quả phép AND bit giữa hai số là số lẻ khi và chỉ khi cả hai số đều lẻ. Nếu có ba số liên tiếp có kết quả AND bit là số lẻ thì cả ba số đó đều lẻ.

Do đó, ta chỉ cần duyệt qua mảng và kiểm tra xem có ba số liên tiếp có kết quả AND bit là số lẻ hay không. Nếu tồn tại, trả về $\textit{True}$; nếu không, trả về $\textit{False}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{arr}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def threeConsecutiveOdds(self, arr: List[int]) -> bool:
        return any(x & arr[i + 1] & arr[i + 2] & 1 for i, x in enumerate(arr[:-2]))
```

#### Java

```java
class Solution {
    public boolean threeConsecutiveOdds(int[] arr) {
        for (int i = 2, n = arr.length; i < n; ++i) {
            if ((arr[i - 2] & arr[i - 1] & arr[i] & 1) == 1) {
                return true;
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
    bool threeConsecutiveOdds(vector<int>& arr) {
        for (int i = 2, n = arr.size(); i < n; ++i) {
            if (arr[i - 2] & arr[i - 1] & arr[i] & 1) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func threeConsecutiveOdds(arr []int) bool {
	for i, n := 2, len(arr); i < n; i++ {
		if arr[i-2]&arr[i-1]&arr[i]&1 == 1 {
			return true
		}
	}
	return false
}
```

#### TypeScript

```ts
function threeConsecutiveOdds(arr: number[]): boolean {
    const n = arr.length;
    for (let i = 2; i < n; ++i) {
        if (arr[i - 2] & arr[i - 1] & arr[i] & 1) {
            return true;
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
