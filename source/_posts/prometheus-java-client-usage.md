---
title: prometheus-java-client-usage
date: 2024-04-12 17:55:33
tags: ["prometheus", "java client"]
---

# 1. prometheus 介绍

Prometheus 是一种开源的系统监控和警报工具，最初由 SoundCloud 开发，现在由 Cloud Native Computing Foundation (CNCF) 维护。它专门设计用于在动态环境中监控大规模的分布式系统。

以下是 Prometheus 的一些主要特点和功能：

1. 多维数据模型：Prometheus 使用多维数据模型来存储时间序列数据，每个时间序列由指标名称和一组标签组成，使得数据可以灵活地被查询和聚合。
1. 灵活的查询语言：PromQL 是 Prometheus 的查询语言，它允许用户对时间序列数据进行高级查询、聚合和操作，以生成自定义的监控指标和警报条件。
1. 持久化存储：Prometheus 使用本地存储引擎来持久化时间序列数据，可以配置多种存储后端，包括本地磁盘、远程对象存储等。




# 2. prometheus 如何接入及示例代码


## maven依赖
```xml
<dependency>
    <groupId>io.prometheus</groupId>
    <artifactId>simpleclient</artifactId>
    <version>0.11.0</version>
</dependency>
<dependency>
    <groupId>io.prometheus</groupId>
    <artifactId>simpleclient_httpserver</artifactId>
    <version>0.11.0</version>
</dependency>

```




## 指标项 
1. 计数器（Counter）：
    - 用途：计数器用于累积值，通常用于表示累积事件的总数，如请求数、任务完成数等。计数器的值只能增加，不能减少或重置。
    - 示例：表示请求数的总数。
2. 计量器（Gauge）：
    - 用途：计量器用于表示可变值，它可以增加、减少或重置。通常用于表示动态变化的数据，如内存使用量、并发连接数等。
    - 示例：表示当前活跃用户数。内存占用数等
3. 直方图（Histogram）：
    - 用途：直方图用于度量观察值的分布情况，并提供一组用于汇总这些观察值的统计信息，如总数、均值、分位数等。
    - 示例：表示请求延迟的分布情况。
1. 摘要（Summary）：

   - 用途：摘要与直方图类似，也用于度量观察值的分布情况。不同之处在于，摘要提供一组用于汇总这些观察值的统计信息，如总数、平均值、标准差等。
   - 示例：表示响应时间的摘要信息。p99耗时统计等

### 指标项Counter
java代码示例
```java
import io.prometheus.client.CollectorRegistry;
import io.prometheus.client.Counter;
import io.prometheus.client.exporter.HTTPServer;

import java.io.IOException;

public class Main {

    private static final Counter requestsTotal = Counter.build()
            .name("myapp_requests_total")
            .help("Total number of requests.")
            .labelNames("name")
            .register();

    public static void main(String[] args) throws IOException {
        // Expose Prometheus metrics at http://localhost:9090/metrics
        HTTPServer server = new HTTPServer(9090);

        // Simulate a request and increment the counter
        simulateRequest();

        // Wait for server to stop (not needed if you have other shutdown mechanisms)
        server.stop();
    }

    private static void simulateRequest() {
        // Increment the requestsTotal counter
        requestsTotal.labels("simulate").inc();
    }
}
```

这代码中count就是一直增加，在未来图表中，可以用当前时间的数字减去1分钟前的数字，就能得到当前1分钟内请求数。

另外需要注意的是`labelNames()`函数和`labels()`函数。这两个是配合使用的。这里可以增加任意的维度。未来搜索或者作图可以更好地展现。

例如，我希望统计不同路径的访问请求。
那么我在`labels('不同的path')`, 这样就能分路径进行统计了。

如果你想统计不同的路径，不同的客户端请求数量。那么在注册Counter时，需要这样调用
```java
    private static final Counter requestsTotal = Counter.build()
            .name("myapp_requests_total")
            .help("Total number of requests.")
            .labelNames("path", "clientType")
            .register();

    ....
    // 然后在需要增加计数时这样处理.
    requestsTotal.labels("请求的URI", "iOS").inc();
    requestsTotal.labels("请求的URI", "Android").inc();
```


### 指标项Gauge
```java
import io.prometheus.client.exporter.HTTPServer;
import io.prometheus.client.Gauge;
import java.io.IOException;

public class MyApp {
    static final Gauge myGauge = Gauge.build()
        .name("my_gauge")
        .help("Example of a Gauge metric")
        .labelNames("label1", "label2") // 这里label非常重要。注意一定是有限集（可枚举，数量要有限制）
        .register();

    public static void main(String[] args) throws IOException {
        // 启动 Prometheus HTTP 服务器，暴露指标数据
        HTTPServer server = new HTTPServer(8080);

        // 模拟应用程序中的指标更新
        updateMetrics();
    }

    // 模拟更新指标数据
    static void updateMetrics() {
        // 假设这是您应用程序的一部分，定期更新 Gauge 指标的值
        while (true) {
            // 更新 Gauge 指标的值，可以基于应用程序的状态或性能指标进行设置
            myGauge.labels("value1", "value2").set(42);

            // 在这里进行其他业务逻辑和操作

            // 休眠一段时间，模拟指标数据的定期更新
            try {
                Thread.sleep(5000); // 5秒钟
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```
### 指标项Histogram
```java
import io.prometheus.client.exporter.HTTPServer;
import io.prometheus.client.Histogram;
import java.io.IOException;

public class MyApp {
    static final Histogram requestDuration = Histogram.build()
        .name("http_request_duration_seconds")
        .help("HTTP request duration in seconds")
        .buckets(0.1, 0.5, 1, 2, 5) // 定义 Histogram 的 buckets
        .register();

    public static void main(String[] args) throws IOException {
        // 启动 Prometheus HTTP 服务器，暴露指标数据
        HTTPServer server = new HTTPServer(8080);

        // 模拟应用程序中的请求处理
        handleRequest();
    }

    // 模拟处理 HTTP 请求，并记录响应时间
    static void handleRequest() {
        // 假设这是您应用程序的一部分，处理 HTTP 请求并记录响应时间
        while (true) {
            long startTime = System.currentTimeMillis();

            // 处理 HTTP 请求，执行一些业务逻辑

            // 响应时间为处理开始到结束之间的时间差
            long duration = System.currentTimeMillis() - startTime;

            // 记录响应时间到 Histogram 中
            requestDuration.observe(duration / 1000.0); // 将毫秒转换为秒并记录

            // 在这里进行其他业务逻辑和操作
        }
    }
}

```
这里的buckets定义了不同桶
`[0, 0.1), [0.1, 0.5), [0.5, 1), [1, 2), [2, 5) [5, 正无穷)`
当耗时0.3秒被记录时，Prometheus将增加`[0, 0.1), [0.1, 0.5)`两个桶的值。

### 指标项Summary
```java
import io.prometheus.client.Summary;
import io.prometheus.client.CollectorRegistry;

// 创建一个 Summary 指标
Summary requestLatency = Summary.build()
    .name("http_request_duration_seconds")
    .help("HTTP request latency in seconds.")
    .quantile(0.5, 0.05) // 统计50%分位数，最大允许误差为 ±5%
    .quantile(0.9, 0.01)
    .quantile(0.99, 0.001)
    .register();

// 在请求处理过程中记录持续时间
Summary.Timer requestTimer = requestLatency.startTimer();
// 执行请求处理逻辑
// 记录请求处理结束时间
requestTimer.observeDuration();

// 也可使用
requestLatency.observe(cost_in_seconds)

```


# 3. prometheus 检查是否成功
这其实要看设置的监听端口是多少。例如
```java
import io.prometheus.client.exporter.HTTPServer;
...
HTTPServer server = new HTTPServer(8080);
```
那么上述代码的启动后，且有指标生成后，访问`http://localhost:8080/metrics`就能看到相应的指标。



# 4. 使用注意事项
1. `labels("value1", "value2")`中的`value1`和`value2`需要可数，不一定是枚举类型，但是数量是有限的。
2. `buckets(0.1, 0.5, 1, 2, 5)`, 这里的值越多，统计性能就越低，占用的内存就越大
3. Histogram用于统计分布，Summary用来统计多少分位的数据。
4. Counter每次放入的是inc()或者inc(number)，放入的是增量。 Gauge每次放入的是当前的数据。