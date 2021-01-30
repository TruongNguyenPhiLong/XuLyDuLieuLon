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

## 2. Spark RDD

RDD (Resilient Distributed Dataset) hay tập dữ liệu phân tán có khả năng phục hồi là một cấu trúc dữ liệu cơ bản của Spark và là trừu tượng hóa dữ liệu (data abstraction) chính trong Apache Spark và Spark Core. Nó là một tập hợp các đối tượng phân tán bất biến không thay đổi và được phân vùng, chỉ có thể được tạo bởi các hoạt động chi tiết thô như bản đồ (map), bộ lọc (filter), nhóm. Mỗi dataset trong RDD được chia thành các phân vùng logical có thể được tính toán trên các node khác nhau của cụm máy chủ. Chúng có thể hoạt động song song và có khả năng chịu lỗi.

Các đối tượng RDD có thể được tạo bằng Python, Java hoặc Scala. Nó cũng có thể bao gồm các lớp do người dùng định nghĩa. RDD cung cấp tính trừu tượng hóa dữ liệu cho việc phân vùng dữ liệu, phân phối dữ liệu được thiết kế để tính toán song song trên các node, khi thực hiện các phép biến đổi trên RDD sự song song luôn được đảm bảo do Spark cung cấp theo mặc định.

### Tạo RDD:

Có hai cách để tạo RDD:

- Song song hóa một tập hợp dữ liệu hiện có trong chương trình trình điều khiển Spark Context có sẵn : Các tập hợp song song được tạo bằng cách gọi phương thức song song hóa của lớp JavaSparkContext trong chương trình điều khiển. 
- Tham chiếu tập dữ liệu trong hệ thống lưu trữ bên ngoài có thể là HDFS, Hbase, các cơ sở dữ liệu quan hệ hoặc bất kỳ nguồn nào có định dạng tệp Hadoop.

Lưu ý: trước khi tạo RDD ta phải khởi tạo spark bằng code python sau:
```
import pyspark
from pyspark import SparkConf, SparkContext
import collections
conf= SparkConf().setMaster('local').setAppName('My spark app')
sc= SparkContext.getOrCreate(conf=conf)   
```
Ví dụ code python tạo RDD bằng cách song song hóa một tập dữ liệu:
```
data = [1, 10, 12, 8, 4]
rdd = sc.parallelize(data)  
```

Ở đây sparkContext.parallelize được sử dụng để song song hóa một tập hợp hiện có trong chương trình trình điều khiển. Đây là một phương pháp cơ bản để tạo RDD  nó yêu cầu tất cả dữ liệu phải có trên chương trình trình điều khiển trước khi tạo. Do đó với các ứng dụng sản xuất nên sử dụng cách tham chiếu tập dữ liệu trong hệ thống lưu trữ bên ngoài.

Ví dụ code python tạo RDD bằng cách tham chiếu tập dữ liệu trong hệ thống lưu trữ bên ngoài bằng phương thức sparkContext.textFile():
```
rdd = sc.textFile("/path/textFile.txt")  
```

Một số lưu ý khi đọc tệp với spark:
- Nếu sử dụng một đường dẫn trên local filesystem, tệp phải có thể truy cập được tại cùng một đường dẫn trên các node đang làm việc.
- Tất cả các phương thức tham chiếu tệp bao gồm textFile, hỗ trợ chạy trên thư mục, tệp nén và cả ký tự đại diện (wildcards).
- Phương thức textFile cũng có một đối số tùy chọn thứ hai để kiểm soát số lượng các phân vùng của tập tin chỉ cần lưu ý không thể có ít phân vùng (partitions) hơn khối (blocks).
Ngoài phương thức textFile thì API Python của Spark cũng hỗ trợ một số định dạng dữ liệu khác như với phương thức SparkContext.wholeTextFiles ta có thể đọc một thư mục chứa nhiều tệp văn bản nhỏ và trả về mỗi tệp dưới dạng cặp (tên tệp, nội dung).   Phương thức RDD.saveAsPickleFile và SparkContext.pickleFile lưu RDD ở một định dạng đơn giản bao gồm các đối tượng Python có sẵn.

### Hoạt động RDD:

RDD hỗ trợ hai loại hoạt động là Transformations và Actions. Ảnh minh họa 2 loại hoạt động cơ bản có thể sử dụng với RDD:

![](https://truongnguyenphilong.github.io/XuLyDuLieuLon/BAITAP2/Anh1.png)
