# 获取订单接口文档

## 请求余额

- **请求方法**：GET
- **请求URL**：`api/v1/balance`

## 请求头

- `API-KEY`: `6E5D8B821A991EC12556662486B5E68A`
- `TIMESTAMP`: `1741778902`
- `SIGNATURE`: `5b2e589d6cddc217e4ac5cb9915d76116f28c052ab942b8c122571c285c61eb7`

## 返回示例

```json
{
    "code": 200,
    "message": "操作成功",
    "data": "14980.940"
}
```

## 返回参数说明

- **code** (integer): 状态码，200表示成功。
- **message** (string): 返回信息，通常为操作结果描述。
- **data** (double): trx余额：
  
  
