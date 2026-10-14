# Báo cáo kiểm thử hiệu năng website bằng Apache JMeter

**Sinh viên:** Nguyễn Thế Cường\
**Mã sinh viên:** 23010176\
**Môn học/lớp:** Đánh giá và kiểm thử chất lượng phần mềm\
**Ngày thực hiện:** 14/10/2026

## 1. Giới thiệu

Đề tài thực hiện kiểm thử hiệu năng cơ bản cho website mẫu bằng Apache
JMeter. Bài kiểm thử gửi các HTTP GET request đến `https://example.com`,
kiểm tra mã phản hồi HTTP và thu thập các chỉ số thời gian phản hồi.

Do máy tính tại trường không có quyền cài đặt phần mềm, Apache JMeter
được tải và chạy trên GitHub Actions. Cách làm này không yêu cầu cài
JMeter trực tiếp trên máy tính cá nhân.

## 2. Mục tiêu

-   Tìm hiểu Test Plan và Thread Group trong JMeter.
-   Mô phỏng nhiều người dùng gửi yêu cầu HTTP đến website.
-   Kiểm tra mã phản hồi HTTP bằng Response Assertion.
-   Thu thập và xem các chỉ số như tổng số request, thời gian phản hồi
    và tỷ lệ lỗi.
-   Lưu Test Plan, báo cáo và ảnh minh chứng trên GitHub.

## 3. Công cụ sử dụng

-   Apache JMeter 5.6.3.
-   Java 17 trên GitHub Actions runner.
-   GitHub repository và GitHub Actions để chạy kiểm thử, lưu kết quả.

## 4. Kịch bản kiểm thử

  Thuộc tính                   Giá trị
  ---------------------------- --------------------------------------------
  Website kiểm thử             `https://example.com`
  Số người dùng ảo (Threads)   5
  Ramp-up period               5 giây
  Loop count                   2
  Request 1                    `GET /`
  Request 2                    `GET /changes.html`
  Điều kiện kiểm tra           Mã phản hồi HTTP bằng 200
  Tổng số mẫu dự kiến          20 (5 người dùng × 2 request × 2 vòng lặp)

**Lưu ý:** 20 là số mẫu dự kiến nếu mọi request đều được thực hiện. Mục
7 ghi lại số liệu đo được từ lần chạy thực tế.

## 5. Cấu trúc repository

-   `test-plan/example-website-load-test.jmx`: kịch bản kiểm thử JMeter.
-   `.github/workflows/jmeter-test.yml`: cấu hình GitHub Actions tải
    JMeter và chạy bài kiểm thử.
-   `results/`: ghi chú hoặc kết quả kiểm thử được lưu trong repository
    (nếu có).
-   `screenshots/`: ảnh minh chứng chụp từ lần chạy thực tế.

## 6. Cách thực hiện

1.  Mở repository trên GitHub và chọn tab **Actions**.
2.  Chọn workflow **JMeter Performance Test**.
3.  Chạy workflow bằng **Run workflow** (nếu chưa tự chạy).
4.  Mở lần chạy mới nhất và kiểm tra trạng thái. Dấu tích xanh cho biết
    job hoàn thành thành công; vẫn cần kiểm tra kết quả JMeter và
    assertion.
5.  Tải artifact `jmeter-results` ở phần **Artifacts** của trang
    Summary.
6.  Giải nén artifact để xem file kết quả `.jtl` và báo cáo HTML nếu
    workflow đã tạo ra chúng.
7.  Chụp ảnh minh chứng thật và upload vào thư mục `screenshots/`.

## 7. Kết quả kiểm thử thực tế

Các chỉ số dưới đây được đọc từ HTML dashboard của lần chạy kiểm thử.

  Chỉ số                                          Kết quả thực tế
  ----------------------------------------- ---------------------
  Tổng số mẫu (Samples)                                        20
  Thời gian phản hồi trung bình (Average)                77,90 ms
  Thời gian phản hồi nhỏ nhất (Min)                         18 ms
  Thời gian phản hồi lớn nhất (Max)                        945 ms
  Throughput tổng                             4,93 giao dịch/giây
  Tỷ lệ lỗi tổng (Error %)                                 50,00%
  Số mẫu PASS                                                  10
  Số mẫu FAIL                                                  10

### 7.1. Kết quả theo từng request

  ---------------------------------------------------------------------------------------------------
  Request                  Samples    Failed      Error %   Average       Min       Max    Throughput
  ---------------------- --------- --------- ------------ --------- --------- --------- -------------
  `GET - Changes Page`          10        10      100,00%  22,50 ms     18 ms     27 ms     3,22 giao
                                                                                            dịch/giây

  `GET - Homepage`              10         0        0,00% 133,30 ms     20 ms    945 ms     2,48 giao
                                                                                            dịch/giây

  **Tổng**                  **20**    **10**   **50,00%**   **77,90 **18 ms**     **945   **4,93 giao
                                                               ms**                ms**   dịch/giây**
  ---------------------------------------------------------------------------------------------------

### 7.2. Nhận xét

-   Kịch bản ghi nhận tổng cộng **20 mẫu**, đúng với số lượng dự kiến: 5
    người dùng ảo × 2 request × 2 vòng lặp.
-   Có **10/20 mẫu thất bại**, tương ứng tỷ lệ lỗi tổng **50,00%**. Vì
    vậy, dù workflow trên GitHub Actions hoàn thành thành công, kết quả
    kiểm thử chưa thể xem là tất cả request đều đạt yêu cầu.
-   Request `GET - Homepage` có **10/10 mẫu thành công**, tỷ lệ lỗi
    **0,00%**. Thời gian phản hồi trung bình là **133,30 ms**, nhỏ nhất
    **20 ms** và lớn nhất **945 ms**.
-   Request `GET - Changes Page` có **10/10 mẫu thất bại**, tỷ lệ lỗi
    **100,00%**. Thời gian phản hồi trung bình là **22,50 ms**. Phản hồi
    nhanh không đồng nghĩa với request thành công; cần kiểm tra mã HTTP
    thực tế và thông tin lỗi trong file `.jtl` hoặc log JMeter để xác
    định nguyên nhân.
-   Thời gian phản hồi trung bình của toàn bộ mẫu là **77,90 ms**, nhưng
    chỉ số này cần được xem cùng tỷ lệ lỗi. Do một nửa số mẫu thất bại,
    không nên kết luận website đáp ứng tốt chỉ dựa vào thời gian phản
    hồi.
-   Đây là bài thử nghiệm nhỏ với 5 người dùng ảo trên website mẫu, nên
    kết quả không đại diện cho khả năng chịu tải của website sản xuất.

**Kết luận từ số liệu:** Bài kiểm thử đã chạy và thu thập được số liệu,
nhưng chưa đạt hoàn toàn do các request tới `GET - Changes Page` đều
thất bại. Cần xem chi tiết lỗi và xác minh endpoint trước khi chạy lại.
Báo cáo hiện tại không khẳng định nguyên nhân thất bại nếu chưa có bằng
chứng từ log.

## 8. Ảnh minh chứng

Upload ảnh thật vào thư mục `screenshots/` của repository. Tên file và
đường dẫn trong Markdown phải khớp chính xác thì ảnh mới hiển thị.

### 8.1. Kịch bản kiểm thử

Ảnh chụp phần cấu hình trong file `.jmx` trên GitHub, hoặc ảnh giao diện
JMeter nếu có thể mở Test Plan bằng JMeter.

![Kịch bản kiểm thử](screenshots/01-test-plan.png)

### 8.2. GitHub Actions chạy thành công

Ảnh chụp trang chi tiết workflow có dấu tích xanh và các bước
`Download JMeter`, `Run JMeter test`, `Upload results` hoàn thành.

![GitHub Actions chạy thành công](screenshots/05-github-actions.png)

### 8.3. Báo cáo tổng hợp

Ảnh chụp bảng số liệu thực tế trong HTML dashboard hoặc
Summary/Aggregate Report được tạo bởi JMeter.

![Báo cáo tổng hợp JMeter](screenshots/04-summary-report.png)

### 8.4. Báo cáo HTML

Ảnh chụp HTML dashboard hiển thị các chỉ số thực tế.

![Báo cáo HTML JMeter](screenshots/06-html-dashboard.png)

> Các đường dẫn ảnh ở trên là vị trí dự kiến. Ảnh chỉ hiển thị sau khi
> file ảnh thật được upload vào đúng thư mục `screenshots/` và được
> commit lên GitHub. Không tạo ảnh giả hoặc tự điền số liệu chưa đo.

## 9. Kết luận

Bài thực hành giúp làm quen với việc xây dựng kịch bản JMeter, mô phỏng
người dùng, kiểm tra mã phản hồi HTTP và thu thập số liệu hiệu năng.
Workflow GitHub Actions đã hoàn thành và báo cáo ghi nhận 20 mẫu với
thời gian phản hồi trung bình 77,90 ms. Tuy nhiên, tỷ lệ lỗi tổng là
50,00%, trong đó tất cả mẫu của `GET - Changes Page` thất bại. Vì vậy,
cần điều tra nguyên nhân và xác minh endpoint trước khi kết luận bài
kiểm thử đạt yêu cầu.

## 10. Tài liệu tham khảo

-   Apache JMeter: https://jmeter.apache.org/
-   Getting Started:
    https://jmeter.apache.org/usermanual/get-started.html
-   Building a Web Test Plan:
    https://jmeter.apache.org/usermanual/build-web-test-plan
