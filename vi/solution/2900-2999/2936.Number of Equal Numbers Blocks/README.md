---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Interactive
---

<!-- problem:start -->

# [2936. Number of Equal Numbers Blocks 🔒](https://leetcode.com/problems/number-of-equal-numbers-blocks)

[中文文档](/solution/2900-2999/2936.Number%20of%20Equal%20Numbers%20Blocks/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>. Mảng <code>nums</code> có tính chất sau:</p>

<ul>
	<li>Mọi lần xuất hiện của một giá trị đều nằm liền kề nhau. Nói cách khác, nếu có hai chỉ số <code>i &lt; j</code> sao cho <code>nums[i] == nums[j]</code>, thì với mọi chỉ số <code>k</code> thỏa mãn <code>i &lt; k &lt; j</code>, <code>nums[k] == nums[i]</code>.</li>
</ul>

<p>Vì <code>nums</code> là một mảng rất lớn, bạn được cung cấp một đối tượng của lớp <code>BigArray</code> có các hàm sau:</p>

<ul>
	<li><code>int at(long long index)</code>: Trả về giá trị của <code>nums[i]</code>.</li>
	<li><code>void size()</code>: Trả về <code>nums.length</code>.</li>
</ul>

<p>Hãy chia mảng thành các block <strong>cực đại</strong> sao cho mỗi block chứa các <strong>giá trị bằng nhau</strong>. Trả về <em>số lượng các block này.</em></p>

<p><strong>Lưu ý</strong> rằng nếu muốn kiểm thử lời giải bằng một test tùy chỉnh, hành vi đối với các test có <code>nums.length &gt; 10</code> là không xác định.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,3,3,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ở đây chỉ có một block là toàn bộ mảng (vì mọi số đều bằng nhau), đó là: [<u>3,3,3,3,3</u>]. Vì vậy, đáp án là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,3,9,9,9,2,10,10]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Có 5 block ở đây:
Block số 1: [<u>1,1,1</u>,3,9,9,9,2,10,10]
Block số 2: [1,1,1,<u>3</u>,9,9,9,2,10,10]
Block số 3: [1,1,1,3,<u>9,9,9</u>,2,10,10]
Block số 4: [1,1,1,3,9,9,9,<u>2</u>,10,10]
Block số 5: [1,1,1,3,9,9,9,2,<u>10,10</u>]
Vì vậy, đáp án là 5.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5,6,7]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Vì mọi số đều khác nhau, ở đây có 7 block và mỗi phần tử tạo thành một block. Vì vậy, đáp án là 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>15</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li>Dữ liệu đầu vào được tạo sao cho mọi giá trị bằng nhau đều nằm liền kề.</li>
	<li>Tổng các phần tử của <code>nums</code> không vượt quá <code>10<sup>15</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mảng là một interface có độ dài lên tới $10^{15}$, nên không thể duyệt tuyến tính. Các giá trị bằng nhau đã tạo thành từng block, và mỗi block chỉ cần xác định vị trí kết thúc bên phải. Tìm kiếm nhị phân trên $[i,n)$ giúp tìm chỉ số đầu tiên có giá trị khác với $nums.at(i)$.
>
> Nếu hai ô liền kề đã khác nhau, ta chỉ cần tăng một đơn vị và bỏ qua việc tìm kiếm. Số lần truy vấn tỷ lệ với số block nhân với một logarit.

<!-- thinking:end -->

Ta có thể dùng tìm kiếm nhị phân để tìm ranh giới bên phải của mỗi block. Cụ thể, ta duyệt mảng từ trái sang phải. Với mỗi chỉ số $i$, ta dùng tìm kiếm nhị phân để tìm chỉ số nhỏ nhất $j$ sao cho mọi phần tử trong $[i,j)$ đều bằng $nums[i]$. Sau đó, ta cập nhật $i$ thành $j$ và tiếp tục duyệt mảng cho đến khi $i$ lớn hơn hoặc bằng độ dài mảng.

Độ phức tạp thời gian là $O(m \times \log n)$, trong đó $m$ là số phần tử khác nhau trong mảng $num$, còn $n$ là độ dài mảng $num$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for BigArray.
# class BigArray:
#     def at(self, index: long) -> int:
#         pass
#     def size(self) -> long:
#         pass
class Solution(object):
    def countBlocks(self, nums: Optional["BigArray"]) -> int:
        i, n = 0, nums.size()
        ans = 0
        while i < n:
            ans += 1
            x = nums.at(i)
            if i + 1 < n and nums.at(i + 1) != x:
                i += 1
            else:
                i += bisect_left(range(i, n), True, key=lambda j: nums.at(j) != x)
        return ans
```

#### Java

```java
/**
 * Definition for BigArray.
 * class BigArray {
 *     public BigArray(int[] elements);
 *     public int at(long index);
 *     public long size();
 * }
 */
class Solution {
    public int countBlocks(BigArray nums) {
        int ans = 0;
        for (long i = 0, n = nums.size(); i < n; ++ans) {
            i = search(nums, i, n);
        }
        return ans;
    }

    private long search(BigArray nums, long l, long n) {
        long r = n;
        int x = nums.at(l);
        while (l < r) {
            long mid = (l + r) >> 1;
            if (nums.at(mid) != x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
/**
 * Definition for BigArray.
 * class BigArray {
 * public:
 *     BigArray(vector<int> elements);
 *     int at(long long index);
 *     long long size();
 * };
 */
class Solution {
public:
    int countBlocks(BigArray* nums) {
        int ans = 0;
        using ll = long long;
        ll n = nums->size();
        auto search = [&](ll l) {
            ll r = n;
            int x = nums->at(l);
            while (l < r) {
                ll mid = (l + r) >> 1;
                if (nums->at(mid) != x) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            return l;
        };
        for (long long i = 0; i < n; ++ans) {
            i = search(i);
        }
        return ans;
    }
};
```

#### TypeScript

```ts
/**
 * Definition for BigArray.
 * class BigArray {
 *     constructor(elements: number[]);
 *     public at(index: number): number;
 *     public size(): number;
 * }
 */
function countBlocks(nums: BigArray | null): number {
    const n = nums.size();
    const search = (l: number): number => {
        let r = n;
        const x = nums.at(l);
        while (l < r) {
            const mid = l + Math.floor((r - l) / 2);
            if (nums.at(mid) !== x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };

    let ans = 0;
    for (let i = 0; i < n; ++ans) {
        i = search(i);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Chia để trị

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm nhị phân cần truy cập ngẫu nhiên. Khi chia một đoạn thành hai nửa, tổng số block của hai nửa sẽ đếm thừa một nếu hai giá trị ở giữa bằng nhau, nên ta trừ đi phần đó. Phương pháp chia để trị cũng chỉ truy vấn các vị trí gần rìa block.
>
> Các đoạn đơn giản (chẳng hạn đoạn có độ dài $2$ với hai đầu mút khác nhau) được trả về ngay. So với phương pháp 1, vòng lặp bên ngoài “tìm vị trí kết thúc bên phải” được thay thế bằng một phép gộp đệ quy.

<!-- thinking:end -->

Ta có thể dùng phương pháp chia để trị để tính đáp án. Cụ thể, ta chia mảng thành hai mảng con, đệ quy tính đáp án cho từng mảng con, rồi gộp các đáp án lại. Nếu phần tử cuối của mảng con thứ nhất bằng phần tử đầu của mảng con thứ hai, ta cần trừ một khỏi đáp án.

Độ phức tạp thời gian là $O(\log n)$, và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài mảng $num$.

<!-- tabs:start -->

#### Java

```java
/**
 * Definition for BigArray.
 * class BigArray {
 *     public BigArray(int[] elements);
 *     public int at(long index);
 *     public long size();
 * }
 */
class Solution {
    public int countBlocks(BigArray nums) {
        return f(nums, 0, nums.size() - 1);
    }

    private int f(BigArray nums, long l, long r) {
        if (nums.at(l) == nums.at(r)) {
            return 1;
        }
        long mid = (l + r) >> 1;
        int a = f(nums, l, mid);
        int b = f(nums, mid + 1, r);
        return a + b - (nums.at(mid) == nums.at(mid + 1) ? 1 : 0);
    }
}
```

#### C++

```cpp
/**
 * Definition for BigArray.
 * class BigArray {
 * public:
 *     BigArray(vector<int> elements);
 *     int at(long long index);
 *     long long size();
 * };
 */
class Solution {
public:
    int countBlocks(BigArray* nums) {
        using ll = long long;
        function<int(ll, ll)> f = [&](ll l, ll r) {
            if (nums->at(l) == nums->at(r)) {
                return 1;
            }
            ll mid = (l + r) >> 1;
            int a = f(l, mid);
            int b = f(mid + 1, r);
            return a + b - (nums->at(mid) == nums->at(mid + 1));
        };
        return f(0, nums->size() - 1);
    }
};
```

#### TypeScript

```ts
/**
 * Definition for BigArray.
 * class BigArray {
 *     constructor(elements: number[]);
 *     public at(index: number): number;
 *     public size(): number;
 * }
 */
function countBlocks(nums: BigArray | null): number {
    const f = (l: number, r: number): number => {
        if (nums.at(l) === nums.at(r)) {
            return 1;
        }
        const mid = l + Math.floor((r - l) / 2);
        const a = f(l, mid);
        const b = f(mid + 1, r);
        return a + b - (nums.at(mid) === nums.at(mid + 1) ? 1 : 0);
    };
    return f(0, nums.size() - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
