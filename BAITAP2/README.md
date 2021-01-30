# Bài tập 2: TÌM HIỂU SPARK PROPERTIES, RDD VÀ DATAFRAME

## 1. Spark properties

Spark cung cấp ba lĩnh vực chính để cấu hình: Thuộc tính Spark (Spark properties), Các biến môi trường (Environment Variables) và Ghi nhật ký (Logging).

**_Spark properties_**:  kiểm soát hầu hết các cài đặt của ứng dụng (application) và được cấu hình riêng cho từng ứng dụng. Các thuộc tính này có thể được thiết lập trực tiếp trên SparkConf và chuyển đến SparkContext. SparkConf cho phép chúng ta cấu hình hầu hết các thuộc tính chung như URL và tên ứng dụng, chúng ta cũng có thể thiết lập các cặp giá trị khóa (key-value) qua phương thức set().

Ví dụ code Scala minh họa tạo một application với 1 luồng:
```
val conf = new SparkConf()
             .setMaster("local")
             .setAppName("My SPARK app ")
             .set("spark.executor.memory ", "1g ")
val sc = new SparkContext(conf)   
```
Code minh họa python khởi tạo Spark:
```
conf = SparkConf().setMaster("local").setAppName("My SPARK app")
sc = SparkContext(conf=conf)   
```

**_SparkConf()_**: Tạo một SparkConf tải các giá trị mặc định từ các system properties và classpath.

**_local_**: Chạy Spark cục bộ với một luồng duy nhất (tức là không có luồng song song nào cả). Có thể thêm giá trị sau chữ local để chạy nhiều luồng song song ví dụ: local [n] để chạy Spark cục bộ với số luồng là n, local[*] để chạy spark cục bộ với số luồng tương ứng với số core trên máy Java virtual machine.

**_set(“spark.executor.memory”, “1g”)_**: cài đặt số lượng bộ nhớ được sử dụng cho mỗi quá trình thực thi là 1g.

Hầu hết các thuộc tính kiểm soát cài đặt nội bộ đều có giá trị mặc định sẵn. Một số thuộc tính phổ biến:

- spark.app.name: Tên ứng dụng, mặc định là chưa có, xuất hiện trong giao diện người dùng và dữ liệu nhật ký (log data).
- spark.executor.memory: Số lượng bộ nhớ được sử dụng cho mỗi quá trình thực thi, mặc định là 1g.
- spark.master: URL chính để kết nối tới, mặc định là chưa có.
- spark.serializer: Class được sử dụng để tuần tự hóa các đối tượng sẽ được gửi qua mạng hoặc cần được lưu vào bộ nhớ đệm ở dạng tuần tự hóa. Mặc định là org.apache.spark.serializer.JavaSerializer nhưng thực tế nên sử dụng class org.apache.spark.serializer.KryoSerializer để có được tốc độ tốt hơn class mặc định của tuần tự hóa. 
- spark.kryo.registrator: Class được sử dụng để đăng ký các lớp tùy chỉnh nếu chúng ta sử dụng tuần tự hóa Kyro, mặc định là chưa được cài đặt.
- spark.local.dir: Đường dẫn thư mục các cho không gian đầu trong spark để lưu trữ các tệp đầu và RDDs vào ổ đĩa, đây nên là ổ đĩa nhanh hoặc local disk, mặc định đường dẫn là: /tmp.
- spark.cores.max: Được sử dụng ở chế độ độc lập để chỉ định số lượng lõi CPU tối đa để yêu cầu cho ứng dụng từ toàn bộ cụm. Nếu không được đặt, mặc định sẽ nằm spark.deploy.defaultCorestrên trình quản lý cụm độc lập của Spark hoặc vô hạn (tất cả các lõi có sẵn) trên Mesos.

Với các Available Properties (thuộc tính có sẵn) ta có thể sử dụng để cài đặt cho các ứng dụng, môi trường thực thi, giao diện người dùng, nén và tuần tự hóa, bảo mật, quản lý bộ nhớ, Spark Streaming, SparkR, GraphX, Cluster Managers(Yarn, Mesos),…

Có thể tìm thêm nhiều spark available properties có sẵn tại trang web:

https://spark.apache.org/docs/latest/configuration.html
