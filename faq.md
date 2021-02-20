# FAQ

1. **在哪个回调里提示用户请刷卡或插入芯片卡？**

   答：onRequestWaitingUser\(\)

2. **刷卡，插卡或贴卡后，会在哪个回调里返回结果**

   答：在onDotradeResult\(\)中返回，且返回结果可以根据DoTradeResult的枚举值进行判断

   | DoTradeResult枚举值 | 解释 |
   | :--- | :---: |
   | MCR | 磁条卡交易 |
   | ICC | 芯片卡交易 |
   | NONE | 刷卡或插卡已超时 |
   | NOT\_ICC | 不是正确的ICC卡 |
   | BAD\_SWIPE | 刷卡不良，请重新刷卡 |
   | NO\_RESPONSE | 刷卡或插卡不正常 |
   | NO\_UPDATE\_WORK\_KEY | 没有更新工作密钥 |
   | NFC\_ONLINE | NFC线上交易 |
   | NFC\_OFFLINE | NFC线下交易 |
   | NFC\_DECLINED | NFC交易拒绝 |
   | TRY\_ANOTHER\_INTERFACE | 交易降级，请尝试其他接口 |
   | CARD\_NOT\_SUPPORT | 卡片不支持 |
   | PLS\_SEE\_PHONE | 做CDCVM手机设备需要see phone指纹验证 |

3. **芯片卡交易的数据在哪个回调里获取？返回的数据格式是什么？**

   答：在onRequestOnlineProcess\(String tlv\)中获取芯片卡交易数据。返回的TLV数据格式，我们可以用TLVParser.parse方法解析各个tag的数据。

4. **收到银行返回的芯片卡交易结果后，需要怎么处理？**

   答：收到银行返回的交易结果后，需要根据银行返回的结果判定此交易是否被允许，如果允许，则发送指令pos.sendOnlineProcessResult\("8A023030"+"55域数据"\);不允许，则发送指令pos.sendOnlineProcessResult\("8A023035"+"55域数据"\)。

5. **在哪个函数中设置金额和交易类型？**

   答：一般在onRequestSetAmount回调中，通过接口pos.setAmount来设置金额和交易类型。其中amount交易金额是以分为单位，即：amount=1，表示0.01元，最高可传入12位数

   最常用的交易类型为货物交易，查询余额。当交易类型为查询余额时，金额输入参数要置为空

6. **如何对pos设备复位？**

   答：调用resetPosStatus\(\)方法

7. **如何选择应用APP？**

   答：在回调onRequestSelectEmvApp\(ArrayList appList\)中去获取得到应用列表。如果卡片只有一个应用，那么不会走到该回调里。

8. **onError方法的作用和其中包含的枚举值**

   答：作用：是可以根据当前交易中出现的问题提示客户

   | Error枚举值 | 解释 |
   | :--- | :---: |
   | TIMEOUT | 命令超时，未返回 |
   | MAC\_ERROR | mac地址计算错误 |
   | CMD\_TIMEOUT | 操作超时 |
   | CMD\_NOT\_AVAILABLE | 命令不可用 |
   | DEVICE\_RESET | 设备已重置 |
   | UNKNOWN | 未知错误 |
   | DEVICE\_BUSY | 设备忙碌 |
   | INPUT\_OUT\_OF\_RANGE | 超出范围的输入 |
   | INPUT\_INVALID\_FORMAT | 输入格式无效 |
   | INPUT\_ZERO\_VALUES | 输入是0值 |
   | INPUT\_INVALID | 输入值无效 |
   | CASHBACK\_NOT\_SUPPORTED | 不支持提款 |
   | CRC\_ERROR | CRC错误 |
   | COMM\_ERROR | 通信错误 |
   | WR\_DATA\_ERROR | 数据写入错误 |
   | EMV\_APP\_CFG\_ERROR | EMV APP配置错误 |
   | EMV\_CAPK\_CFG\_ERROR | EMV CAPK配置错误 |
   | APDU\_ERROR | APDU指令错误 |
   | APP\_SELECT\_TIMEOUT | app选择超时 |
   | ICC\_ONLINE\_TIMEOUT | icc线上交易超时 |
   | AMOUNT\_OUT\_OF\_LIMIT | 交易金额超出限制 |

9. **如何判断蓝牙连接状态？**

   答：通过函数getBluetoothState\(\)获取蓝牙连接状态

10. **Android sdk和pos机设备连接一般有几种方式？**

    答：有蓝牙，otg和串口连接。在连接时要选择适当的开启方式，调用CommunicationMode枚举值。

11. **如何开始扫描设备，扫描时接口在主线程还是在子线程显示？**

    答：调用接口scanQPos2mode方法去开始扫描，使用该接口时要在主线程调用，因为该接口底层是使用Handler去定时扫描的。

