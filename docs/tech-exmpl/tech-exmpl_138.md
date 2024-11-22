# 将查询参数字符串转换为 Ruby 语言中的查询参数哈希

> 原文：[`techbyexample.com/query-param-string-to-hash/`](https://techbyexample.com/query-param-string-to-hash/)

# **概述**

假设我们有以下查询参数字符串

```go
a=b&x=y
```

我们希望输出为

```go
{
 "a" => "b",
 "x" => "y"
}
```

# **程序**

以下是相同的程序

```go
input_sring = 'a=b&x=y'

query_params_hash = {}
input_string_split = input_sring.split('&')
input_string_split.each do |q|
    q_split = q.split('=')
    query_params_hash[q_split[0]] = q_split[1]
end 

puts query_params_hash
```

**输出**

```go
{"a"=>"b", "x"=>"y"}
```

+   [ruby](https://techbyexample.com/tag/ruby/)
