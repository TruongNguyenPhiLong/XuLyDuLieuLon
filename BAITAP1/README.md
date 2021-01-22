## Bài tập tuần 1


### Tìm hiểu MapReduce Và ApacheSpark
1. Map Reduce
MapReduce là một mô hình lập trình được thiết kế bởi Google xử lý tập dữ liệu lớn song song, thuật toán được phân tán trên 1 cụm. MapReduce gồm các thủ tục Map và  Reduce.
Hàm Map : Các xử lý một cặp (key, value), lọc và phân loại dữ liệu. Dữ liệu này input vào hàm Reduce.
Hàm Reduce : Tiếp nhận các (keyI, valueI) thực hiện tổng hợp dữ liệu.
Thư viện thủ tục Map() và Reduce() được viết bằng nhiều ngôn ngữ. Cài đặt miễn phí, phổ biến nhất là Apache Hadoop.

### Hoạt động của MapReduce:

- Đọc dữ liệu đầu vào

- Xử lý dữ liệu đầu vào bằng cách thực thi hàm map() được cung cấp bởi người dùng

- Trộn các kết quả dữ liệu thu được từ các máy tính phân tán thích hợp nhất.

- Tổng hợp các kết quả trung gian thu được bằng hàm reduce()

- Đưa ra kết quả dữ liệu cuối.

*Một ví dụ luồng dữ liệu (dataflow) của nền tảng MapReduce:*
![](https://truongnguyenphilong.github.io/XuLyDuLieuLon/BAITAP1/pic1.jpg)

Nguồn : [hadoop mapreduce framework](https://www.edupristine.com/blog/hadoop-mapreduce-framework)


2. Apache Spark

Apache Spark là một framework mã nguồn mở tính toán cụm, được phát triển vào năm 2009 bởi AMPLab.
Spark cung cấp một giao diện để lập trình toàn bộ các cụm với tính song song dữ liệu ngầm, xử lý dữ liệu theo thời gian thực, vừa nhận dữ liệu từ các nguồn vừa thực hiện việc xử lý dữ liệu vừa nhận và khả năng chịu lỗi
 
(Nguồn https://www.polarsparc.com/xhtml/Spark-1.html)

Thành phần Spark gồm:

- Lớp dữ liệu (Hadoop HDFS).

- Trình quản lý cụm để quản lý các nút trong cụm (Trình quản lý cụm độc lập, Apache Mesos, Hadoop YARN hoặc Kubernetes).

- Công cụ điện toán cụm Spark Core chịu trách nhiệm lập lịch làm việc, quản lý bộ nhớ, quản lý lỗi và quản lý lưu trữ.

- Spark SQL cung cấp giao diện SQL để truy cập dữ liệu được phân phối trên các nút trong cụm.

- Spark Streaming cho phép xử lý luồng dữ liệu trực tiếp trong thời gian thực.

- Spark GraphX là một thư viện để quản lý đồ thị của các đối tượng dữ liệu.

- Spark MLlib là một thư viện dành cho các thuật toán học máy phổ biến.

Apache Spark là một công cụ tính toán cụm phân tích thống nhất, hiệu suất cao, mã nguồn mở, mục đích chung để xử lý dữ liệu phân tán trên quy mô lớn trên một cụm máy tính hàng hóa (còn được gọi là nút).

Ngăn xếp Spark cung cấp hỗ trợ xử lý hàng loạt, truy vấn tương tác bằng cách sử dụng sql, phát trực tuyến, học máy và xử lý đồ thị.

Spark có thể chạy trên nhiều công cụ điện toán cụm khác nhau như trình quản lý cụm độc lập tích hợp sẵn, Hadoop YARN, Apache Mesos, Kubernetes hoặc trong môi trường đám mây như AWS, Azure hoặc Google Cloud.

### Tài liệu kham thảo

1. [https://kipalog.com/posts/Tong-quan-mo-hinh-lap-trinh-MapReduce](https://kipalog.com/posts/Tong-quan-mo-hinh-lap-trinh-MapReduce)
2. [https://expressmagazine.net/posts/view/3673/ngay-7-gioi-thieu-big-data-mapreduce-la-gi](https://expressmagazine.net/posts/view/3673/ngay-7-gioi-thieu-big-data-mapreduce-la-gi)
3. [https://viblo.asia/p/tim-hieu-ve-apache-spark-ByEZkQQW5Q0](https://viblo.asia/p/tim-hieu-ve-apache-spark-ByEZkQQW5Q0)
4. [https://en.wikipedia.org/wiki/Apache_Spark](https://en.wikipedia.org/wiki/Apache_Spark)
5. [https://www.polarsparc.com/xhtml/Spark-1.html](https://www.polarsparc.com/xhtml/Spark-1.html)


