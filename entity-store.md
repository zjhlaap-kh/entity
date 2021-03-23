## 变量
| 变量 | 值 |
| --- | --- |
| protocol | https |
| eshost | entitystore.dev.coherent.com.hk (dev还是staging根据环境来，不一定) |
| relam | 商户(如connect-saas) |

### privacyLevel的三个值，达标三种不同的权限

| 值 | 含义 |
| --- | --- |
| Global | 同商户下所有人都可以访问 |
| Tenant | 组织内可用，同一个公司 |
| Private | 私有，只有自己可用(但是现在好像同一个公司下的所有员工都可用) |

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
    "path":"/test/test1234", // 必须，文件夹的路径(推荐：/Root/Apps/*****)
    "effectiveStartDate": , // 文件夹什么时候可以开始使用(可选)
    "effectiveEndDate": , // 文件夹什么时候不可以使用(可选)
}
```
