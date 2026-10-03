---
comments: true
difficulty: Hard
tags:
    - Design
    - Hash Table
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2254. Design Video Sharing Platform 🔒](https://leetcode.com/problems/design-video-sharing-platform)

[中文文档](/solution/2200-2299/2254.Design%20Video%20Sharing%20Platform/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một nền tảng chia sẻ video, nơi người dùng có thể tải lên và xóa video. Mỗi <code>video</code> là một <strong>string</strong> chỉ gồm các chữ số, trong đó chữ số thứ <code>i<sup>th</sup></code> của chuỗi biểu diễn nội dung video tại phút <code>i</code>. Ví dụ, chữ số đầu tiên biểu diễn nội dung tại phút <code>0</code> của video, chữ số thứ hai biểu diễn nội dung tại phút <code>1</code> của video, v.v. Người xem cũng có thể thích hoặc không thích video. Bên trong, nền tảng theo dõi <strong>số lượt xem, lượt thích và lượt không thích</strong> của mỗi video.</p>

<p>Khi một video được tải lên, video được gán với <code>videoId</code> là số nguyên nhỏ nhất còn trống, bắt đầu từ <code>0</code>. Sau khi một video bị xóa, <code>videoId</code> gắn với video đó có thể được tái sử dụng cho video khác.</p>

<p>Hãy triển khai lớp <code>VideoSharingPlatform</code>:</p>

<ul>
	<li><code>VideoSharingPlatform()</code> Khởi tạo đối tượng.</li>
	<li><code>int upload(String video)</code> Người dùng tải lên một <code>video</code>. Trả về <code>videoId</code> được gán cho video.</li>
	<li><code>void remove(int videoId)</code> Nếu có video gắn với <code>videoId</code>, xóa video đó.</li>
	<li><code>String watch(int videoId, int startMinute, int endMinute)</code> Nếu có video gắn với <code>videoId</code>, tăng số lượt xem video lên <code>1</code> và trả về chuỗi con của chuỗi video bắt đầu tại <code>startMinute</code> và kết thúc tại <code>min(endMinute, video.length - 1</code><code>)</code> (<strong>bao gồm cả hai đầu mút</strong>). Nếu không, trả về <code>&quot;-1&quot;</code>.</li>
	<li><code>void like(int videoId)</code> Tăng số lượt thích của video gắn với <code>videoId</code> lên <code>1</code> nếu có video gắn với <code>videoId</code>.</li>
	<li><code>void dislike(int videoId)</code> Tăng số lượt không thích của video gắn với <code>videoId</code> lên <code>1</code> nếu có video gắn với <code>videoId</code>.</li>
	<li><code>int[] getLikesAndDislikes(int videoId)</code> Trả về một mảng số nguyên <code>values</code> có độ dài <code>2</code> với chỉ số <strong>0-indexed</strong>, trong đó <code>values[0]</code> là số lượt thích và <code>values[1]</code> là số lượt không thích của video gắn với <code>videoId</code>. Nếu không có video gắn với <code>videoId</code>, trả về <code>[-1]</code>.</li>
	<li><code>int getViews(int videoId)</code> Trả về số lượt xem của video gắn với <code>videoId</code>; nếu không có video gắn với <code>videoId</code>, trả về <code>-1</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;VideoSharingPlatform&quot;, &quot;upload&quot;, &quot;upload&quot;, &quot;remove&quot;, &quot;remove&quot;, &quot;upload&quot;, &quot;watch&quot;, &quot;watch&quot;, &quot;like&quot;, &quot;dislike&quot;, &quot;dislike&quot;, &quot;getLikesAndDislikes&quot;, &quot;getViews&quot;]
[[], [&quot;123&quot;], [&quot;456&quot;], [4], [0], [&quot;789&quot;], [1, 0, 5], [1, 0, 1], [1], [1], [1], [1], [1]]
<strong>Đầu ra</strong>
[null, 0, 1, null, null, 0, &quot;456&quot;, &quot;45&quot;, null, null, null, [1, 2], 2]

<strong>Giải thích</strong>
VideoSharingPlatform videoSharingPlatform = new VideoSharingPlatform();
videoSharingPlatform.upload(&quot;123&quot;);          // The smallest available videoId is 0, so return 0.
videoSharingPlatform.upload(&quot;456&quot;);          // The smallest available <code>videoId</code> is 1, so return 1.
videoSharingPlatform.remove(4);              // There is no video associated with videoId 4, so do nothing.
videoSharingPlatform.remove(0);              // Remove the video associated with videoId 0.
videoSharingPlatform.upload(&quot;789&quot;);          // Since the video associated with videoId 0 was deleted,
                                             // 0 is the smallest available <code>videoId</code>, so return 0.
videoSharingPlatform.watch(1, 0, 5);         // The video associated with videoId 1 is &quot;456&quot;.
                                             // The video from minute 0 to min(5, 3 - 1) = 2 is &quot;456&quot;, so return &quot;456&quot;.
videoSharingPlatform.watch(1, 0, 1);         // The video associated with videoId 1 is &quot;456&quot;.
                                             // The video from minute 0 to min(1, 3 - 1) = 1 is &quot;45&quot;, so return &quot;45&quot;.
videoSharingPlatform.like(1);                // Increase the number of likes on the video associated with videoId 1.
videoSharingPlatform.dislike(1);             // Increase the number of dislikes on the video associated with videoId 1.
videoSharingPlatform.dislike(1);             // Increase the number of dislikes on the video associated with videoId 1.
videoSharingPlatform.getLikesAndDislikes(1); // There is 1 like and 2 dislikes on the video associated with videoId 1, so return [1, 2].
videoSharingPlatform.getViews(1);            // The video associated with videoId 1 has 2 views, so return 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;VideoSharingPlatform&quot;, &quot;remove&quot;, &quot;watch&quot;, &quot;like&quot;, &quot;dislike&quot;, &quot;getLikesAndDislikes&quot;, &quot;getViews&quot;]
[[], [0], [0, 0, 1], [0], [0], [0], [0]]
<strong>Đầu ra</strong>
[null, null, &quot;-1&quot;, null, null, [-1], -1]

<strong>Giải thích</strong>
VideoSharingPlatform videoSharingPlatform = new VideoSharingPlatform();
videoSharingPlatform.remove(0);              // There is no video associated with videoId 0, so do nothing.
videoSharingPlatform.watch(0, 0, 1);         // There is no video associated with videoId 0, so return &quot;-1&quot;.
videoSharingPlatform.like(0);                // There is no video associated with videoId 0, so do nothing.
videoSharingPlatform.dislike(0);             // There is no video associated with videoId 0, so do nothing.
videoSharingPlatform.getLikesAndDislikes(0); // There is no video associated with videoId 0, so return [-1].
videoSharingPlatform.getViews(0);            // There is no video associated with videoId 0, so return -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= video.length &lt;= 10<sup>5</sup></code></li>
	<li>Tổng <code>video.length</code> trong tất cả các lần gọi <code>upload</code> không vượt quá <code>10<sup>5</sup></code></li>
	<li><code>video</code> chỉ gồm các chữ số.</li>
	<li><code>0 &lt;= videoId &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= startMinute &lt; endMinute &lt; 10<sup>5</sup></code></li>
	<li><code>startMinute &lt; video.length</code></li>
	<li>Tổng <code>endMinute - startMinute</code> trong tất cả các lần gọi <code>watch</code> không vượt quá <code>10<sup>5</sup></code>.</li>
	<li>Tổng cộng <strong>tất cả</strong> các hàm được gọi nhiều nhất <code>10<sup>5</sup></code> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Nền tảng phải cấp $\textit{videoId}$ nhỏ nhất còn trống và tái sử dụng các id sau khi xóa; các thao tác watch, like và truy cập bộ đếm đều dựa trực tiếp trên id. Với $10^5$ lần gọi, việc quét tuyến tính để tìm id trống sẽ quá chậm.
>
> Ta dùng một min-heap để lưu các id đã được giải phóng, cùng với một bộ đếm id mới tiếp theo. Nội dung và số liệu thống kê được lưu trong các hash map; khi xóa, ta đưa id trở lại heap. watch trả về lát cắt $[\textit{start},\min(\textit{end},|v|-1)]$. Các tab không có phần cài đặt; một heap cùng các map là đủ để đáp ứng giới hạn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python

```

#### Java

```java

```

#### C++

```cpp

```

#### Go

```go

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
