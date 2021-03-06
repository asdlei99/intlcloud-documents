云函数是管理、运行的基本单元，通常由一系列配置与一系列可运行代码/软件包组成。

## 函数配置属性
在函数创建时，您需要提供以下信息：
- 函数名 FunctionName：必选，函数名作为函数的唯一标识名称，创建后不可修改。 
- 运行环境 Runtime：必选，指定函数的运行环境，创建后不可修改。
- 函数代码 Code：必选，函数运行的代码，创建后可修改。
- 执行方法 Handler：必选，函数处理方法名称，通常由“文件名.方法名”组成。具体运行环境的情况下稍有不同，可见各开发语言说明。
- 函数描述 Description：可选，可用于记录函数相关作用等信息。


除以上内容外，另外还可通过 [更新函数配置](https://cloud.tencent.com/document/product/583/19806#.E6.9B.B4.E6.96.B0.E5.87.BD.E6.95.B0.E9.85.8D.E7.BD.AE) 修改以下内容，配置更多函数运行时的信息：
- 内存大小 MemorySize：可选，指定函数运行时可用的内存大小，最小为128MB，并按128MB递增，默认128MB。
- 运行超时 Timeout：可选，指定函数的最长运行时间，可选值范围为1秒- 300秒，默认3秒。
- 环境变量 Environment：可选，在配置中定义的环境变量可在函数运行时从环境中获取到。
- 私有网络 VPCConfig：可选，配置私有网络后，可将函数置于配置的私有网络中运行。

## 函数可执行的操作
- [创建函数](https://intl.cloud.tencent.com/document/product/583/19806)：创建一个新的函数。
- [更新函数](https://intl.cloud.tencent.com/document/product/583/19808)：
 - 更新函数配置：更新函数的各项配置。
 - 更新函数代码：更新函数的运行代码。
- [获取详情](https://intl.cloud.tencent.com/document/product/583/19809)：获取函数配置、触发器及代码详情；
- [测试运行函数](https://intl.cloud.tencent.com/document/product/583/14572)：根据需要，通过同步或异步方法触发函数运行；
- [获取日志](https://intl.cloud.tencent.com/document/product/583/19810)：获取函数运行情况及输出的日志。
- [删除函数](https://intl.cloud.tencent.com/document/product/583/19807)：删除不再需要的函数。
- 复制函数：复制函数到指定的地域、指定的名称、指定的配置。

函数的触发器相关操作有：
- [创建触发器](https://cloud.tencent.com/document/product/583/30230)：创建一个新的触发器。
- [删除触发器](https://cloud.tencent.com/document/product/583/30231)：删除已有触发器。

