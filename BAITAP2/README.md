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
