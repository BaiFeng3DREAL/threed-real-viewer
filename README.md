# ThreeView开发文档

### 名词定义
- 普通模型（General Model）：与模型中心“我的模型”中的模型一一对应。
- 场景（Scene）：与模型中心“我的搭配”中的场景一一对应。场景由一个或多个产品构成，如衬衫、裤子等构成一个场景。
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




## 3D Real 开发者工具

3D Real 开发者工具，可以帮助您开发自己的三维产品展示页面，提供了高度自定义的接口。使用方式如下： 

### 引入方式
<script src="js/threed-real-sdk-1.0.0.js"></script>
### 初始化

```
var api = new ThreeDRealAPI(token: string);

参数解析:
    token: 使用 AccessKey 和 AccessSecret 获取的token。
```

### 获取私有场景

获取私有搭配列表
```
api.fetchScenes()

参数解析:
  返回值: Promise<{
    id: number,
    name: string,
    cover: string
  }[]>
```

获取私有模型列表
```
api.fetchScenes()

参数解析:
  返回值: Promise<{
    id: number,
    name: string,
    cover: string
  }[]>

```

### 获取 远端数据 以及 三维场景交互控制器

获取搭配数据，以及三维场景交互控制器。

```
api.getCollocationView();

参数解析:
  返回值: Promise<{
    data: object（如何使用，请看 demo 中的例子）,
    threedViewer: ThreeDRealViewer
  }[]>
```

获取模型数据，以及三维场景交互控制器。

```
api.getModelView(div: HTMLDivElement, id: number, finish?: () => void);
参数解析:
  返回值: Promise<{
    data: object（如何使用，请看 demo 中的例子）,
    threedViewer: ThreeDRealViewer
  }[]>
```


通过上面的接口获取三维视图控制器，对于搭配来说，我们可以通过调用以下接口触发模型的变换：
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

### 销毁 三维视图控制器
```
  viewer.destroy()
```
