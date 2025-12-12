---
title: "CloudWatch Logs Insights"

weight: 2
chapter: false
pre: " <b> 4.2 </b> "
---

#### CloudWatch Logs Insights

Trong phần này thì chúng ta sẽ thực hiện tạo logs từ một ứng dụng, sau đó là sẽ query những logs này ở trong CloudWatch Logs Insights. Mình sẽ chọn một instance nào đó để làm mẫu.

1. Trên thanh tìm kiếm dịch vụ.

   - Nhập `EC2`.
   - Chọn **EC2**.

![4.2.1](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.1.png)

2. Vào trong trang Instances của **EC2 Console**

   - Chọn một Instance bất kì, ở đây mình chọn Instance-A.
   - Ấn chọn **Connect**.

![4.2.2](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.2.png)

3. Trong trang **Connect to instance**.

   - Chọn tab **Session Manager**.
   - Ấn **Connect**.

![4.2.3](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.3.png)

4. Chờ một ít phút thì một Terminal hiện lên.

   - Đầu tiên là vào trong **tmp**.
   - Sau đó là tải file py script về.

```bash
cd /tmp
sudo aws s3 cp s3://<workshop-template-bucket>/logger.py .
```
   - Trong đó <workshop-template-bucket> là tên bucket bạn đã tạo ở phần 2

![4.2.4](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.4.png)

5. Cấp quyền thực thi và thực thi file script này.

```bash
sudo chmod +x logger.py
python3 logger.py &
```

![4.2.5](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.5.png)

6. Kiểm tra các logger đang chạy dưới dạng process

```bash
ps -aux | grep logger
```

Có thể thấy thì hiện tại có 2 processes đang chạy, nó sẽ chạy cho tới cuối bài.

![4.2.6](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.6.png)

7. Dùng lệnh để in ra các dòng log từ file `/var/log/messages`, nó sẽ in cho tới khi nào mình huỷ.

```bash
sudo tail -f /var/log/messages
```

![4.2.7](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.7.png)

8. Trở lại **CloudWatch Console**, vào trong **Logs Insights** ở thanh menu bên trái. Chúng ta sẽ thực hiện query logs ở trong này.

![4.2.8](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.8.png)

9. Trong **Selection criteria**, tìm `/ec2` và chọn **/ec2/linux/var/log/messages**.

![4.2.9](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.9.png)

10. Nhập vào dòng query như bên dưới và ấn **Run query**.

```
fields @timestamp, @message
| sort @timestamp desc
| limit 20
```

Và được kết quả.

![4.2.10](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.10.png)

Đây chính là log mà chúng ta mới tạo ra ở bước trên.

![4.2.11](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.11.png)

11. Giờ thì thử query là các ERROR Logs.

```
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20
```

![4.2.12](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.12.png)

Cũng là những log lỗi mà hồi nãy chúng ta đã tạo ra.

![4.2.13](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.13.png)

12. Tiếp theo là các WARN Logs.

```
fields @timestamp, @message
| filter @message like /WARN/
| sort @timestamp desc
| limit 20
```

Đây là các logs mới được tạo ra.

![4.2.14](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.14.png)

13. Giờ thử query lại log, lỗi, chúng ta cũng sẽ thấy được các log mới được tạo ra.

![4.2.15](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.15.png)

14. Ngoài ra thì chúng ta còn có thể thực hiện được việc query theo từ khoá khác. Như câu query ở bên dưới.

```
fields @timestamp, @message
| filter @message like /eth0/
| sort @timestamp desc
| stats count (*) by bin()
```

![4.2.16](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.16.png)

![4.2.17](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.17.png)

#### Trực quan hoá query log

Ngoài ra thì chúng ta còn có thể xem được biểu đồ của các query này nữa. Chuyển sang tab **Visualization**.

![4.2.18](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.18.png)

#### Lưu lệnh truy vấn

Về sau thì có thể chúng ta sẽ có nhiều câu truy vấn cần sử dụng lại hoặc là có những câu truy vấn phức tạp hơn mà chúng ta cần phải lưu lại. Thì Logs Insights có hỗ trợ cho chúng ta lưu lại các câu truy vấn này.

1. Ví dụ mình sẽ lưu lại câu truy vấn lỗi.

   - Trở lại màn hình chính của Logs Insights.
   - Dán lại lệnh query tìm Error logs.
   - Ấn chọn **Save**.

![4.2.19](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.19.png)

2. Trong trang Save a new query, điền các thông tin như

   - Query name: `Erors`.
   - Folder: `cloudwatch-workshop`, và tích chọn **Create new**.
   - Kiểm tra lại thông tin trong phần **Query definition details**.
   - Ấn **Save**.

![4.2.20](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.20.png)

![4.2.21](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.21.png)

#### Lịch sử truy vấn

Logs Insights còn cho phép chúng ta có thể xem lại được lịch sử đã truy vấn. Trên giao diện, ấn chọn **History** (dưới Query editor).

![4.2.22](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.22.png)

![4.2.23](/images/4-cloud-watch-logs/4.2-logs-insights/4.2.23.png)

Trong phần tiếp theo, chúng ta sẽ tiếp tục dùng hành vi này của các logger để tạo ra một Metrics Filter, để chuyển các Log thành Metrics. Sau đó sẽ thao tác nó với Alarm.
