# Bài tập lớn
## PHẦN 1: Tìm hiểu MapReduce Và ApacheSpark

## 1. Map Reduce
MapReduce là một mô hình lập trình được thiết kế bởi Google xử lý tập dữ liệu lớn song song, thuật toán được phân tán trên 1 cụm. 
MapReduce gồm các thủ tục Map và  Reduce.
- Hàm Map : Các xử lý một cặp (key, value), lọc và phân loại dữ liệu. Dữ liệu này input vào hàm Reduce.
- Hàm Reduce : Tiếp nhận các (keyI, valueI) thực hiện tổng hợp dữ liệu.
Thư viện thủ tục Map() và Reduce() được viết bằng nhiều ngôn ngữ. Cài đặt miễn phí, phổ biến nhất là Apache Hadoop.
MapReduce có thể xử lý dễ dàng mọi bài toán có dữ liệu lớn bằng khả năng phân tích và tính toán phức tạp.
Mapreduce chạy song song được trên các máy có sự phân tán khác nhau. 
MapRedue thực hiện được trên nhiều nguồn ngôn ngữ lập trình khác nhau

### Hoạt động của MapReduce:

- Đọc dữ liệu đầu vào

- Xử lý dữ liệu đầu vào bằng cách thực thi hàm map() được cung cấp bởi người dùng

- Trộn các kết quả dữ liệu thu được từ các máy tính phân tán thích hợp nhất.

- Tổng hợp các kết quả trung gian thu được bằng hàm reduce()

- Đưa ra kết quả dữ liệu cuối.

*Một ví dụ luồng dữ liệu (dataflow) của nền tảng MapReduce:*
![](https://truongnguyenphilong.github.io/XuLyDuLieuLon/BAITAP1/pic1.jpg)

Nguồn : [hadoop mapreduce framework](https://www.edupristine.com/blog/hadoop-mapreduce-framework)


## 2. Apache Spark

Apache Spark là một framework mã nguồn mở tính toán cụm, được phát triển vào năm 2009 bởi AMPLab.
Spark cung cấp một giao diện để lập trình toàn bộ các cụm với tính song song dữ liệu ngầm, xử lý dữ liệu theo thời gian thực, vừa nhận dữ liệu từ các nguồn vừa thực hiện việc xử lý dữ liệu vừa nhận và khả năng chịu lỗi
 
![](https://truongnguyenphilong.github.io/XuLyDuLieuLon/BAITAP1/pic2.jpg)
 
Nguồn : [https://www.polarsparc.com/xhtml/Spark-1.html](https://www.polarsparc.com/xhtml/Spark-1.html)

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


# PHẦN 2: TÌM HIỂU SPARK PROPERTIES, RDD VÀ DATAFRAME

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
[https://spark.apache.org/docs/latest/configuration.html](https://spark.apache.org/docs/latest/configuration.html)

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

Nguồn : [https://intellipaat.com/blog/tutorial/spark-tutorial/programming-with-rdds/](https://intellipaat.com/blog/tutorial/spark-tutorial/programming-with-rdds/) 

Transformations: Một tập dữ liệu (dataset) mới được tạo từ một tập dữ liệu hiện có. Mỗi tập dữ liệu được chuyển qua một hàm. Kết quả giá trị trả về là nó sẽ gửi một RDD mới.

Actions: Trả về giá trị cho chương trình điều khiển sau khi thực thi mã trên tập dữ liệu, thực hiện các tính toán trên tập dữ liệu cần thiết. RDD trả về các giá trị không phải RDD. Các giá trị này được lưu trữ trên hệ thống bên ngoài.

Một số Transformation:

- distinct: loại bỏ trùng lắp trong RDD.
- map: Trả về một RDD mới bằng cách áp dụng hàm trên từng phần tử dữ liệu. Trong Python sử dụng lambda với từng phần tử để truyền vào map.
- filter: Trả về một RDD mới được hình thành bằng cách chọn các phần tử của nguồn mà hàm trả về true. 

Một số Action:

- count: Đếm số dòng, phần tử dữ liệu trong RDD.
- reduce: Tổng hợp các phần tử dữ liệu thành một RDD.
- first: lấy giá trị đầu tiên của RDD
- max: lấy giá trị lớn nhất của RDD
- min: lấy giá trị nhỏ nhất của RDD

Tham khảo các trang này để biết danh sách đầy đủ các Transformations và Actions của RDD.

[https://sparkbyexamples.com/apache-spark-rdd/spark-rdd-transformations/](https://sparkbyexamples.com/apache-spark-rdd/spark-rdd-transformations/)

[https://sparkbyexamples.com/apache-spark-rdd/spark-rdd-actions/](https://sparkbyexamples.com/apache-spark-rdd/spark-rdd-actions/)

Danh mục các loại RDD:

![](https://truongnguyenphilong.github.io/XuLyDuLieuLon/BAITAP2/Anh2.jpg)

Nguồn : [https://www.slideshare.net/cfregly/spark-streaming-40659876](https://www.slideshare.net/cfregly/spark-streaming-40659876)

### Ưu điểm của RDD

Các đặc tính, ưu điểm chính:

- Xử lý trong bộ nhớ: Dữ liệu bên trong RDD được lưu trữ trong bộ nhớ chứ không phải đĩa, do đó tăng tốc độ thực thi của Spark vì dữ liệu đang được tìm nạp từ dữ liệu trong bộ nhớ
- Tính bất biến: RDD được tạo ra không thể được sửa đổi. Bất kỳ biến đổi nào trên nó sẽ tạo ra một RDD mới.
- Khả năng chịu lỗi: Bất kỳ hoạt động RDD nào không thành công, nó sẽ có thể tính toán lại phân vùng bị mất của RDD từ bản gốc.
- Tiến hóa lười biếng: Dữ liệu bên trong RDD được giữ lại và chỉ đánh giá khi một hành động được kích hoạt.
- Phân vùng: Các RDD được chia thành các phần nhỏ hơn gọi là phân vùng mặc định, nó phân vùng theo số lượng lõi có sẵn.
- Tính bền bỉ: Việc có thể được tái sử dụng khiến chúng trở nên bền bỉ.
- Không có giới hạn:  Không có giới hạn về số lượng RDD có thể có bao nhiêu tùy thích chỉ phụ thuộc vào kích thước của bộ nhớ và ổ đĩa.

### Nhược điểm của RDD

- Spark RDD không phù hợp nhiều cho các ứng dụng thực hiện cập nhật cho kho lưu trữ trạng thái như hệ thống lưu trữ cho ứng dụng web.
- Một RDD chỉ có thể có trong một  SparkContext và RDD có thể có tên và mã định danh duy nhất (id).

## 3. Spark DataFrame

DataFrame là một tập hợp dữ liệu phân tán được tổ chức thành các cột được đặt tên. Về mặt khái niệm, nó tương đương với một bảng trong cơ sở dữ liệu quan hệ hoặc một khung dữ liệu trong R / Python, nhưng được tối ưu hóa phong phú hơn. DataFrames có thể được xây dựng từ nhiều nguồn như tệp dữ liệu có cấu trúc, bảng trong Hive, cơ sở dữ liệu bên ngoài hoặc RDD hiện có.
Tương tự như RDD, dữ liệu trong DataFrame cũng được quản lý theo kiểu phân tán và không thể thay đổi và được sắp sếp theo các cột. DataFrame được phát triển để có thể dễ dàng thực hiện các thao tác xử lý dữ liệu cũng như làm tăng đáng kể hiệu quả xử lý của hệ thống.
### Các tính năng và lợi ích của DataFrame

- DataFrames có cấu trúc dữ liệu có khả năng chịu lỗi và có tính sẵn sàng cao.
- Khả năng xử lý dữ liệu có kích thước từ Kilobyte (Kb) đến Petabyte (PB) trên một cụm node đơn đến cụm lớn.
- Xử lý dữ liệu có cấu trúc và bán cấu trúc (Processing Structured and Semi-Structured Data).
- Slicing và Dicing : DataFrames quản lý rõ ràng dữ liệu bị thiếu.
- Hỗ trợ cho nhiều định dạng và nguồn dữ liệu: Hỗ trợ các định dạng dữ liệu (Avro, csv, …) và hệ thống lưu trữ khác nhau (HDFS, bảng HIVE, mysql, ....).
- Tối ưu hóa hiện đại và tạo mã thông qua trình tối ưu hóa Spark SQL Catalyst  (tree transformation framework).
- Có thể dễ dàng tích hợp với tất cả các công cụ và framework xử lý dữ liệu lớn thông qua Spark-Core.
- Hỗ trợ nhiều ngôn ngữ
- Cung cấp API cho Python, Java, Scala và R.
- DataFrames là bất biến

### Tạo DataFrame

DataFrames trong Pyspark có thể được tạo theo nhiều cách. Dữ liệu có thể được tải thông qua tệp CSV, JSON, XML,.... Nó cũng có thể được tạo bằng RDD hiện có và thông qua các cơ sở dữ liệu khác như Hive, Cassandra,... . Nó cũng có thể lấy dữ liệu từ HDFS hoặc tệp cục bộ.
Cách đơn giản nhất để tạo DataFrame là từ bộ sưu tập seq. DataFrame cũng có thể được tạo từ RDD và bằng cách đọc các tệp từ một số nguồn. 

Ví dụ code python tạo DataFrame:
```
# Tạo dataframe từ bảng users trong bảng Hive.
users = context.table("users")

# từ các tệp JSON trong S3
logs = context.load("s3n://path/to/data.json", "json")

# Tạo một DataFrame chỉ chứa những người trẻ dưới 21 tuổi
young = users.filter(users.age < 21) 
```

# Phần 3: TÌM HIỂU MACHINE LEARNING VÀ XÂY DỰNG MACHINE LEARNING MODEL TRÊN PYSPARK 

## 1. Giới thiệu về Machine Learning

### Định nghĩa Machine Learning

Machine learning (học máy) là công cụ chuyển đổi thông tin thành tri thức, là một lĩnh vực con của Trí tuệ nhân tạo (Artificial Intelligence). Nó là một bước tiến so với các hệ thống rule-based (dựa trên quy tắc) bằng việc sử dụng các thuật toán cho phép máy tính  học từ dữ liệu để tìm ra các mẫu dữ liệu ẩn từ đó thực hiện các công việc thay vì được các kỹ sư lập trình theo truyền thống. 
Machine Learning và Big Data có một mối quan hệ tương quan với nhau. Các khối dữ liệu lớn là vô dụng trừ khi ta phân tích và tìm ra các quy luật ẩn bên trong nó. Do đó giá trị của Big Data phụ thuộc vào khả năng học dữ liệu của machine learning mà cũng nhờ đó mà Machine Learning phát triển vì sự gia tăng của Big Data. Từ đó nhu cầu về nhân lực ngành Machine Learning (Deep Learning) đang ngày một cao.

### Các ứng dụng đang sử dụng Machine Learning
- Xử lý ảnh (Image Processing): phân tích thông tin từ hình ảnh hay thực hiện một số phép biến đổi như; Gắn thẻ hình ảnh (Image Tagging) để tự động phát hiện khuôn mặt; Nhận dạng ký tự (Optical Character Recognition) để số hóa các dữ liệu trên giấy tờ, văn bản; hay các xe tự hành sử dụng machine learning để phát hiện đường đi, chướng ngại bằng cách xem xét từng khung hình video từ camera.
- Phân tích văn bản (Text analysis): trích xuất hoặc phân loại thông tin. Một số ví dụ phổ biến đó là Lọc spam(Spam filtering) dựa trên nội dung và tiêu đề văn bản, Phân tích ngữ nghĩa (Sentiment Analysis) để phân loại một câu từ,ý kiến và Khai thác thông tin (Information Extraction) để lấy các thông tin hữu ích từ văn bản.
- Video games và Robot: machine đóng vai trò rất quan trọng trong việc phát triển robot để robot có thể tự học, tự hành, giải quyết các công việc.
- Khai phá dữ liệu (Data mining): khám phá các thông tin có giá trị, đưa ra các dự đoán từ dữ liệu. Đây chính là điểm mấu chốt trong vấn đề về big data, cho phép kiếm thông tin hữu ích trong các dữ liệu lớn, phát hiện các quy luật(Association rules), bất thường(Anomaly detection) và dự đoán (Predictions).

## 2. Phân loại thuật toán machine learning
- Học có giám sát (supervised) : mục tiêu của mô hình là tìm ra luật để ánh xạ giữa đầu vào và đầu ra dựa trên các cặp (đầu vào, đầu ra) đã biết. Học có giám sát còn được chia nhỏ ra thành hai loại : Classification (Phân loại) và Regression (Hồi quy) mỗi loại có các thuật toán phổ biến như Linear Regression, Logistic Regression, Linear Classifier,…
- Học không giám sát (unsupervised) : mục tiêu của mô hình là tự tìm ra các mẫu dữ liệu ẩn. Trong học không giám sát ta không biết dữ liệu đầu ra hay nhãn của dữ liệu mà chỉ có dữ liệu đầu vào. Điển hình là bài toán phân cụm (clustering algorithm) với thuật toán phổ biến để giải quyết đó là K-means (phân cụm chỉ học từ tập dữ liệu đầu vào).
- Học nửa giám sát (semi-supervised): kết hợp các ví dụ có gắn nhãn và không gắn nhãn để sinh một hàm hoặc một bộ phân loại thích hợp.
- Học tăng cường (reinforcement learning): tự động xác định hành vi dựa trên hoàn cảnh để đạt được lợi ích cao nhất.
Học có giám sát và học không giám sát là thuật toán kinh điển và được sử dụng phổ biến nhất.

## 3. Quy trình xây dựng mô hình machine learning
1. Thu thập dữ liệu: Thu thập thông tin dữ liệu để mô hình học.
2. Chuẩn bị dữ liệu: Xử lý và đưa dữ liệu về định dạng tối ưu, chọn đặc trưng (Feature selection) hoặc Trích xuất đặc trưng (Feature extraction).
3. Huấn luyện mô hình:  Machine learning thực hiện việc học dữ liệu qua những gì đã có ở 2 bước trên. Một tập dữ liệu huấn luyện bao gồm nhiều mẫu. Mỗi mẫu sẽ là một thể hiện của bài toán. Machine learning sẽ học từ các thể hiện đó để tìm ra lời giải thích hợp.
4. Đánh giá: Sử dụng dữ liệu kiểm thử (testing data) kiểm thử mô hình để đánh giá xem chất lượng, độ chính xác của mô hình.
5. Tinh chỉnh: Tinh chỉnh mô hình để tối ưu hóa

Với Machine Learning trên PySpark thì ta cũng sẽ trải qua các bước như sau:
1.	Thu thập dữ liệu
2.	Xử lý tiền dữ liệu
3.	Xây dựng mô hình 
4.	Huấn luyện dữ liệu 
5.	Đánh giá mô hình

Link Source Code: [https://colab.research.google.com/drive/10YXuTPJWNSzwJEZS0lMTaUkk9A4gr7w_?usp=sharing]( https://colab.research.google.com/drive/10YXuTPJWNSzwJEZS0lMTaUkk9A4gr7w_?usp=sharing)

Mô hình được xây dựng ở source code trên là Logistic Regression Model:
Dataset: bank.csv
Input: age, job, marital, education, default, balance, housing, loan, contact, day, month, duration, campaign, pdays, previous, poutcome. Output: deposit
Kết quả sau khi thực hiện xong ta đánh giá model Logistic Regression, ta thấy có độ chính xác khá cao:

![](https://truongnguyenphilong.github.io/XuLyDuLieuLon/BAITAPLON/Picture1.png)

## TÀI LIỆU THAM KHẢO

1.  [https://kipalog.com/posts/Tong-quan-mo-hinh-lap-trinh-MapReduce](https://kipalog.com/posts/Tong-quan-mo-hinh-lap-trinh-MapReduce)
2.  [https://expressmagazine.net/posts/view/3673/ngay-7-gioi-thieu-big-data-mapreduce-la-gi](https://expressmagazine.net/posts/view/3673/ngay-7-gioi-thieu-big-data-mapreduce-la-gi)
3.  [https://viblo.asia/p/tim-hieu-ve-apache-spark-ByEZkQQW5Q0](https://viblo.asia/p/tim-hieu-ve-apache-spark-ByEZkQQW5Q0)
4.  [https://en.wikipedia.org/wiki/Apache_Spark](https://en.wikipedia.org/wiki/Apache_Spark)
5.  [https://www.polarsparc.com/xhtml/Spark-1.html](https://www.polarsparc.com/xhtml/Spark-1.html)
6.  [https://spark.apache.org/docs/latest/configuration.html](https://spark.apache.org/docs/latest/configuration.html)
7.  [https://spark.apache.org/docs/latest/rdd-programming-guide.html](https://spark.apache.org/docs/latest/rdd-programming-guide.html)
8.  [http://techalpine.com/what-is-apache-spark/?lang=vi](http://techalpine.com/what-is-apache-spark/?lang=vi)
9.  [https://sparkbyexamples.com/spark-rdd-tutorial/](https://sparkbyexamples.com/spark-rdd-tutorial/)
10. [https://sparkbyexamples.com/](https://sparkbyexamples.com/)
11. [https://laptrinh.vn/books/apache-spark/page/apache-spark-rdd](https://laptrinh.vn/books/apache-spark/page/apache-spark-rdd)
12. [https://helpex.vn/article/rdd-trong-spark-la-gi-va-tai-sao-chung-ta-can-no-5c6afe5bae03f628d053a84c](https://helpex.vn/article/rdd-trong-spark-la-gi-va-tai-sao-chung-ta-can-no-5c6afe5bae03f628d053a84c)
13. [https://www.educba.com/what-is-rdd/](https://www.educba.com/what-is-rdd/)
14. [https://intellipaat.com/blog/tutorial/spark-tutorial/programming-with-rdds/](https://intellipaat.com/blog/tutorial/spark-tutorial/programming-with-rdds/)
15. [https://www.quora.com/What-are-the-advantages-of-RDD](https://www.quora.com/What-are-the-advantages-of-RDD)
16. [https://www.tutorialspoint.com/spark_sql/spark_sql_dataframes.htm](https://www.tutorialspoint.com/spark_sql/spark_sql_dataframes.htm)
17. [https://databricks.com/blog/2015/02/17/introducing-dataframes-in-spark-for-large-scale-data-science.html](https://databricks.com/blog/2015/02/17/introducing-dataframes-in-spark-for-large-scale-data-science.html)
18. [https://khanh-personal.gitbook.io/ml-book-vn/machine-learning-la-gi](https://khanh-personal.gitbook.io/ml-book-vn/machine-learning-la-gi)
19. [https://vi.wikipedia.org/wiki/H%E1%BB%8Dc_m%C3%A1y](https://vi.wikipedia.org/wiki/H%E1%BB%8Dc_m%C3%A1y)
20. [https://machinelearningcoban.com/2016/12/27/categories/](https://machinelearningcoban.com/2016/12/27/categories/)
21. [https://nguyenvanhieu.vn/machine-learning-la-gi/](https://nguyenvanhieu.vn/machine-learning-la-gi/)
22. [https://trituenhantao.io/kien-thuc/machine-learning-va-cac-khai-niem-co-ban/](https://trituenhantao.io/kien-thuc/machine-learning-va-cac-khai-niem-co-ban/)
23. [https://topdev.vn/blog/big-data/](https://topdev.vn/blog/big-data/)
24. [https://helpex.vn/article/huong-dan-pyspark-dataframe-gioi-thieu-ve-dataframes-5c6b21e6ae03f628d053c29e](https://helpex.vn/article/huong-dan-pyspark-dataframe-gioi-thieu-ve-dataframes-5c6b21e6ae03f628d053c29e)
