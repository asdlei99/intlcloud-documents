

## 简介

本文档提供关于存储桶标签的 API 概览以及 SDK 示例代码。

| API                                                          | 操作名         | 操作描述                         |
| ------------------------------------------------------------ | -------------- | -------------------------------- |
| [PUT Bucket tagging](https://intl.cloud.tencent.com/document/product/436/8281) | 设置存储桶标签 | 为已存在的存储桶设置标签         |
| [GET Bucket tagging](https://intl.cloud.tencent.com/document/product/436/8277) | 查询存储桶标签 | 查询指定存储桶下已有的存储桶标签 |
| [DELETE Bucket tagging](https://intl.cloud.tencent.com/document/product/436/8286) | 删除存储桶标签 | 删除指定的存储桶标签             |

## 设置存储桶标签

#### 功能说明

PUT Bucket tagging 用于为已存在的存储桶设置标签。

#### 方法原型

```go
func (s *BucketService) PutTagging(ctx context.Context, opt *BucketPutTaggingOptions) (*Response, error)
```

#### 请求示例

```go
opt := &cos.BucketPutTaggingOptions{
	TagSet: []cos.BucketTaggingTag{
	{   
		Key:   "testk1",
		Value: "testv1",
    },  
    {   
    	Key:   "testk2",
        Value: "testv2",
    },  
    },  
}   
resp, err := client.Bucket.PutTagging(context.Background(), opt)
```

#### 参数说明

```go
type BucketTaggingTag struct {
    Key   string
    Value string
}
type BucketPutTaggingOptions struct {
    XMLName xml.Name           
    TagSet  []BucketTaggingTag 
}
```

| 参数名称                | 描述                                                         | 类型   |
| ----------------------- | ------------------------------------------------------------ | ------ |
| BucketPutTaggingOptions | 存储桶标签配置参数                                           | Struct |
| TagSet                  | 存储桶标签配置信息                                           | Struct |
| key                     | 标签的 Key，长度不超过128字节，支持英文字母、数字、空格、加号、减号、下划线、等号、点号、冒号、斜线 | String |
| value                   | 标签的 Value，长度不超过256字节，支持英文字母、数字、空格、加号、减号、下划线、等号、点号、冒号、斜线 | String |

## 查询存储桶标签

#### 功能说明

GET Bucket tagging 用于查询指定存储桶下已有的存储桶标签。

#### 方法原型

```go
func (s *BucketService) GetTagging(ctx context.Context) (*BucketGetTaggingResult, *Response, error)
```

#### 请求示例

```go
v, resp, err := client.Bucket.GetTagging(context.Background())
```

#### 返回结果说明

```go
type BucketTaggingTag struct {
    Key   string
    Value string
}
type BucketGetTaggingResult struct {
    XMLName xml.Name           
    TagSet  []BucketTaggingTag 
}

```

| 参数名称               | 描述                                                         | 类型   |
| ---------------------- | ------------------------------------------------------------ | ------ |
| BucketGetTaggingResult | 存储桶标签配置参数                                           | Struct |
| TagSet                 | 存储桶标签配置信息                                           | Struct |
| key                    | 标签的 Key，长度不超过128字节，支持英文字母、数字、空格、加号、减号、下划线、等号、点号、冒号、斜线 | String |
| value                  | 标签的 Value，长度不超过256字节，支持英文字母、数字、空格、加号、减号、下划线、等号、点号、冒号、斜线 | String |

## 删除存储桶标签

#### 功能说明

DELETE Bucket tagging 用于删除指定存储桶下已有的存储桶标签。

#### 方法原型

```go
func (s *BucketService) DeleteTagging(ctx context.Context) (*Response, error)
```

#### 请求示例

```go
resp, err := client.Bucket.DeleteTagging(context.Background())
```
