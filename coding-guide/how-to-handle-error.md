# 错误处理指南

错误处理是最容易忽略但是对业务系统至关重要的功能。

## 基本原则

- 对用户展示有用的错误信息。最好有准确的错误原因和建议的措施。
- 错误信息分成二类：用户可以采取措施的信息和程序员可用的调试信息。后者是系统管理员和开发人员用的，可以隐藏。

## 错误代码

错误代码和错误信息用在二个地方：用户交互界面和服务之间的 API。这二个地方都适用上面的基本原则。API 的调用者也是用户。有个通用原则是要在界面和对外（跨进程）的 API Catch 所有的错误，返回一个错误代码和尽可能准确、简短的错误信息。给用户返回未经处理的错误不但不友好，还可能会泄露数据。

在 API 调用之间，有很多层次。比如简单的二层（多层类似）：应用层调用网络层的服务发送请求和接受数据。每层有自己的错误处理。错误代码也要各自独立。

一套 API 牵涉到多个业务模块。相应的错误代码也可分为二类：一类是标准化的错误代码，另一类是业务模块自己独立的错误代码。

标准的错误代码统一编制，用来标示通用的和业务无关的错误信息或共同种类的错误。比如所有模块用 0 表示成功。用 900 到 999 表示共同的错误种类或统一处理的业务逻辑。比如请求参数错误用 901， 没有访问权限 912， 错误地址 922 等。这样方便在客户端和服务端用共同框架处理这类错误。

业务模块的错误代码自行定制。比如错误代码 2， 在用户系统表示用户名重复，在订单系统可能是订单过期。业务模块的客户端根据具体业务场景和错误代码给出正确的错误信息。

## 错误处理

软件都分成多个层级。高层代码的调用底层的函数/方法。这种上下层调用的错误处理非常简单：如果调用者知道如何处理错误，则捕获并处理这种错误。一个常见的例子是应用层知道网络超时需要重试。如果不知道怎么处理则不用捕获，由最上层（API 的对外出入口）代码或用户界面代码来做处理。
