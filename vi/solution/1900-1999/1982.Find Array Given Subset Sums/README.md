---
comments: true
difficulty: Hard
rating: 2872
source: Weekly Contest 255 Q4
tags:
    - Array
    - Hash Table
    - Counting
    - Sorting
---

<!-- problem:start -->

# [1982. Find Array Given Subset Sums](https://leetcode.com/problems/find-array-given-subset-sums)

[中文文档](/solution/1900-1999/1982.Find%20Array%20Given%20Subset%20Sums/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> đại diện cho độ dài của một mảng chưa biết mà bạn cần khôi phục. Bạn cũng được cho một mảng <code>sums</code> chứa giá trị của tất cả <code>2<sup>n</sup></code> <strong>tổng tập con</strong> của mảng chưa biết (không theo thứ tự cụ thể).</p>

<p>Trả về <em>mảng </em><code>ans</code><em> có độ dài </em><code>n</code><em> đại diện cho mảng chưa biết. Nếu tồn tại <strong>nhiều</strong> đáp án, hãy trả về <strong>bất kỳ</strong> đáp án nào</em>.</p>

<p>Một mảng <code>sub</code> là <strong>tập con</strong> của một mảng <code>arr</code> nếu có thể thu được <code>sub</code> từ <code>arr</code> bằng cách xóa một số phần tử (có thể là không xóa hoặc xóa tất cả) khỏi <code>arr</code>. Tổng các phần tử trong <code>sub</code> là một <strong>tổng tập con</strong> có thể có của <code>arr</code>. Tổng của một mảng rỗng được xem là <code>0</code>.</p>

<p><strong>Lưu ý:</strong> Các test case được tạo sao cho <strong>luôn</strong> tồn tại ít nhất một đáp án đúng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, sums = [-3,-2,-1,0,0,1,2,3]
<strong>Đầu ra:</strong> [1,2,-3]
<strong>Giải thích: </strong>[1,2,-3] có thể tạo ra các tổng tập con đã cho:
- []: tổng là 0
- [1]: tổng là 1
- [2]: tổng là 2
- [1,2]: tổng là 3
- [-3]: tổng là -3
- [1,-3]: tổng là -2
- [2,-3]: tổng là -1
- [1,2,-3]: tổng là 0
Lưu ý rằng mọi hoán vị của [1,2,-3] cũng như mọi hoán vị của [-1,-2,3] đều được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, sums = [0,0,0,0]
<strong>Đầu ra:</strong> [0,0]
<strong>Giải thích:</strong> Đáp án đúng duy nhất là [0,0].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, sums = [0,0,5,5,4,-1,4,9,9,-1,4,3,4,8,3,8]
<strong>Đầu ra:</strong> [0,-1,4,5]
<strong>Giải thích:</strong> [0,-1,4,5] có thể tạo ra các tổng tập con đã cho.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 15</code></li>
	<li><code>sums.length == 2<sup>n</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= sums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tập hợp có thứ tự

<!-- thinking:start -->

> **Tư duy**
>
> Ta được cho tất cả $2^n$ tổng tập con và cần khôi phục mảng. Dịch sao cho tổng nhỏ nhất bằng $0$; phần tử nhỏ nhất tiếp theo trong multiset đã sắp xếp là một phần tử dương chưa biết.
>
> Mỗi giá trị mới sẽ xóa mọi tổng cũ cộng với giá trị đó. Nếu phần tử dịch là một tổng tập con, hãy đổi dấu các phần tử đó để hoàn tác phép dịch.

<!-- thinking:end -->

Dịch các tổng tập con về không âm, sau đó liên tục chọn tổng nhỏ nhất còn lại làm giá trị tiếp theo.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def recoverArray(self, n: int, sums: List[int]) -> List[int]:
        m = -min(sums)
        sl = SortedList(x + m for x in sums)
        sl.remove(0)
        ans = [sl[0]]
        for i in range(1, n):
            for j in range(1 << i):
                if j >> (i - 1) & 1:
                    s = sum(ans[k] for k in range(i) if j >> k & 1)
                    sl.remove(s)
            ans.append(sl[0])
        for i in range(1 << n):
            s = sum(ans[j] for j in range(n) if i >> j & 1)
            if s == m:
                for j in range(n):
                    if i >> j & 1:
                        ans[j] *= -1
                break
        return ans
```

#### Java

```java
class Solution {
    public int[] recoverArray(int n, int[] sums) {
        int m = 1 << 30;
        for (int x : sums) {
            m = Math.min(m, x);
        }
        m = -m;
        TreeMap<Integer, Integer> tm = new TreeMap<>();
        for (int x : sums) {
            tm.merge(x + m, 1, Integer::sum);
        }
        int[] ans = new int[n];
        if (tm.merge(0, -1, Integer::sum) == 0) {
            tm.remove(0);
        }
        ans[0] = tm.firstKey();
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < 1 << i; ++j) {
                if ((j >> (i - 1) & 1) == 1) {
                    int s = 0;
                    for (int k = 0; k < i; ++k) {
                        if (((j >> k) & 1) == 1) {
                            s += ans[k];
                        }
                    }
                    if (tm.merge(s, -1, Integer::sum) == 0) {
                        tm.remove(s);
                    }
                }
            }
            ans[i] = tm.firstKey();
        }
        for (int i = 0; i < 1 << n; ++i) {
            int s = 0;
            for (int j = 0; j < n; ++j) {
                if (((i >> j) & 1) == 1) {
                    s += ans[j];
                }
            }
            if (s == m) {
                for (int j = 0; j < n; ++j) {
                    if (((i >> j) & 1) == 1) {
                        ans[j] *= -1;
                    }
                }
                break;
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
    vector<int> recoverArray(int n, vector<int>& sums) {
        int m = *min_element(sums.begin(), sums.end());
        m = -m;
        multiset<int> st;
        for (int x : sums) {
            st.insert(x + m);
        }
        st.erase(st.begin());
        vector<int> ans;
        ans.push_back(*st.begin());
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < 1 << i; ++j) {
                if (j >> (i - 1) & 1) {
                    int s = 0;
                    for (int k = 0; k < i; ++k) {
                        if (j >> k & 1) {
                            s += ans[k];
                        }
                    }
                    st.erase(st.find(s));
                }
            }
            ans.push_back(*st.begin());
        }
        for (int i = 0; i < 1 << n; ++i) {
            int s = 0;
            for (int j = 0; j < n; ++j) {
                if (i >> j & 1) {
                    s += ans[j];
                }
            }
            if (s == m) {
                for (int j = 0; j < n; ++j) {
                    if (i >> j & 1) {
                        ans[j] = -ans[j];
                    }
                }
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func recoverArray(n int, sums []int) []int {
	m := -slices.Min(sums)
	rbt := redblacktree.NewWithIntComparator()
	merge := func(key int, value int) {
		if v, ok := rbt.Get(key); ok {
			nxt := v.(int) + value
			if nxt == 0 {
				rbt.Remove(key)
			} else {
				rbt.Put(key, nxt)
			}
		} else {
			rbt.Put(key, value)
		}
	}
	for _, x := range sums {
		merge(x+m, 1)
	}
	ans := make([]int, n)
	merge(ans[0], -1)
	ans[0] = rbt.Left().Key.(int)
	for i := 1; i < n; i++ {
		for j := 0; j < 1<<i; j++ {
			if j>>(i-1)&1 == 1 {
				s := 0
				for k := 0; k < i; k++ {
					if j>>k&1 == 1 {
						s += ans[k]
					}
				}
				merge(s, -1)
			}
		}
		ans[i] = rbt.Left().Key.(int)
	}
	for i := 0; i < 1<<n; i++ {
		s := 0
		for j := 0; j < n; j++ {
			if i>>j&1 == 1 {
				s += ans[j]
			}
		}
		if s == m {
			for j := 0; j < n; j++ {
				if i>>j&1 == 1 {
					ans[j] = -ans[j]
				}
			}
			break
		}
	}
	return ans

}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Việc xóa phần tử trong sorted list phải trả thêm một thừa số log. Sau khi sắp xếp, hiệu giữa hai tổng lớn nhất là giá trị tuyệt đối hiện tại $d$; một bộ đếm chia các tổng thành nhóm không chứa và chứa $d$, còn nửa chứa $0$ sẽ xác định dấu.

<!-- thinking:end -->

Khôi phục giá trị tuyệt đối tiếp theo từ hiệu giữa các phần tử kề nhau, rồi dùng bộ đếm để chia các tổng tập con.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def recoverArray(self, n: int, sums: List[int]) -> List[int]:
        sums.sort()
        ans = []
        for i in range(n, 0, -1):
            k = 1 << i
            d = sums[k - 1] - sums[k - 2]
            cnt = Counter(sums[:k])
            sums1, sums2 = [], []
            sign = 1
            for s in sums[:k]:
                if not cnt[s]:
                    continue
                cnt[s] -= 1
                cnt[s + d] -= 1
                sums1.append(s)
                sums2.append(s + d)
                if s + d == 0:
                    sign = -1
            ans.append(sign * d)
            sums = sums1 if sign == 1 else sums2
        return ans
```

#### Java

```java
class Solution {
    public int[] recoverArray(int n, int[] sums) {
        Arrays.sort(sums);
        int[] sums1 = new int[1 << n];
        int[] sums2 = new int[1 << n];
        Map<Integer, Integer> cnt = new HashMap<>();
        int[] ans = new int[n];
        for (int i = n; i > 0; --i) {
            int k = 1 << i;
            int d = sums[k - 1] - sums[k - 2];
            cnt.clear();
            for (int j = 0; j < k; ++j) {
                cnt.merge(sums[j], 1, Integer::sum);
            }
            int sign = 1;
            for (int j = 0, p = 0; j < k; ++j) {
                if (cnt.getOrDefault(sums[j], 0) == 0) {
                    continue;
                }
                cnt.merge(sums[j], -1, Integer::sum);
                cnt.merge(sums[j] + d, -1, Integer::sum);
                sums1[p] = sums[j];
                sums2[p++] = sums[j] + d;
                if (sums[j] + d == 0) {
                    sign = -1;
                }
            }
            ans[i - 1] = sign * d;
            System.arraycopy(sign == 1 ? sums1 : sums2, 0, sums, 0, k / 2);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> recoverArray(int n, vector<int>& sums) {
        sort(sums.begin(), sums.end());
        vector<int> ans(n);
        unordered_map<int, int> cnt;
        for (int i = n; i; --i) {
            cnt.clear();
            int k = 1 << i;
            int d = sums[k - 1] - sums[k - 2];
            for (int j = 0; j < k; ++j) {
                cnt[sums[j]]++;
            }
            vector<int> sums1, sums2;
            int sign = 1;
            for (int j = 0; j < k; ++j) {
                if (cnt[sums[j]] == 0) {
                    continue;
                }
                --cnt[sums[j]];
                --cnt[sums[j] + d];
                sums1.push_back(sums[j]);
                sums2.push_back(sums[j] + d);
                if (sums2.back() == 0) {
                    sign = -1;
                }
            }
            ans[i - 1] = sign * d;
            for (int j = 0; j < k / 2; ++j) {
                sums[j] = sign == 1 ? sums1[j] : sums2[j];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func recoverArray(n int, sums []int) (ans []int) {
	sort.Ints(sums)
	for i := n; i > 0; i-- {
		k := 1 << i
		d := sums[k-1] - sums[k-2]
		cnt := map[int]int{}
		for _, s := range sums[:k] {
			cnt[s]++
		}
		sums1, sums2 := []int{}, []int{}
		sign := 1
		for _, s := range sums[:k] {
			if cnt[s] == 0 {
				continue
			}
			cnt[s]--
			cnt[s+d]--
			sums1 = append(sums1, s)
			sums2 = append(sums2, s+d)
			if s+d == 0 {
				sign = -1
			}
		}
		ans = append(ans, sign*d)
		if sign == -1 {
			sums1 = sums2
		}
		sums = sums1
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
