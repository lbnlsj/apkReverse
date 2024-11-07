电脑访问接口，需要和手机在【同一网络环境下】，并把127.0.0.1换成手机的【局域网ip】

注意:所有接口统一返回值 {"code":code,"message":message,"data":data}

code{int} 0代表成功 1代表失败

message{string} 代表具体消息（错误或错误提示）

data{object} 为实体类对象 

系统参数过滤wg.cust.hook.accessibility (true关闭or false开启无障碍服务)
# 建议流程：
**设置应用权限等 - 改机 - 连接VPN(可选) - 定位(可选) - 上传附加信息(可选) - 应用操作 - 备份 - ...... - 还原**

## 一、设置应用权限等接口（接口操作会覆盖手动的设备设置）


### ①设置改机修改的网络类型(用于微狗助手2308及以后的版本)

#### http://127.0.0.1:1990/selectNetWork?type=2

- type`{数字}` （type=0=4G,1=WiFi,2=随机）

### ②无障碍服务设置

wg.cust.AUTO_SCRIPT_PKG_NAME 这个属性设置为无障碍应用的包名
例如com.autoJS.js，多个包名用英文逗号连接，设置后hook无障碍服务

### ③设置/删除sd白名单

设置白名单

#### http://127.0.0.1:1990/setWhiteList?path=/storage/emulated/0/文件夹名称/
  
删除sd白名单

#### http://127.0.0.1:1990/deleteWhitePath?path=/storage/emulated/0/文件夹名称/

- path`{文本}`
  sd卡路径列如`/storage/emulated/0/`（此白名单为根目录白名单，多个路径使用逗号分隔
  ）

### ④设置修改定位的app

#### http://127.0.0.1:1990/setReplaceLocationApp?packageNames=com.sina.weibo

packageNames`{文本}`设置定位的app包名,多个app包名之间用英文逗号隔开（当该参数为`null`时清除定位）

### ⑤隐藏(屏蔽)应用接口
对微博隐藏百度地图

#### http://127.0.0.1:1990/hideApp?srcPackageName=com.sina.weibo&destPackageNames=com.baidu.BaiduMap


- srcPackageName`{文本}`对谁隐藏app的包名
- destPackageNames`{文本}`隐藏的app包名（多个app包名之间用英文逗号隔开）

### ⑥设置屏幕亮度

#### http://127.0.0.1:1990/setSysScreenBrightness?value=200(亮度值)

- value`{数字}` 屏幕亮度的值区间（0-255）


### ⑦设置是否修改分辨率和是否修改sdk

#### http://127.0.0.1:1990/setSDKandWindow?isChangeSDK=1&isChangeWindow=1
isChangeSDK`{1 or 2}`1开启、2关闭修改sdk 

isChangeWindow`{1 or 2}`1开启、2关闭修改分辨率

### ⑧执行cmd  shell命令接口

#### http://127.0.0.1:1990/cmdexe?cmd=`ls /storage/emulated/0/`(查询目录文件)

- cmd`{文本}`命令字符列如`ls /storage/emulated/0/`

### ⑨设置/取消应用的root权限

设置应用的root权限

#### http://127.0.0.1:1990/setRootApp?packageName=com.sina.weibo

取消应用的root权限

#### http://127.0.0.1:1990/removerSu?pakNames=com.sina.weibo

- packageName`{文本}`（多个app包名之间用英文逗号隔开）

### ⑩是否修改gpu

#### http://127.0.0.1:1990/isChangeGpu?isChangeGpu=1(修改gpu)

- isChangeGpu `{1 or 2}`1为修改gpu 2为不修改


### ⑪设置vpn如果自动断开就立马断开wifi，防止获取到真实ip网络

#### http://127.0.0.1:1990/shouldVpnNetWork?shouldVpnNetWork=true(断开)

- shouldVpnNetWork`{true or false}` true为断开 false为不断开


### ⑫设置vpn连接超时时间

#### http://127.0.0.1:1990/setVPNConnectTimeOut?timeout=30(超时秒数)

- timeout`{数字}`区间10-30

### ⑬设置vpn应用白名单

#### http://127.0.0.1:1990/setVpnWhiteList?packageNames=com.sina.weibo(包名)

- packageNames`{文本}` 应用包名多个包名使用英文逗号隔开

### ⑭是否开启修改通讯录

#### http://127.0.0.1:1990/isChangeContact?isChangeContact=true
- isChangeContact`{true or false }`

- false不开启模拟通讯录

- true 为开启

### ⑮模拟应用列表开关

#### http://127.0.0.1:1990/setChangeAppList?isOpen=true(开启应用模拟)

- isOpen`{true or false}`
  **注：开启过本接口后，不管模拟应用列表是否开启 ，请使用 adb install -r
  xxxx.apk 强制安装方式安装，如果使用 adb install xxxx.apk
  可能会出现安装不上的情况**

## 二、VPN相关接口

### VPN接口

#### http://127.0.0.1:1990/vpnCtrl

获取VPN状态
isConn {number} 1表示连接，返回：连接成功？连接失败？0表示断开连接 2 获取vpn状态
返回：断开成功？断开失败？注：此接口代表连接之前配置好的vpn ip {string}
vpn地址 name {string} vpn账号 password {string} vpn密码 mppe {boolean}
是否启用ppp加密 可不填 ，默认加密

## 三、定位相关接口

### 根据城市定位
#### http://127.0.0.1:1990/myLocation?city=北京市
### 根据经纬度定位（不建议使用）
#### http://127.0.0.1:1990/myLocation?lat=121.47&lon=31.23
### 根据当前ip定位
#### http://127.0.0.1:1990/myLocation
### 根据国家(非中国)或者地区定位
#### http://127.0.0.1:1990/myLocation?country=台湾省花莲市
#### http://127.0.0.1:1990/myLocation?country=美国
以下参数不能同时设置

- 无参数时默认当前城市范围随机地址
- city{string} 城市（只输入城市名或者省份 如 北京市 西藏省）
- lat=30.2017984148&lon=121.1897295713（传入经纬度准确定位） 
## 四、改机相关接口
### ①手机号改机/还原接口
#### http://127.0.0.1:1990/wx?survival=false&phoneNumber=?&device=?&sdk=?isOpenStoragepermissions=ture
- id `{数字}` 如果使用还原则使用该字段
- androidId `{数字}` 如果使用还原则使用该字段
- survival `{true or false}`true为还原，false为新机
- phoneNumber `{数字}`新机时用：大写的两位ISO国家码+手机号，如中国移动CN15888888888
- destPackageInfos `{文本}` 改机目标应用的包名
- device `{文本}` 新机时用：指定改机后的手机型号（可选）
- sdk`{数字}`新机时用：指定改机后的sdk版本改机（可选）
- isOpenStoragepermissions `{true or fasle}` 新机时用：默认为true，打开应用的sd卡读写权限
###  ②CPA改机/还原接口
#### http://127.0.0.1:1990/getDeviceInfoFromServer?destPackageInfos=?&packageName=?&chanel=?&survival=?

- destPackageInfos`{文本}`要改机的应用包名
- packageName `{文本}`后台设置的包名
- chanel `{文本}` 后台设置的渠道号
- sOpenStoragepermissions`{true or false}` 默认打开应用的sd卡读写权限
- survival `{true or false}` 是否留存 （只能还原昨天以前的备份信息）

### ③上传附加信息

#### http://127.0.0.1:1990/uploadAddInfo?deviceInfoId=(改机后返回的设备唯一id)&androidId=(改机后返回的androidId)&account_password=(上传的文字附件信息)

- deviceInfoId`{文本}`改机后返回的设备唯一id
- androidId`{文本}`改机后返回的androidId
- account_password`{文本}`上传的文字附件信息
- uploadFilePath`{文本}` 上传的文件路径（可选）

## 五、全息备份相关接口
### 全息备份接口

#### http://127.0.0.1:1990/uploadBackup?id=(设备信息id)&androidId=(设备androidId)&packageName=(要备份的包名)

- id `{文本}` 设备信息id
- androidId `{文本}` 设备信息
- packageName `{文本}` 要备份的包名

## 六、其他接口
### 清理sd卡所有数据

#### http://127.0.0.1:1990/clearSd
### 对应用请求读写、电话 等权限【注意：注：perssion android权限请自己查阅】
#### http://127.0.0.1:1990/requestAppPerssion?packageName=？&perssion=android.permission.READ_EXTERNAL_STORAGE
（packageName=请求权限的app包名，多个包名用英文逗号连接）
- packageName `{文本}` 请求权限的app包名，多个包名用英文逗号连接
- perssion `{文本}` 权限名称如读sd卡权限android.permission.READ_EXTERNAL_STORAGE

### 刷新手机接口

#### http://127.0.0.1:1990/reFreshPhone

**当设备验证失败时调用**


### 上传手机本地文件到ftp服务器

#### http://127.0.0.1:1990/uploadfile

- username`{文本}`ftp用户名
- password`{文本}`ftp密码
- url`{文本}` 文件路径
- host`{文本}`上传到服务器的地址
- port`{数字}`服务端端口

### 获取备份时间

#### http://127.0.0.1:1990/getBackUptime

### 刷新媒体文件

#### http://127.0.0.1:1990/updataMediaStore

- url`{文本}`文件绝对路径

### 修改系统时区

#### http://127.0.0.1:1990/setTimeZone?timeZone=Asia/Shanghai(亚洲上海时区)

timeZone`{文本}` 时区名称列如`Asia/Shanghai`


### 获取部分机型信息
#### http://127.0.0.1:1990/getDeviceList

### 脚本掉无障碍，重新唤醒脚本
#### http://127.0.0.1:1990/onAccessibilityService?packageName=脚本包名


### 设置usb共享网络
#### setUsbTethering
- isusb`{true or false }` 是否开启
