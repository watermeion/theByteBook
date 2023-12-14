# 追踪

单体系统时代追踪的范畴基本只局限于栈追踪（Stack Tracing），调试程序时，在 IDE 打个断点，看到的 Call Stack 视图上的内容便是追踪；编写代码时，处理异常调用了 Exception::printStackTrace()方法，它输出的堆栈信息也是追踪。微服务时代，追踪就不只局限于调用栈了，一个外部请求需要内部若干服务的联动响应，这时候完整的调用轨迹将跨越多个服务，同时包括服务间的网络传输信息与各个服务内部的调用堆栈信息，因此，分布式系统中的追踪在国内常被称为“全链路追踪”（后文就直接称“链路追踪”了），许多资料中也称它为“分布式追踪”（Distributed Tracing）。追踪的主要目的是排查故障，如分析调用链的哪一部分、哪个方法出现错误或阻塞，输入输出是否符合预期，等等。

追踪方面的情况与日志、度量有所不同，追踪是与具体网络协议、程序语言密切相关的，收集日志不必关心这段日志是由 Java 程序输出的还是由 Golang 程序输出的，对程序来说它们就只是一段非结构化文本而已，同理，度量对程序来说也只是一个个聚合的数据指标而已。但链路追踪就不一样，各个服务之间是使用 HTTP 还是 gRPC 来进行通信会直接影响追踪的实现，各个服务是使用 Java、Golang 还是 Node.js 来编写，也会直接影响到进程内调用栈的追踪方式。这决定了追踪工具本身有较强的侵入性，通常是以插件式的探针来实现；也决定了追踪领域很难出现一家独大的情况，通常要有多种产品来针对不同的语言和网络。近年来各种链路追踪产品层出不穷，市面上主流的工具既有像 Datadog 这样的一揽子商业方案，也有 AWS X-Ray 和 Google Stackdriver Trace 这样的云计算厂商产品，还有像 SkyWalking、Zipkin、Jaeger 这样来自开源社区的优秀产品。