---
permalink: /index.html
---
# USDT永续合约API文档

**更新说明**：

文档说明：

API请求BaseURL：ex-admin.nxwise.io/api

注：

 1、以下所有接口地址请加上API请求BaseURL

 2、所有接口返回中id字段即为该记录在对应数据库表中的唯一数据id

 例：

   GET `/otcCoin `   即请求方式为GET，请求地址为https://ex-admin.nxwise.io/api/otcCoin

[TOC]

### 公共数据接口

#### 获取强平订单（公共）

请求方式及地址：GET`/swap_public/instruments/liquidation`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                      |
| ------------- | ------ | -------- | ----------------------------------------- |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP` |
| size          | string | Y        | 页数(默认 1)                              |
| page          | string | N        | 每页条数（默认20）                        |
| status        | string | Y        | 0或1  否则参数错误(未成交 或 已成交)      |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | 参数错误         |
| 102    | no instrument_id |

返回示例：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "timestamp": "2019-12-25 10:06:09",//委托时间
            "type": 1,//订单类型。1:买入开多 2:卖出开空 3:卖出平多 4:买入平空
            "price": "10.00000000",//委托价格
            "size": "10.00000",//委托数量
            "status": 1,//状态 0未成交 1已成交 
            "contract_val": "10.00000",//合约面值
            "loss": "0.00000000"//穿仓用户亏损
        }
    ],
    "total": 10.00000000, //穿仓用户亏损总数
    "count": 1 //总条数 
}
```

#### 风险准备金（公共）

请求方式及地址：GET`/swap_public/instruments/risk_reserve`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                      |
| ------------- | ------ | -------- | ----------------------------------------- |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP` |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | no instrument_id |

返回示例：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "created_at": "2019-12-06 13:05:51",//日期
            "type": 1,//类型 0 系统注入 1强平剩余注入 
            "amount": "0.00000000",//金额
            "balance": "1000.00081600"//余额
        },
        {
            "created_at": "2019-12-06 13:05:30",
            "type": 1,
            "amount": "0.00000000",
            "balance": "1000.00081600"
        },
        {
            "created_at": "2019-12-06 13:04:50",
            "type": 1,
            "amount": "0.00000000",
            "balance": "1000.00081600"
        },
        {
            "created_at": "2019-12-06 13:04:30",
            "type": 1,
            "amount": "0.00000000",
            "balance": "1000.00081600"
        },
        {
            "created_at": "2019-12-06 13:03:50",
            "type": 1,
            "amount": "0.00000000",
            "balance": "1000.00081600"
        },
        {
            "created_at": "2019-12-06 13:03:30",
            "type": 1,
            "amount": "0.00000000",
            "balance": "1000.00081600"
        },
        {
            "created_at": "2019-12-06 13:02:50",
            "type": 1,
            "amount": "0.00000000",
            "balance": "1000.00081600"
        }
    ],
    "total": "1000.00081600"//风险准备金 最新余额
}
```

#### 结算记录（公共）

请求方式及地址：GET`/swap_public/instruments/settlement`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                      |
| ------------- | ------ | -------- | ----------------------------------------- |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP` |
| size          | string | Y        | 页数(默认 1)                              |
| page          | string | N        | 每页条数（默认20）                        |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | no instrument_id |

返回示例：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "created_at": "2019-12-04 10:43:35",//结算日期
            "set_price": "7880.00000000",//结算价格 Settlement Price
            "res_apt": "0.00000000",//穿仓用户亏损分摊(准备金分摊)Reserve apportionment
            "res_apt_percent": "0.00000000",//分摊比例
            "total_profit": "0.00000000",//总盈利
            "total_loss": "0.00000000"//总亏损
        },
        {
            "created_at": "2019-12-04 10:27:19",
            "set_price": "7880.00000000",
            "res_apt": "0.00000000",
            "res_apt_percent": "0.00000000",
            "total_profit": "0.00000000",
            "total_loss": "0.00000000"
        }
    ],
    "count": 1
}
```

#### 资金费率，最近7天的数据。（公共）

请求方式及地址：GET`/swap_public/instruments/historical_funding_rate`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                      |
| ------------- | ------ | -------- | ----------------------------------------- |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP` |
| size          | string | Y        | 页数(默认 1)                              |
| page          | string | N        | 每页条数（默认20）                        |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | no instrument_id |

返回示例：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "created_at": "2019-12-15 14:34:52",//日期
            "instrument_id": "BTC-USDT-SWAP",//合约名称
            "expected_funding_rate": "28.682134",//预测资金费率
            "real_funding_rate": "3.441856"//实际资金费率
        },
        {
            "created_at": "2019-12-15 14:34:13",
            "instrument_id": "BTC-USDT-SWAP",
            "expected_funding_rate": "28.682134",
            "real_funding_rate": "3.441856"
        },
        {
            "created_at": "2019-12-15 14:32:46",
            "instrument_id": "BTC-USDT-SWAP",
            "expected_funding_rate": "28.682134",
            "real_funding_rate": "3.441856"
        }
    ],
    "count": 1
}
```

#### 仓位档位说明（公共）

请求方式及地址：GET `/swap_public/instruments/block_list`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                      |
| ------------- | ------ | -------- | ----------------------------------------- |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP` |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | no instrument_id |

返回示例：

```json
{
  "success": true,
  "code": 1,
  "msg": "获取成功",
  "data": [
    {
      "id": 1,
      "min_amount": 0,                       //最小张数
      "max_amount": 50000,                   //最大张数
      "position_margin_rate": "0.0050",      //维持保证金率
      "min_level_open_margin_rate": "0.0100",//最低起始保证金率
      "max_level_rate": "100.00",             //最高可用杠杆倍数
      "mul_value": "20000"                    //档位计算值
    }
  ]
}


```



#### 获取实时成交数据（公共）

请求：GET `/swap_public/getTradeDetail`

参数：

|     字段      | 类型   | 是否必选 |                         说明                          |
| :-----------: | :----- | :------: | :---------------------------------------------------: |
| instrument_id | string |    Y     | 合约名称（BTC-USDT-SWAP,ETH-USDT-SWAP,EOS-USDT-SWAP） |

返回结果及code状态码说明：

| code值 |       说明       |
| :----: | :--------------: |
|   1    |       成功       |
|  101   | no instrument_id |
|  102   |       失败       |

返回结果：

最大data有100条，要是没有数据 data 为 null

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "amount": "0.12",    //成交数量
            "ts": 1574904910,     //成交时间
            "id": 102,            //id
            "price": "12.2",     // 成交价格
            "direction": "buy"   //成交方向
        }
    ]
}
```



#### 获取系统时间（公共）

请求：GET `/swap_public/timestamp`

参数：无

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": 1578362408 //系统 unix时间（类型int）
}
```



#### 获取市场（盘口/深度）行情数据（公共）

 请求：GET  `/swap_public/depth `

 参数：

|     字段      |  类型  | 是否必选 |                       备注                        |
| :-----------: | :----: | :------: | :-----------------------------------------------: |
| instrument_id | string |    Y     | 市场（BTC-USDT-SWAP,ETH-USDT-SWAP,EOS-USDT-SWAP） |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | no instrument_id |
| 102    | 失败             |

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": {
        "ch": "BTC-USDT-SWAP",
        "ts": 1578362755158,
        "data": {
            "asks": null,
            "bids": [
                [
                    "156.326",
                    "8"
                ],
                [
                    "152.48",
                    "0.7"
                ],
                [
                    "3",
                    "6"
                ],
                [
                    "2",
                    "6"
                ]
            ]
        }
    }
}
```



#### 获取k线历史记录（公共）

请求：GET  `/swap_public/k_lines`

参数：

|     字段      |  类型  | 是否必选 |                             备注                             |
| :-----------: | :----: | :------: | :----------------------------------------------------------: |
| instrument_id | string |    Y     |      市场（BTC-USDT-SWAP,ETH-USDT-SWAP,EOS-USDT-SWAP）       |
|    period     | string |    Y     | 周期（time,1min, 5min, 15min, 30min, 60min, 4hour,  1week，1mon,） |
|    endTime    |  int   |    N     |        结束时间，没有结束时间默认读取最新500条数据；         |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | no instrument_id |
| 102    | 失败             |
| 103    | 参数错误         |

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": {
        "noData": false,     //当 =true时表示没有多余的数据了，此时 data=null
        "data": [            
            {   
                "id": 41,
                "open": "10.3",
                "close": "10.3",
                "low": "10.3",
                "high": "0",
                "amount": "10.22",
                "vol": "105.266",
                "time": 1575518125
            }
        ]
    }
}
```



####  获取k线历史记录（数组形式）（公共）

   这种形式会慢一些

请求:GET  `/swap_public/history/kline`

参数：

|     字段      |  类型  | 是否必选 |                             备注                             |
| :-----------: | :----: | :------: | :----------------------------------------------------------: |
| instrument_id | string |    Y     |      市场（BTC-USDT-SWAP,ETH-USDT-SWAP,EOS-USDT-SWAP）       |
|    period     | string |    Y     | 周期（time,1min, 5min, 15min, 30min, 60min, 4hour,  1week，1mon,） |
|    endTime    |  int   |    N     |        结束时间，没有结束时间默认读取最新500条数据；         |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | no instrument_id |
| 102    | 失败             |
| 103    | 参数错误         |

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        [
            1575627300,   //time 
            3.5934,      //low
            20.6565,      //high
            5.4777,  	//open	
            11.9765,    //close
            49948.764444 //volume
        ]
   ]
}
```

   当数据没有值时 返回：data=null



#### 获取现货指数价格（公共）

请求：GET `/swap_public/index_price`

参数：

| 字段          | 类型   | 是否必选 | 备注                                                         |
| ------------- | ------ | -------- | ------------------------------------------------------------ |
| instrument_id | string | Y        | $market可选择的值（BTC-USDT-SWAP,ETH-USDT-SWAP,EOS-USDT-SWAP） |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | no instrument_id |
| 102    | 失败             |

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": "7893.7975" //类型 string 价格
}
```



**撮合系统需要读取这个值 ，直接读取redis里面的值  键和这里的market参数一样**

#### 获取头部24H ticker（公共）

请求：GET  `/swap_public/get_head_ticker`

参数：

|     字段      |  类型  | 是否必选 |                          备注                           |
| :-----------: | :----: | :------: | :-----------------------------------------------------: |
| instrument_id | string |    Y     | 可选择的值（BTC-USDT-SWAP,ETH-USDT-SWAP,EOS-USDT-SWAP） |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | no instrument_id |
| 102    | 失败             |

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": {
        "Id": 1,
        "last": "42.23333",     //最新成交价  
        "best_bid": "22.23333", //买一价
        "best_ask": "22.23333", //卖一价
        "open": "22.23333",    //24h开盘价
        "high": "42.23333",   //24h最高价
        "low": "22.23333",    //24h最低价
        "amount": "135", 	  //24h成交量
        "vol": "4381.49955",  //24h成交额
        "time": 1576139664   //系统时间戳
    }
}
```



#### 头部费率指数（公共）

 请求：GET  `/swap_public/get_head_index`

 参数：

|     字段      |  类型  | 是否必选 |                          备注                           |
| :-----------: | :----: | :------: | :-----------------------------------------------------: |
| instrument_id | string |    Y     | 可选择的值（BTC-USDT-SWAP,ETH-USDT-SWAP,EOS-USDT-SWAP） |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | no instrument_id |
| 102    | 失败             |

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": {
        "index_price": 7187.875,  //现货指数 float
        "mark_price": 0,        //标记价格 float
        "depot_num": 10.05,   //持仓量 float
        "index_pirce_rate": 0, //当前资金费率 float
        "forecast_pirce_rate": 0,  //预测资金费率 float
    }
}
```



#### 所有合约列表（公共）

请求方式及地址：GET `/swap_public/get_swap_name`

请求参数：无

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "id": 1,   // 类型 int  系统id
            "instrument_id": "BTC-USDT-SWAP", //永续合约名称（也是主题名字）
            "base_currency": "BTC",       //币种，如BTC
            "quote_currency": "USDT",     //计价货币，如BTC-USDT-SWAP中的USDT
            "base_quote": "BTCUSDT",   //币种+计价 如BTCUSD
            "settlement_currency": "USDT",  //盈亏结算和保证金币种，USDT
            "contract_val_currency": "BTC",//合约面值计价币种
            "contract_value": "0.0001",//合约面值也即最小下单数量 如 0.0001 即代表0.0001BTCT
            "tick_size": "0.1",     //最小价位变动  也即价格精度
            "size_increment": "0.0001",  //数量精度
            "max_leve": "100",   //最高可用杠杆倍数
            "min_leve": "0.01",   //最高小用杠杆倍数
            "new_price": 22.23333,    //float 最新价格
            "change":0.0215,              //涨跌幅
            "amount": "4.01" //24H成交量
        },
        {
            "id": 2,
            "instrument_id": "ETH-USDT-SWAP",
            "base_currency": "ETH",
            "quote_currency": "USDT",
            "base_quote": "ETHUSDT",
            "settlement_currency": "USDT",
            "contract_val_currency": "ETH",
            "contract_value": "0.001",
            "tick_size": "0.01",
            "size_increment": "0.001",
            "max_leve": "50",
            "min_leve": "0.01",
            "new_price": 0,
            "change": 0,
            "amount": "4.01" //24H成交量
        },
    ]
}
```

| 字段                   | 说明                   |
| ---------------------- | ---------------------- |
| 参照上面返回字段的注释 | 参照上面返回字段的注释 |



#### 单个合约列表（公共）

请求方式及地址：GET `/swap_public/get_swap_name_one`

请求参数：

|     字段      | 类型   | 是否必选 |                         说明                          |
| :-----------: | :----- | :------: | :---------------------------------------------------: |
| instrument_id | string |    Y     | 合约名称（BTC-USDT-SWAP,ETH-USDT-SWAP,EOS-USDT-SWAP） |

返回结果及code状态码说明：

| code值 |       说明       |
| :----: | :--------------: |
|   1    |       成功       |
|  101   | no instrument_id |
|  102   |       失败       |

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": {
        "id": 2,
        "instrument_id": "ETH-USDT-SWAP",
        "base_currency": "ETH",
        "quote_currency": "USDT",
        "base_quote": "ETHUSDT",
        "settlement_currency": "USDT",
        "contract_val_currency": "ETH",
        "contract_value": "0.001",
        "tick_size": "0.01",
        "size_increment": "0.001",
        "max_leve": "50",
        "min_leve": "0.01",
        "new_price": 22.23333,
        "change": 0.09,
        "amount": "4.01" //24H成交量
    }
}
```



### 用户合约持仓信息

#### 获取当前用户的所有持仓

请求方式及地址：GET `/swap_api/positions`

请求参数：

| 字段  | 类型   | 是否必选 | 说明                    |
| ----- | ------ | -------- | ----------------------- |
| token | string | Y        | 登录之后获取的验证token |

返回结果及参数说明：

```json
{
    "success": true,
    "code": 1,
    "msg": "成功",
    "data": [
            {
                "avail_position": "5.0000",//可平数量
                "avg_cost": "6720.78000000",//	开仓平均价
                "instrument_id": "BTC-USDT-SWAP",//	合约名称
                "last": "6728.00000000",//最新成交价
                "leverage": 10,//杠杆
                "liquidation_price": "6703.19000000",//	预估强平价
                "maint_margin_ratio": "10.000000",//维持保证金率
                "margin": "1000.00000000",//保证金
                "position": "5.0000",//持仓数量
                "realized_pnl": "0.00000000",//	已实现盈亏
                "settled_pnl": "-0.00113313",//	已结算收益
                "settlement_price": "7880.00000000",//开仓平均价
                "side": "short",//	方向
                "created_at": "2019-11-25 17:34:37",//创建时间
                "margin_mode": "crossed", //仓位模式：全仓crossed 	仓位模式：逐仓fixed
                "margin_ratio": 0,//保证金率
                "unlong_pnl": 1, //未实现盈亏
                "long_pnl": 1, //收益
                "long_pnl_rate": 0 //收益率
            },
            {
                "avail_position": "5.0000",
                "avg_cost": "6720.78000000",
                "instrument_id": "EOS-USDT-SWAP",
                "last": "6728.00000000",
                "leverage": 10,
                "liquidation_price": "6703.19000000",
                "maint_margin_ratio": "10.000000",
                "margin": "1000.00000000",
                "position": "5.0500",
                "realized_pnl": "0.00000000",
                "settled_pnl": "-0.00113313",
                "settlement_price": "7880.00000000",
                "side": "short",
                "created_at": "2019-11-25 17:34:37",
                "margin_mode": "fixed",
                "margin_ratio": 0,//保证金率
                "unlong_pnl": 1, //未实现盈亏
                "long_pnl": 1, //收益
                "long_pnl_rate": 0 //收益率
            }
        ]
}
```

| 字段                   | 说明                   |
| ---------------------- | ---------------------- |
| 参照上面返回字段的注释 | 参照上面返回字段的注释 |

#### 用户单个合约持仓信息

请求方式及地址：GET `/swap_api/position`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                      |
| ------------- | ------ | -------- | ----------------------------------------- |
| token         | string | Y        | 登录之后获取的验证token                   |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP` |

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": {
        "crossed": [
            {
                "id":1,
                "avail_position": "5.0000",//可平数量
                "avg_cost": "6720.78000000",//	开仓平均价
                "instrument_id": "BTC-USDT-SWAP",//	合约名称
                "last": "6728.00000000",//最新成交价
                "leverage": 10,//杠杆
                "liquidation_price": "6703.19000000",//	预估强平价
                "maint_margin_ratio": "10.000000",//维持保证金率
                "margin": "1000.00000000",//保证金
                "position": "5.0000",//持仓数量
                "realized_pnl": "0.00000000",//	已实现盈亏
                "settled_pnl": "-0.00113313",//	已结算收益
                "settlement_price": "7880.00000000",//结算基准价
                "side": "short",//	方向 'long'多 'short' 空
                "created_at": "2019-11-25 17:34:37",//创建时间
                "margin_mode": "crossed",//仓位模式：全仓crossed 	仓位模式：逐仓fixed
                "margin_ratio": 0,//保证金率
                "un_pnl": 1, //未实现盈亏
                "pnl": 1, //收益
                "pnl_rate": 0 //收益率
            }
        ]
    }
}
```

| 字段                   | 说明                   |
| ---------------------- | ---------------------- |
| 参照上面返回字段的注释 | 参照上面返回字段的注释 |

### 合约账户信息

#### 所有币种合约的账户信息

 请求方式及地址：GET `/swap_api/accounts`

请求参数：

| 字段  | 类型   | 是否必选 | 说明                    |
| ----- | ------ | -------- | ----------------------- |
| token | string | Y        | 登录之后获取的验证token |

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "equity": "1.00000000",//账户权益 (账号动态权益)  USDT
            "total_avail_balance": "0.00079514",//账户余额（账户静态权益）USDT
            "fixed_balance": "0.00000000",//逐仓余额
            "margin": "1000.00000000",//保证金（挂单冻结+持仓已用)  USDT
            "realized_pnl": "0.00000000",//已实现盈亏
            "unrealized_pnl": "0.00000000",//未实现盈亏
            "margin_ratio": "1.00",//保证金率
            "instrument_id": "",//合约名称 "BTC-USDT-SWAP"
            "margin_frozen": "1000.00000000",//开仓(挂单)冻结保证金
            "margin_mode": "crossed",//仓位模式 全仓：crossed  逐仓 fixed
            "maintain_margin_ratio": "1.00",//维持保证金率
            "underlying": "BTC-USDT",//标的指数，如：BTC-USDT，BTC-USDT
            "created_at": "2019-11-27 11:53:16"//创建时间
        },
        null,
        null
    ],
    "balanceTotal": 2551 //总资产值 
    totalLegalAssets: "745201.77 CNY" //折合法币
    totalBtcAssets: "13.338146 BTC"   //折合BTC
}
```

返回结果及code状态码说明：

| code值                 | 说明                   |
| ---------------------- | ---------------------- |
| 1                      | 获取成功               |
| 参照上面返回字段的注释 | 参照上面返回字段的注释 |

#### 用户单个币种的账户信息

 请求方式及地址：GET `/swap_api/account`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                      |
| ------------- | ------ | -------- | ----------------------------------------- |
| token         | string | Y        | 登录之后获取的验证token                   |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP` |

返回结果：

```json
{
    "success": true,
    "user_id": 2201,
    "data": {
        "equity": "1.00000000",//账户权益 (账号动态权益)  USDT
        "total_avail_balance": "0.00079514",//账户余额（账户静态权益）USDT
        "fixed_balance": "0.00000000",//逐仓余额
        "margin": "1000.00000000",//保证金（挂单冻结+持仓已用)  USDT
        "realized_pnl": "0.00000000",//已实现盈亏
        "unrealized_pnl": "0.00000000",//未实现盈亏
        "margin_ratio": "1.00",//保证金率
        "instrument_id": "BTC-USDT-SWAP",//合约名称 "BTC-USDT-SWAP"
        "margin_frozen": "1000.00000000",//开仓(挂单)冻结保证金
        "margin_mode": "crossed",//仓位模式 全仓：crossed  逐仓 fixed
        "maintain_margin_ratio": "1.00",//维持保证金率
        "underlying": "BTC-USDT",//标的指数，如：BTC-USDT，BTC-USDT
        "created_at": "2019-11-27 11:53:16",//创建时间
        "taker_fee": "0.045",//	吃单手续费率
        "maker_fee": "0.018",//挂单手续费率
    }
}

//错误  （合约名称不存在时返回）
{
    "success": false,
    "code": 101,
    "msg": "no instrument_id"
}
```

返回结果及code状态码说明：

| code值                 | 说明                   |
| ---------------------- | ---------------------- |
| 1                      | 获取成功               |
| 101                    | 获取失败               |
| 参照上面返回字段的注释 | 参照上面返回字段的注释 |

### 合约订单相关

#### 下单

请求方式及地址：POST`/swap_api/order`

请求参数：

| 字段         | 类型   | 是否必选 | 说明                                                         |
| ------------ | ------ | -------- | ------------------------------------------------------------ |
| token        | string | Y        | 登录之后获取的验证token                                      |
| instrumentId | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP`                    |
| type         | string | Y        | 交易类型   1:买入开多 2:卖出开空 3:卖出平多 4:买入平空       |
| entrustType  | string | Y        | 委托类型：值只能为 limit0 (限价委托)，limit1 (高级限价委托)  stop_sl (止盈止损) |
| orderType    | string | Y        | 0 普通委托 1 只做maker 2立即成交或全部取消 3立即取消并取消剩余,      **仅当entrustType值为stop_sl即止盈止损下单时此值可以不传** |
| price        | string | Y        | 委托价格                                                     |
| amount       | string | Y        | 委托量                                                       |
| triggerPrice | string | Y        | 触发价格，**仅当entrustType值为stop_sl即止盈止损下单时此值必填** |

返回结果及code状态码说明：

**下单成功后，委托订单详情将通过websocket,推送给响应用户**

```json
{
    "success": true,
    "code": 1,
    "msg": "下单成功",
    "data": ""
}
```

#### 撤单

请求方式及地址：POST `/swap_api/cancel_order `

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                                         |
| ------------- | ------ | -------- | ------------------------------------------------------------ |
| token         | string | Y        | 登录之后获取的验证token                                      |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP`                    |
| entrustType   | string | Y        | 委托类型：值只能为 limit0 (限价委托)，limit1 (高级限价委托)  stop-sl (止盈止损) |
| orderId       | string | Y        | 委托单订单id                                                 |

返回结果及code状态码说明：

```json
{
    "success": true,
    "code": 1,
    "msg": "撤单成功",
    "data": ""
}

```



#### 获取所有限价订单列表(当前委托) 

请求方式及地址：GET `/swap_api/orders`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                                         |
| ------------- | ------ | -------- | ------------------------------------------------------------ |
| token         | string | Y        | 登录之后获取的验证token                                      |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP`                    |
| match_type    | string | Y        | 委托类型：值只能为 limit0 (限价委托，默认请求为 限价委托)，limit1 (高级限价委托)  stop_sl (止盈止损) |
| type          | int    | N        | 交易类型 默认为全部类型 参数值只能为（ 1:买入开多 2:卖出开空 3:卖出平多 4:买入平空） |
| page          | int    | Y        | 页数(默认 1)                                                 |
| size          | int    | N        | 每页条数（默认20）                                           |

返回结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 获取成功         |
| 101    | no instrument_id |
| 102    | 参数错误         |
| 103    | 委托类型错误     |

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    
    "data": [
        {
            "id": 1,
            "timestamp": "2019-12-14 11:36:51",//委托时间
            "instrument_id": "BTC-USDT-SWAP",//合约
            "leverage": "10.00000000",//杠杆
            "type": 1,//交易类型。1:买入开多 2:卖出开空 3:卖出平多 4:买入平空
            "fee": "1.00000000",//手续费
            "filled_qty": "10.01542",//已成交量
            "amount": "10.01542",//委托总量
            "price_avg": "10.00000000",//成交均价
            "price": "1.00000000",//委托价格
            "order_type": 0,//委托类型（高级限价委托中 生效机制 字段值）
                                //0：普通委托
                                //1：只做Maker（Post only）
                                //2：全部成交或立即取消（FOK）
                                //3：立即成交并取消剩余（IOC）
            "need_margin": "100.00000000",//保证金
            "fee": "1.00000000",//手续费
            "status": 1,//状态           //-2:失败
                                        //-1:撤单成功
                                        //0:等待成交
                                        //1:部分成交
                                        //2:完全成交
                                        //3:下单中
                                        //4:撤单中
            "order_id": "b1b28964-21d3-49e5-899f-c40e07c6b88f",//委托单id
            "contract_val": "0.00010",//合约面值 如 0.0001 BTC
            "income": "0.00000000", //收益
            "enstrust_type": "limit0" // limit0 限价 limit1 高级限价
        }
    ],
    "count":1
}

//当match_type 为stop-sl止盈止损 时  数据对象如下 字段注释对应页面
  {
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "id": 1,
            "timestamp": "2019-12-14 11:36:51",////委托时间
            "instrument_id": "BTC-USDT-SWAP",//合约
            "leverage": "10.00000000",//杠杆
            "type": 1,//交易类型。1:买入开多 2:卖出开空 3:卖出平多 4:买入平空
            "amount": "10.01542",//委托数量
            "trigger_price": "1.00000000",//触发价格
            "price": "100.00000000",//委托价格（当达到触发价格时以此价格下单）
            "order_id": "b1b28964-21d3-49e5-899f-c40e07c6b88f",//委托单id
            "order_type": 1,//委托类型  1：止盈止损 2：跟踪委托 3：冰山委托 4：时间加权
            "status": 1,//状态
            		    //-2:失败
                        //-1:撤单成功
                        //0:等待成交
                        //1:部分成交
                        //2:完全成交
                        //3:下单中(暂时无用)
                        //4:撤单中(暂时无用)
                        //5:待生效
                        //6:部分生效
                        //7:已生效
            "contract_val": "0.00010",//合约面值 如 0.0001 BTC
            "income": "0.00000000", //收益
            "enstrust_type": "stop_sl"  //stop_sl 止盈止损
        }
    ],
    "count":1
}

```



#### 获取所有历史订单列表（历史委托）

请求方式及地址：GET `/swap_api/history_orders`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                                         |
| ------------- | ------ | -------- | ------------------------------------------------------------ |
| token         | string | Y        | 登录之后获取的验证token                                      |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP`                    |
| match_type    | string | Y        | 委托类型：值只能为 limit0 (限价委托，默认请求为 限价委托)，limit1 (高级限价委托)  stop_sl (止盈止损) |
| type          | int    | N        | 交易类型 默认为全部类型 参数值只能为（ 1:买入开多 2:卖出开空 3:卖出平多 4:买入平空） |
| status        | string | N        | 委托状态 默认为 已成交（2） 参数值只能为（-2 委托失败 -1 已撤销 2 已成交） |
| start_time    | string | N        | 开始时间 默认为七天之前 2019-12-01                           |
| end_time      | string | N        | 结束时间 默认为当前时间 2019-12-10                           |
| page          | int    | Y        | 页数(默认 1)                                                 |
| size          | int    | N        | 每页条数（默认20）                                           |

结果及code状态码说明：

| code值 | 说明             |
| ------ | ---------------- |
| 1      | 成功             |
| 101    | no instrument_id |
| 102    | 参数错误         |
| 103    | 委托类型错误     |

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "id": 1,
            "timestamp": "2019-12-14 11:36:51",//委托时间
            "instrument_id": "BTC-USDT-SWAP",//合约名称
            "type": 1,//订单类型。1:开多 2:开空 3:平多 4:平空
            "fee": "1.00000000",//手续费
            "leverage": "10.00000000",//杠杆
            "filled_qty": "10.01542",//成交数量
            "amount": "10.01542",//委托数量
            "order_id": "b1b28964-21d3-49e5-899f-c40e07c6b88f",//委托单id
            "price": "1.00000000",//委托价格
            "order_type": 0,//委托类型  	
                                //0：普通委托
                                //1：只做Maker（Post only）
                                //2：全部成交或立即取消（FOK）
                                //3：立即成交并取消剩余（IOC）
            "price_avg": "10.00000000",//成交均价
            "need_margin": "0.00000000",//保证金
            "contract_val": "0.00010",//合约面值 如 0.0001 BTC
            "income": "2000.00000000",//收益
            "status": 1//-2:失败
                                        //-1:撤单成功
                                        //0:等待成交
                                        //1:部分成交
                                        //2:完全成交
                                        //3:下单中
                                        //4:撤单中
        }
    ],
    "count":1
}

//当match_type 为stop-sl止盈止损 时  数据对象如下  字段注释对应页面
  {
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "id": 1,
            "timestamp": "2019-12-14 11:36:51",////委托时间
            "instrument_id": "BTC-USDT-SWAP",//合约
            "leverage": "10.00000000",//杠杆
            "type": 1,//交易类型。1:买入开多 2:卖出开空 3:卖出平多 4:买入平空
            "amount": "10.01542",//委托数量 以及实际委托数量
            "trigger_price": "1.00000000",//触发价格
            "price": "100.00000000",//委托价格 以及实际委托价格（当达到触发价格时以此价格下单）
            "order_id": "b1b28964-21d3-49e5-899f-c40e07c6b88f",//委托单id
            "order_type": 1,//委托类型  1：止盈止损 2：跟踪委托 3：冰山委托 4：时间加权
            "status": 1,//状态
            		    //-2:失败
                        //-1:撤单成功
                        //0:等待成交
                        //1:部分成交
                        //2:完全成交
                        //3:下单中
                        //4:撤单中
            "contract_val": "0.00010",//合约面值 如 0.0001 BTC
            "need_margin": "0.00000000",//保证金
            "income": "0.00000000" //收益
        }
    ],
    "count":1
}

//错误  （合约名称不存在时返回）
{
    "success": false,
    "code": 101,
    "msg": "no instrument_id"
}
```

#### 获取成交明细

请求方式及地址：GET `/swap_api/fills`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                                         |
| ------------- | ------ | -------- | ------------------------------------------------------------ |
| token         | string | Y        | 登录之后获取的验证token                                      |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP`                    |
| size          | string | Y        | 页数(默认 1)                                                 |
| page          | string | N        | 每页条数（默认20）                                           |
| order_id      | string | N        | 订单号,不填写`order_id`，返回当前`instrument_id`下的所有订单成交明细 |

结果及code状态码说明：

| code值 | 说明     |
| ------ | -------- |
| 1      | 成功     |
| 101    | 获取失败 |

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "order_id": "b1b28964-21d3-49e5-899f-c40e07c6b88f",//订单号
            "instrument_id": "EOS-USDT-SWAP",//合约名称
            "price": "1000.00000000",//	价格
            "amount": "100.00000",//成交数量
            "fee": "50.00000000",//手续费
            "deal_time": "2019-12-14 16:22:42",//成交时间
            "exec_type": "M",//流动性方向 T：taker; M：maker
            "type": 1//1:买入开多 2:卖出开空 3:卖出平多 4:买入平空
        }
    ],
    "count":1
}

//错误  （合约名称不存在时返回）
{
    "success": false,
    "code": 101,
    "msg": "no instrument_id"
}
```

### 用户合约设置相关

#### 获取某个合约的用户配置

请求方式及地址：GET `/swap_api/accounts/settings`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                      |
| ------------- | ------ | -------- | ----------------------------------------- |
| token         | string | Y        | 登录之后获取的验证token                   |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP` |

返回结果及code状态码说明：

| code值 | 说明 |
| ------ | ---- |
| 1      | 成功 |

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": {
        "long_leverage": "10.00",//多仓杠杆
        "short_leverage": "10.00",//空仓杠杆
        "margin_mode": "crossed",//	持仓模式 ,全仓crossed
        "isEdit": false, //是否可以调整杠杆倍数 true为可以 false为不可以
        "margin": 0.2 //当前持仓所需保证金
    }
}
```

####  用户设定某个合约的杠杆倍数

请求方式及地址：POST `/swap_api/accounts/leverage`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                      |
| ------------- | ------ | -------- | ----------------------------------------- |
| token         | string | Y        | 登录之后获取的验证token                   |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP` |
| leverage      | string | Y        | 杠杆倍数（不填默认为10倍）                |

结果及code状态码说明：

| code值 | 说明     |
| ------ | -------- |
| 1      | 成功     |
| 101    | 获取失败 |

返回结果：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": {
        "id": 9,
        "long_leverage": "10.00",//多仓杠杆
        "short_leverage": "10.00",//空仓杠杆
        "margin_mode": "crossed",//	持仓模式 ,全仓crossed
        "instrument_id": "ETH-USDT-SWAP"//合约名称
    }
}


//错误信息
{
    "success": false,
    "code": 101,
    "msg": "setting error"
}
```

### 合约流水账单

#### 合约流水账单类型列表

请求方式及地址：GET`/swap_public/instruments/ledger_type`

请求参数：无

返回结果及code状态码说明：

| code值 | 说明 |
| ------ | ---- |
| 1      | 成功 |

返回示例：

```json
{
    "success": true,
    "code": 1,
    "msg": "获取成功",
    "data": [
        {
            "id": 21,
            "name": "强平",
            "data": [
                {
                    "id": 10,
                    "name": "强平多"
                },
                {
                    "id": 11,
                    "name": "强平空"
                },
                {
                    "id": 25,
                    "name": "强减多"
                },
                {
                    "id": 26,
                    "name": "强减空"
                }
            ]
        },
        {
            "id": 22,
            "name": "划转",
            "data": [
                {
                    "id": 5,
                    "name": "转入"
                },
                {
                    "id": 6,
                    "name": "转出"
                },
                {
                    "id": 13,
                    "name": "手动追加"
                },
                {
                    "id": 14,
                    "name": "手动减少"
                },
                {
                    "id": 15,
                    "name": "自动追加"
                },
                {
                    "id": 19,
                    "name": "用户调低杠杆追加保证金"
                }
            ]
        },
        {
            "id": 23,
            "name": "交割结算",
            "data": [
                {
                    "id": 7,
                    "name": "清算未实现"
                },
                {
                    "id": 8,
                    "name": "分摊"
                },
                {
                    "id": 12,
                    "name": "资金费"
                },
                {
                    "id": 20,
                    "name": "清算已实现"
                }
            ]
        },
        {
            "id": 24,
            "name": "交易",
            "data": [
                {
                    "id": 1,
                    "name": "开多"
                },
                {
                    "id": 2,
                    "name": "开空"
                },
                {
                    "id": 3,
                    "name": "平多"
                },
                {
                    "id": 4,
                    "name": "平空"
                }
            ]
        }
    ]
}
```

#### 合约账户 账单

请求方式及地址：GET`/swap_api/accounts/ledger`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                                         |
| ------------- | ------ | -------- | ------------------------------------------------------------ |
| token         | string | Y        | 登录之后获取的验证token                                      |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP`                    |
| size          | string | Y        | 页数(默认 1)                                                 |
| page          | string | N        | 每页条数（默认20）                                           |
| type          | string | N        | 账单类型(默认为空 所有类型 **参照接口18 传入值为id 不是name** ) |
| start_time    | string | Y        | 开始时间 默认为七天之前 2019-12-01                           |
| end_time      | string | Y        | 结束时间 默认为当前时间 2019-12-10                           |
| time_type     | string | N        | 值为 1 或 2  APP端需必须要此参数，其他为空                   |

返回结果及code状态码说明：

| code值 | 说明 |      |
| ------ | ---- | ---- |
| 1      | 成功 |      |

返回示例：

```json
{
  "success": true,
  "code": 1,
  "msg": "获取成功",
  "data": [
    {
      "id":7,
      "timestamp": "2019-12-15 12:36:41",//账单创建时间
      "instrument_id": "BTC-USDT-SWAP",//合约名称
      "type": 1,//流水来源类型	
                            //开多1
                            //开空2
                            //平多3
                            //平空4
                            //转入5
                            //转出6
                            //清算未实现7
                            //分摊8
                            //强平多10
                            //强平空11
                            //资金费12
                            //手动追加13
                            //手动减少14
                            //自动追加15
                            //用户调低杠杆追加保证金19
                            //清算已实现20
                            //强减多25
                            //强减空26
      "fee": "0.00000000",//手续费
      "amount": "-37159.08905809",//变动金额（USDT）
      "currency": "BTC",//币种 (USDT)
      "order_id": 2,//订单ID
      "balance": 185795,//开仓平仓的数量（开仓为正数，平仓为负数，当type是fee时，balance为0）
      "remarks": "收取资金费用"//备注
    },
    {
      "id":2,
      "timestamp": "2019-12-15 12:36:19",
      "instrument_id": "BTC-USDT-SWAP",
      "type": "funding",
      "fee": "0.00000000",
      "amount": "-37159.08905809",
      "currency": "BTC",
      "order_id": 2,
      "balance": 148636,
      "remarks": "收取资金费用"
    }
  ],
  "count": 7
}
```

#### 获取合约挂单冻结数量

请求方式及地址：GET`/swap_api/accounts/holds`

请求参数：

| 字段          | 类型   | 是否必选 | 说明                                      |
| ------------- | ------ | -------- | ----------------------------------------- |
| token         | string | Y        | 登录之后获取的验证token                   |
| instrument_id | string | Y        | 合约名称，如`BTC-USDT-SWAP,BTC-USDT-SWAP` |

返回结果及code状态码说明：



| code值 | 说明     |
| ------ | -------- |
| 1      | 成功     |
| 101    | 获取失败 |

返回示例：

```json
{
    "success": true,
    "user_id": 2201,
    "data": {
        "margin_frozen": "1000.00000000",//开仓冻结保证金
        "instrument_id": "BTC-USDT-SWAP" //合约名称
    }
}

//错误  （合约名称不存在时返回）
{
    "success": false,
    "code": 101,
    "msg": "no instrument_id"
}
```
