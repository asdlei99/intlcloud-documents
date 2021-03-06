## 功能描述

本接口用于修改机器组。

## 请求

#### 请求示例

```shell
PUT /machinegroup HTTP/1.1
Host: <Region>.cls.tencentyun.com
Authorization: <AuthorizationString>
Content-Type: application/json

{
    "group_id": "xxxx-xx-xx-xx-xxxxxxx", 
    "group_name": "testname", 
    "type"： "ip", 
    "ips": ["10.10.10.10", "10.10.10.11"]
}
```

#### 请求行

```shell
PUT /machinegroup
```

#### 请求头

除公共头部外，无特殊请求头部。

#### 请求参数

| 字段名        |  类型  | 位置  | 必须 |      含义                       |
|--------------|--------|------|---------|--------------------------------|
| group_id     | string | body | 是      |要修改的机器组的 ID                |
| group_name   | string | body | 否      |机器组的名字，不能重名             |
| type       | string    | body | 否   | 机器组类型，只支持 ip 和 label 两种类型，默认是 ip |
| ips          | JsonArray| body | 否      |机器组下的 IP 列表                  |
| labels     | JsonArray | body | 否   | label 机器组的标签表                             |


>!group_name 、 ips 和 labels 至少要提供一个。

## 响应

#### 响应示例

```shell
HTTP/1.1 200 OK
Content-Length: 0
```

#### 响应头

除公共响应头部外，无特殊响应头部。

#### 响应参数

无。

## 错误码

参加 [错误码](https://intl.cloud.tencent.com/document/product/614/12402) 文档。

