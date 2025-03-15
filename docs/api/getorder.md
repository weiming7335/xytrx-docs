# 获取订单接口文档

## 请求说明

- **请求方法**：GET
- **请求URL**：`api/v1/order/{client_order_id}`

## 请求头

- `API-KEY`: `6E5D8B821A991EC12556662486B5E68A`
- `TIMESTAMP`: `1741778902`
- `SIGNATURE`: `5b2e589d6cddc217e4ac5cb9915d76116f28c052ab942b8c122571c285c61eb7`

## 返回示例

```json
{
  "code": 200,
  "message": "操作成功",
  "data": [
    {
      "quantity": 65000,
      "delegate_hash": "0df61a65f01c7739b8c3f942eb2e263cfcd10fd8d017ce63e9eaec886a467e65",
      "pay_amount": 2.380,
      "server_order_id": "6341bc66-e0c2-4f43-8a86-572b74c56ff8",
      "delegate_timestamp": 1741778923249,
      "delegate_status": "DELEGATION_CONFIRMED",
      "receive_address": "TSP7GWu4JJ2eY9XrY5QG8FUymRRC2cHc2j",
      "status": "DELEGATE_SUCCESS"
    },
    {
      "quantity": 131000,
      "delegate_hash": "67c4f4c6136a792c3987212baa5530384594105dd5f56492c7cf99dc94dcd629",
      "pay_amount": 4.797,
      "server_order_id": "8200c6af-851b-4239-a79b-a89bb033fd39",
      "delegate_timestamp": 1741778923879,
      "delegate_status": "DELEGATION_CONFIRMED",
      "receive_address": "TM5gtDqJTK5b92KHefHLJihxZKvNy6sBFY",
      "status": "DELEGATE_SUCCESS"
    }
  ]
}
```

## 返回参数说明

- **code** (integer): 状态码，200表示成功。
- **message** (string): 返回信息，通常为操作结果描述。
- **data** (array): 订单数据数组，每个元素包含以下字段：
  - **quantity** (integer): 能量数量。
  - **delegate_hash** (string): 委托哈希值。
  - **pay_amount** (float): 支付金额。
  - **server_order_id** (string): 服务器订单ID。
  - **delegate_timestamp** (integer): 委托时间戳。
  - **delegate_status** (string): 委托状态。 DELEGATION_CONFIRMED ： 委托成功｜ 
DELEGATION_CONFIRMED_FAIL: 委托失败 ｜ 
UNCONFIRMED: 未开始。 
  - **receive_address** (string): 接收地址。
  - **status** (string): 订单状态。
  PAYMENT_SUCCESS: 支付成功｜
  DELEGATE_SUCCESS：代理成功 ｜ 
INVALID_ADDRESS：无效激活 ｜ 
UNKNOWN：其他错误
