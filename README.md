# ThreeView开发文档

### 名词定义
- 普通模型（General Model）：区别于场景，一个普通模型场景只有一个模型。
- 场景（Scene）：场景由一个或多个品类组成，如衬衫、裤子等组成一个场景。
- 产品（Product）: 产品由一个或多个组合组成，如圆摆衬衫可由口袋、大身两个组合组成。
- 组合（Combination）：组合由一个或多个部件组成，如圆摆衬衫大身（组合）由袖口裥、门襟、后片、领型、袖口等组成。
备注：每个组合可关联多种材料。
- 部件（Part）：部件包括一个或多个基本模型，如圆摆衬衫领型包括小方领、小尖领、一字领、立领、中标领等。
- 基本模型（Basic Model）：基本模型由一个模型和（零个或多个）固定模型组成，如衬衫领型可由领子模型，扣子、扣眼、缝线等固定模型组成。
- 固定模型 (Fixed Model) ：如衬衫领型的扣子、扣眼、缝线等可以作为固定模型。
- 面料（Fabric）：贴在模型上的贴图。



## 后台接口

为了保证安全，以下接口需要通过后端接入，然后通过你们自行设计的接口将数据传入到前端，并把数据给到3D前端SDK。

### 获取access token

URL：`/api/getAccessToken`
Method：`GET`

queryData:
  accessKey: 从API申请页面拿到的accessKey,
  accessSecret: 从API申请页面拿到的accessSecret

Return: 

```
{
	// 返回状态码， 0为成功，其他为失败。
	code: number,

	// 错误信息
	message: string,
	
	// token
	data: string
}
```




### 私有搭配列表

URL：`/api/scene/list`
Method：`GET`

header: {

  AccessToken : `${之前传入的token}`

}

Return: 

```json
{
    // 返回状态码， 0为成功，其他为失败。
    code: number,
    // 返回数据，数组，包含了分类信息
    data: [{
        id: number,
        name: string,
        cover: string,
        remark: string
    }]
}
```

### 私有模型列表

URL：`/api/generalModel/list`
Method：`GET`

header: {

  AccessToken : `${之前传入的token}`

}

Return: 

```json
{
    // 返回状态码， 0为成功，其他为失败。
    code: number,
    // 返回数据，数组，包含了分类信息
    data: [{
        id: number,
        name: string,
				cover: string
    }]
}
```


### 指定id的搭配

URL：`/api/scene/{id}`
Method：`GET`

header: {

AccessToken : `${拿到的token}`

}

Return: 

```json
{
  // 返回状态码， 0为成功，其他为失败。
  code: Number,
  // 返回数据，数组，包含了分类信息
  data: {
    "id": number,
    "name": string,
    "remark": string,
    "cover": string,
    "argument": string,
    "products": [{
      "id": number,
      "name": string,
      "cover": string,
      "combinations": [{
        "id": 9,
        "name": "123平下摆",
        "cover": "",
        "parts": [{
          "id": 22,
          "name": "",
          "cover": "",
          "elements": [{
            "id": 16,
            "name": "",
            "cover": "",
            "model": "",
            "fixedModels": [{
              "id": 13,
              "name": "",
              "cover": "",
              "model": "",
              "material": "",
              "createdAt": "",
              "updatedAt": "",
            }],
          }],

          "fabrics": [{
            "id": 9,
            "name": "",
            "cover": "",
            "assets": string,
            "config": string
          }]
        }]
      }]
    }],
    "fixedModels": [{
      "id": 3,
      "name": String,
      "cover": String,
      "model": String,
      "material": Number
    }]
  }
}
```



### 指定id的模型

URL：`/api/scene/:sceneId`
Method：`GET`

header: {

AccessToken : `${拿到的token}`

}

Return: 

```
{
  // 返回状态码， 0为成功，其他为失败。
  code: Number,
  // 返回数据，数组，包含了分类信息
  data: {
    "id": number,
    "name": string,
    "remark": string,
    "cover": string,
    "argument": string,
    ”model“: string,
    "material": string
  }
}
```



## 3D场景插件

该3D插件主要使用来渲染一个私有的3D场景，使用方式如下。

### 引入方式
<script src="js/threed-real-viewer.js"></script>
### 初始化

```
var viewer = new ThreedRealViewer(div: HTMLDivElement, type: type: 'GENERAL' | 'COLLOCATION', data: object, finishLoading: () => void);

参数解析:
    div: div HTML元素.
    type: 类型，请传 'GENERAL' 或者 'COLLOCATION'.
    data: 从后台拿到的对应的3D场景数据.
    finishLoading: 完成加载回调.
```

### 切换产品

```
viewer.selectProduct(productIndex: number);

参数解析:
	productIndex: 当前产品对应的下标.
	返回值: undefined

说明:
  用于动画切换，如果没有触发动画，请确认后台是否有设置动画操作。

```

### 切换组合

```
viewer.selectCombination(productIndex: number, combIndex: number);

参数解析:
	productIndex: 当前产品对应的下标.
	combIndex: 当前组合的下标.
	返回值: undefined

说明:
  用于动画切换，如果没有触发动画，请确认后台是否有设置动画操作。
```

### 切换组件

```
viewer.selectElement(productIndex: number, combIndex: number, pIndex: number, eIndex: number);

参数解析:
	productIndex: 当前产品对应的下标.
	combIndex: 当前组合的下标.
	pIndex: 当前部件的下标.
	eIndex: 需要切换到的组件的下标.
	返回值: Promise
```

### 切换面料

```
viewer.selectFabric (productIndex: number, combIndex: number, targetFIndex: number);

参数解析:
	productIndex: 当前产品对应的下标.
	combIndex: 当前组合的下标.
	targetFIndex: 需要切换到的面料的下标
  返回值: Promise
```
