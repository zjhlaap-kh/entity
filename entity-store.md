## 变量
| 变量 | 值 |
| --- | --- |
| protocol | https |
| eshost | entitystore.dev.coherent.com.hk (dev还是staging根据环境来，不一定) |
| relam | 商户(如connect-saas) |
| privacyLevel | [privacyLevel的三个值](#privacyLevel的三个值，达标三种不同的权限) |
| folderPath | 文件夹路径 |
### privacyLevel的三个值，达标三种不同的权限

| 值 | 含义 |
| --- | --- |
| Global | 同商户下所有人都可以访问 |
| Tenant | 组织内可用，同一个公司 |
| Private | 私有，只有自己可用(但是现在好像同一个公司下的所有员工都可用) |

### 目录
### [文件夹](#文件夹的相关操作)
- [创建文件夹](#创建文件夹)
- [删除文件夹](#删除文件夹)
- [修改文件夹权限](#修改文件夹权限)
- [获取文件夹下所有子文件](#获取文件夹下的所有子文件)
- [通过文件夹名获取文件夹相关信息](#通过文件夹名获取文件夹相关信息)

### [文件的操作](#文件的操作)

- [添加文件](#添加文件)
- [删除文件](#删除文件)
- [修改文件](#修改文件)
- [查找文件](#查找文件)

## 文件夹的相关操作

| 变量 | 含义 |
| --- | --- |
| folderId | 文件夹的id |

### 创建文件夹
``` javascript
{protocol}://{eshost}/{relam}/docstore/api/v1/folders
Method: Post
content-type: form-data
参数: data
data的内容：
{
    "privacyLevel": "Tenant", // 必须
    "dataJson": { // 必须，但是可以不填任何东西
        "name": "Tintin in America",
        "characters": [
            {
                "name": "Snowy",
                "color": "shiro"
            },
            {
                "name": "AlCapone",
                "color": "red"
            }
        ]
    },
    "path":"/test/test1234", // 必须，文件夹的路径，不允许重复(推荐：/Root/Apps/*****)
    "effectiveStartDate": , // 文件夹什么时候可以开始使用(可选)
    "effectiveEndDate": , // 文件夹什么时候不可以使用(可选)
}
// 成功会返回文件夹的id，
```

### 删除文件夹
``` javascript
 {protocol}://{eshost}/{relam}/docstore/api/v1/folders/{folderId}
 Method: Delete
```

### 修改文件夹权限

``` javascript
{protocol}://{eshost}/{relam}/docstore/api/v1/perm
Method: PUT
content-type: application/json
data: {
    "folderId": {folderId},
    "principal": "user:pf",
    "action": read / update / delete, // 每种权限只能单独修改，假如想要三种权限就需要发三个请求，
    "value": true,
}   
```

### 获取文件夹下的所有子文件

``` javascript
{protocol}://{eshost}/{relam}/docstore/api/v2/folders/child/docs
Method: Get
Query: {
    query?： 类似sql语句
    __sort?: 排序方式，默认是按照创建时间倒序排列，例子：倒叙:-created,正序: created
    __size?: 每页多少条，最大不能超过20，不然会报错，默认20
    __page?: 获取第几页的数据，默认为1
    tagsQuery?: 通过tags去搜索，语法 tags like "%,amit,%" and tags like "%,home,%",tags的值需要是`,{value},`
    inheritedSearch?: 默认值：true。 如果为false，则将仅在搜索结果中返回用户具有读取权限的子实体。 设置为true时，用户只需要在fpath参数所引用的文件夹上具有读取权限即可
}
query参数需要用转义
其他的参数可以直接拼接到网址后面
tagsQuery参数为空的时候不要传，不然会报错
import { stringifyUrl } from 'query-string';
stringifyUrl({
    url: 'domain',
    query: '查询条件'
});

** 获取列表的时候会返回一个name字段，但是获取详情的时候找不到这个字段
```

### 通过文件夹名获取文件夹相关信息
``` javascript
{protocol}://{eshost}/{relam}/docstore/api/v1/folders/path
Method: Get
querys : {
    path: 文件夹的名字
}
```

## 文件的操作

| 变量 | 值 |
| --- | --- |
| documentId | 文件id |

### 添加文件
``` javascript
{protocol}://{eshost}/{relam}/docstore/api/v1/documents
Method: Post
content-type: form-data
参数： data，file(可选， 上传的文件)
data内容： {
    "privacyLevel": {privacyLevel},
    "dateTime1"?: 时间格式,
    "dateTime2"?: 时间格式,
    "status1"?: 数字,
    "status2"?: "status2",
    "email1"?: 字符串,
    "email2"?: 字符串,
    "amount1"?: "10.1",
    "amount2"?: 数字类型,
    "amount3"?: 12.1,
    "Type"?: "Type",
    "policyNumber"?: "abc123",
    "name1"?: "name1",
    "name2"?: "name2",
    "otherString1"?: "otherString1",
    "otherString2"?: "otherString2",
    "referenceId"?: "referenceId",
    "disposition"?: "disposition",
    "productName"?: "productName",
    "tags"?: ",amit,home,cars,{{$randomCity}},", // 传标签的时候需要的格式就是这样，前后都要有逗号
    // dataJson是必须的参数,但是每次操作都需要全部拿全部存
    "dataJson": {
        "name": "Tintin in America",
        "characters": [
            {
                "name": "Snowy",
                "color": "shiro"
            },
            {
                "name": "AlCapone",
                "color": "red"
            }
        ]
    },
    "path": "{folderPath}/文件名", // 不可重复，不可修改
    // 下面两个除非是需要一个一定时间内有效的文件，否则最好忽略这两个字段
    "effectiveStartDate"?: "2000-05-13T12:24:43.0873Z",
    "effectiveEndDate"?: "2025-05-13T12:24:43.0873Z"
}
```
### 删除文件
``` javascript
{protocol}://{eshost}/{relam}/docstore/api/v1/documents/{documentId}
Method: Delete
```
### 修改文件

``` javascript
{protocol}://{eshost}/{relam}/docstore/api/v1/documents/{documentId}
Method: Put
参数、content-type和添加的时候一样
注意：dataJson的值和其他的不一样，其他的参数在你修改的时候不去传递参数不会导致这个参数的值消失，但是dataJson会
如：productName添加的时候给的值是 productName1,修改的时候你没有传，等你去查询这个信息的时候，productName的值还是productName1,
但是如果是dataJson里面的值，修改时忘记把name字段在传回去，就会导致你下次去查看的时候会发现name字段找不到了
所有在修改的时候，对dataJson字段的值需要做到全部取全部存
```

### 查找文件

``` javascript
{protocol}://{eshost}/{relam}/docstore/api/v1/documents/{documentId}
Method: Get
querys: {
    ttl?: 以分钟为单位的文档下载网址的生存时间。 默认为6分钟
}

** 获取列表的时候会返回一个name字段，但是获取详情的时候找不到这个字段
```
