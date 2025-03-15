# 批量下单接口

## 请求方法
**POST**

## 请求URL
`/api/v1/order`

## 请求头
- `Content-Type: application/json`
- `API-KEY: 6E5D8B821A991EC12556662486B5E68A`
- `TIMESTAMP: 1741778902`
- `SIGNATURE: 5b2e589d6cddc217e4ac5cb9915d76116f28c052ab942b8c122571c285c61eb7`

## 请求体
```json
{
    "mainAddress":"TUfT8iNT95pVs1oG6VQhFqtGwdmGmR6mMB",
    "clientOrderId":"5d965651-3489-4cec-b45e-700353569735",
    "receiver":[{
            "quantity":65000,
            "receiveAddress":"TSP7GWu4JJ2eY9XrY5QG8FUymRRC2cHc2j"
        },
        {
            "quantity":131000,
            "receiveAddress":"TM5gtDqJTK5b92KHefHLJihxZKvNy6sBFY"
        }
        
    ]
}
```

## 参数说明

- **mainAddress** (string, 必填)
  - 描述：主地址，用于标识发送方或交易的主要地址。
  - 示例值：`"TUfT8iNT95pVs1oG6VQhFqtGwdmGmR6mMB"`

- **clientOrderId** (string, 必填)
  - 描述：客户端订单ID，用于唯一标识一个订单。
  - 示例值：`"5d965651-3489-4cec-b45e-700353569735"`

- **receiver** (array, 必填)
  - 描述：接收者信息的数组，每个元素包含接收者的详细信息。
  - 子参数：
    - **quantity** (integer, 必填)
      - 描述：接收者将接收的数量。
      - 示例值：`65000`
    - **receiveAddress** (string, 必填)
      - 描述：接收者的接收地址。
      - 示例值：`"TSP7GWu4JJ2eY9XrY5QG8FUymRRC2cHc2j"`

## 响应示例

### 成功响应
- **状态码**：200 OK
- **响应体**：
  ```json
  {
    "code": 200,
    "message": "操作成功",
    "data": null
  }
  ```
