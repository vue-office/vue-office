# vue-office

[中文](#中文) | [English](#english) | [日本語](#日本語) 


<a id="english"></a>
## English

A Vue component library supporting preview of multiple file types (**docx, excel, pdf, pptx**), compatible with Vue 2/3. Also supports preview in non-Vue frameworks.

[Demo](https://501351981.github.io/vue-office/examples/dist/)

[Using non-Vue frameworks (vanilla JS, React, etc.) or troubleshooting Vue errors](https://501351981.github.io/vue-office/examples/docs/guide/js-preview.html)

### Features
- All-in-one: Provides online preview solutions for various document types including Word (.docx), PDF, Excel (.xlsx, .xls), and PowerPoint (.pptx)
- Simple: Just provide the document's src (URL) to complete the document preview
- Great experience: Selects the best preview solution for each document type, ensuring optimal user experience and performance
- High performance: Optimized for large data volumes

### Installation
```shell
# docx document preview component
npm install @vue-office/docx vue-demi@0.14.6

# excel document preview component
npm install @vue-office/excel vue-demi@0.14.6

# pdf document preview component
npm install @vue-office/pdf vue-demi@0.14.6

# pptx document preview component
npm install @vue-office/pptx vue-demi@0.14.6
```
For Vue 2.6 or below, you also need to install @vue/composition-api
```shell
npm install @vue/composition-api
```

### Usage Examples

Document preview scenarios can be roughly divided into three types:
- Using a document CDN URL, such as https://***.docx, passing the document URL string to the component's src property
- Requesting file content through an API, where you can get the file's ArrayBuffer or Blob format data and pass it to the component's src property
- Previewing during file upload, where you can get the file's ArrayBuffer or Blob format data and pass it to the component's src property

<details>
<summary>docx file preview examples (examples for all three usage methods)</summary>

**1. Using a network URL for preview**
```vue
<template>
    <vue-office-docx
        :src="docx"
        style="height: 100vh;"
        @rendered="rendered"
    />
</template>

<script>
// Import VueOfficeDocx component
import VueOfficeDocx from '@vue-office/docx'
// Import related styles
import '@vue-office/docx/lib/index.css'

export default {
    components:{
        VueOfficeDocx
    },
    data(){
        return {
            docx: 'http://static.shanhuxueyuan.com/test6.docx' // Set document network address, can be relative
        }
    },
    methods:{
        rendered(){
            console.log("Rendering completed")
        }
    }
}
</script>
```

**2. Upload file preview**

Reading the file's ArrayBuffer
```vue
<template>
    <div>
        <input type="file" @change="changeHandle"/>
        <vue-office-docx :src="src"/>
    </div>
</template>

<script>
import VueOfficeDocx from '@vue-office/docx'
import '@vue-office/docx/lib/index.css'

export default {
    components: {
        VueOfficeDocx
    },
    data(){
        return {
            src: ''
        }
    },
    methods:{
        changeHandle(event){
            let file = event.target.files[0]
            let fileReader = new FileReader()
            fileReader.readAsArrayBuffer(file)
            fileReader.onload =  () => {
                this.src = fileReader.result
            }
        }
    }
}
</script>
```

**3. Binary file preview**

If the backend provides a POST API that returns a binary stream instead of a CDN URL, you can call the API to get the file's ArrayBuffer data and pass it to the src property.

```vue
<template>
    <vue-office-docx
        :src="docx"
        style="height: 100vh;"
        @rendered="rendered"
    />
</template>

<script>
// Import VueOfficeDocx component
import VueOfficeDocx from '@vue-office/docx'
// Import related styles
import '@vue-office/docx/lib/index.css'

export default {
    components:{
        VueOfficeDocx
    },
    data(){
        return {
            docx: ''
        }
    },
    mounted(){
        fetch('your API file address', {
            method: 'post'
        }).then(res=>{
            // Read the file's arrayBuffer
            res.arrayBuffer().then(res=>{
                this.docx = res
            })
        })
    },
    methods:{
        rendered(){
            console.log("Rendering completed")
        }
    }
}
</script>
```

</details>

<details>
<summary>excel file preview examples</summary>

The example of preview via network address is as follows. The preview via file ArrayBuffer is the same as the docx usage above.
```vue
<template>
    <vue-office-excel
        :src="excel"
        style="height: 100vh;"
        @rendered="renderedHandler"
        @error="errorHandler"
    />
</template>

<script>
// Import VueOfficeExcel component
import VueOfficeExcel from '@vue-office/excel'
// Import related styles
import '@vue-office/excel/lib/index.css'

export default {
    components: {
        VueOfficeExcel
    },
    data() {
        return {
            excel: 'http://static.shanhuxueyuan.com/demo/excel.xlsx' // Set document address
        }
    },
    methods: {
        renderedHandler() {
            console.log("Rendering completed")
        },
        errorHandler() {
            console.log("Rendering failed")
        }
    }
}
</script>
```
</details>

<details>
<summary>pdf file preview examples</summary>

The example of preview via network address is as follows. The preview via file ArrayBuffer is the same as the docx usage above.
```vue
<template>
    <vue-office-pdf
        :src="pdf"
        style="height: 100vh"
        @rendered="renderedHandler"
        @error="errorHandler"
    />
</template>

<script>
// Import VueOfficePdf component
import VueOfficePdf from '@vue-office/pdf'

export default {
    components: {
        VueOfficePdf
    },
    data() {
        return {
            pdf: 'http://static.shanhuxueyuan.com/test.pdf' // Set document address
        }
    },
    methods: {
        renderedHandler() {
            console.log("Rendering completed")
        },
        errorHandler() {
            console.log("Rendering failed")
        }
    }
}
</script>
```

</details>

<details>
<summary>pptx file preview examples</summary>

The example of preview via network address is as follows. The preview via file ArrayBuffer is the same as the docx usage above.
```vue
<template>
    <vue-office-pptx
        :src="pdf"
        style="height: 100vh"
        @rendered="renderedHandler"
        @error="errorHandler"
    />
</template>

<script>
import VueOfficePptx from '@vue-office/pptx'

export default {
    components: {
        VueOfficePptx
    },
    data() {
        return {
            pdf: 'http://****/test.pptx' // Set document address
        }
    },
    methods: {
        renderedHandler() {
            console.log("Rendering completed")
        },
        errorHandler() {
            console.log("Rendering failed")
        }
    }
}
</script>
```

</details>

### Third-party Libraries Dependencies

- docx: Implemented based on the docx-preview library, related issues are not currently being addressed
- pdf: Implemented based on the pdfjs library, with virtual list implementation for improved performance
- excel: Implemented based on exceljs and x-data-spreadsheet, with better style support across the web
- pptx: Implemented based on the self-developed [pptx-preview](https://github.com/501351981/pptx-preview) library, source code available separately for a fee from the author

<a id="中文"></a>
## 中文

支持多种文件(**docx、excel、pdf、pptx**)预览的vue组件库，支持vue2/3。也支持非Vue框架的预览。

[《演示效果》](https://501351981.github.io/vue-office/examples/dist/)

[《使用非Vue框架（原生js、React等）、或者Vue里面报错，看这里》](https://501351981.github.io/vue-office/examples/docs/guide/js-preview.html)

### 功能特色
- 一站式：提供word(.docx)、pdf、excel(.xlsx, .xls)、ppt(.pptx)多种文档的在线预览方案，有它就够了
- 简单：只需提供文档的src(网络地址)即可完成文档预览
- 体验好：选择每个文档的最佳预览方案，保证用户体验和性能都达到最佳状态
- 性能好：针对数据量较大做了优化

### 安装
```shell
#docx文档预览组件
npm install @vue-office/docx vue-demi@0.14.6

#excel文档预览组件
npm install @vue-office/excel vue-demi@0.14.6

#pdf文档预览组件
npm install @vue-office/pdf vue-demi@0.14.6

#pptx文档预览组件
npm install @vue-office/pptx vue-demi@0.14.6
```
如果是vue2.6版本或以下还需要额外安装 @vue/composition-api
```shell
npm install @vue/composition-api
```

### 使用示例

文档预览场景大致可以分为三种：
- 有文档CDN地址，比如 https://***.docx，将文档地址字符串传给组件的src属性
- 通过接口请求获取文件内容，此时可以获取文件的ArrayBuffer或Blob格式数据传给组件的src属性
- 文件上传时预览，此时可以获取文件的ArrayBuffer或Blob格式数据传给组件的src属性

<details>
<summary>docx文件预览示例 （三种使用方式均有示例）</summary>

**1. 使用网络地址预览**
```vue
<template>
    <vue-office-docx
        :src="docx"
        style="height: 100vh;"
        @rendered="rendered"
    />
</template>

<script>
//引入VueOfficeDocx组件
import VueOfficeDocx from '@vue-office/docx'
//引入相关样式
import '@vue-office/docx/lib/index.css'

export default {
    components:{
        VueOfficeDocx
    },
    data(){
        return {
            docx: 'http://static.shanhuxueyuan.com/test6.docx' //设置文档网络地址，可以是相对地址
        }
    },
    methods:{
        rendered(){
            console.log("渲染完成")
        }
    }
}
</script>
```

**2. 上传文件预览**

读取文件的ArrayBuffer
```vue
<template>
    <div>
        <input type="file" @change="changeHandle"/>
        <vue-office-docx :src="src"/>
    </div>
</template>

<script>
import VueOfficeDocx from '@vue-office/docx'
import '@vue-office/docx/lib/index.css'

export default {
    components: {
        VueOfficeDocx
    },
    data(){
        return {
            src: ''
        }
    },
    methods:{
        changeHandle(event){
            let file = event.target.files[0]
            let fileReader = new FileReader()
            fileReader.readAsArrayBuffer(file)
            fileReader.onload =  () => {
                this.src = fileReader.result
            }
        }
    }
}
</script>
```

**3. 二进制文件预览**

如果后端给的不是CDN地址，而是一些POST接口，该接口返回二进制流，则可以调用接口获取文件的ArrayBuffer数据，传递给src属性。

```vue
<template>
    <vue-office-docx
        :src="docx"
        style="height: 100vh;"
        @rendered="rendered"
    />
</template>

<script>
//引入VueOfficeDocx组件
import VueOfficeDocx from '@vue-office/docx'
//引入相关样式
import '@vue-office/docx/lib/index.css'

export default {
    components:{
        VueOfficeDocx
    },
    data(){
        return {
            docx: ''
        }
    },
    mounted(){
        fetch('你的API文件地址', {
            method: 'post'
        }).then(res=>{
            //读取文件的arrayBuffer
            res.arrayBuffer().then(res=>{
                this.docx = res
            })
        })
    },
    methods:{
        rendered(){
            console.log("渲染完成")
        }
    }
}
</script>
```

</details>

<details>
<summary>excel 文件预览示例 </summary>

通过网络地址预览示例如下，通过文件ArrayBuffer预览和上面docx的使用方式一致。
```vue
<template>
    <vue-office-excel
        :src="excel"
        style="height: 100vh;"
        @rendered="renderedHandler"
        @error="errorHandler"
    />
</template>

<script>
//引入VueOfficeExcel组件
import VueOfficeExcel from '@vue-office/excel'
//引入相关样式
import '@vue-office/excel/lib/index.css'

export default {
    components: {
        VueOfficeExcel
    },
    data() {
        return {
            excel: 'http://static.shanhuxueyuan.com/demo/excel.xlsx'//设置文档地址
        }
    },
    methods: {
        renderedHandler() {
            console.log("渲染完成")
        },
        errorHandler() {
            console.log("渲染失败")
        }
    }
}
</script>
```
</details>

<details>
<summary>pdf 文件预览示例 </summary>

通过网络地址预览示例如下，通过文件ArrayBuffer预览和上面docx的使用方式一致。
```vue
<template>
    <vue-office-pdf
        :src="pdf"
        style="height: 100vh"
        @rendered="renderedHandler"
        @error="errorHandler"
    />
</template>

<script>
//引入VueOfficePdf组件
import VueOfficePdf from '@vue-office/pdf'

export default {
    components: {
        VueOfficePdf
    },
    data() {
        return {
            pdf: 'http://static.shanhuxueyuan.com/test.pdf' //设置文档地址
        }
    },
    methods: {
        renderedHandler() {
            console.log("渲染完成")
        },
        errorHandler() {
            console.log("渲染失败")
        }
    }
}
</script>
```

</details>


<details>
<summary>pptx 文件预览示例 </summary>

通过网络地址预览示例如下，通过文件ArrayBuffer预览和上面docx的使用方式一致。
```vue
<template>
    <vue-office-pptx
        :src="pdf"
        style="height: 100vh"
        @rendered="renderedHandler"
        @error="errorHandler"
    />
</template>

<script>
import VueOfficePptx from '@vue-office/pptx'

export default {
    components: {
        VueOfficePptx
    },
    data() {
        return {
            pdf: 'http://****/test.pptx' //设置文档地址
        }
    },
    methods: {
        renderedHandler() {
            console.log("渲染完成")
        },
        errorHandler() {
            console.log("渲染失败")
        }
    }
}
</script>
```

</details>

### 项目依赖的第三方库

- docx： 基于docx-preview库实现，相关issues暂不处理
- pdf： 基于pdfjs库实现，实现了虚拟列表增加性能
- excel: 基于exceljs 和 x-data-spreadsheet实现，全网样式支持更好
- pptx： 基于自研库 [pptx-preview](https://github.com/501351981/pptx-preview) 实现，源码单独付费向作者索取


# 支持作者

如果该项目帮到了您，节省了您宝贵的开发时间，还请您不吝给项目点个免费的赞。

当然了，如果您能请作者喝杯咖啡，哪怕喝瓶娃娃哈矿泉水，也是对作者最真诚的鼓励，打赏用户欢迎添加微信，后续交流前端相关问题。

![](https://501351981.github.io/vue-office/examples/dist/static/wx.png)

如果您有问题要咨询或者解决，可打赏咨询作者，自2024年12月起，可能不会及时处理issues内容，作者也要吃饭，也面临大龄程序员困境。

## 为什么没有开放源码（打赏50+送源码）

我们都知道，如果一件事情没有收益，那是很难长久的，特别是对于一个大龄程序员来说，花费大量的时间"用爱发电"对大家来说是非常值得尊敬的，而我感觉对家庭来说可能是不道德的，没有收益，没有正反馈，很难把这个库完善下去，我们也看到了，很多开源库已经多年没有更新了。为了后续开发出更好用的前端office文件预览库，本项目需要大家的支持！

源码需付费向作者索要（_**打赏50**+_），打赏用户（无论多少）均可添加作者微信，交流该库或者前端领域话题，源码不得用于开源（这也是关闭源码的原因之一，有些人直接复制开源作为自己项目）。

## 掘金小册

凝聚作者近10年工作经验的掘金小册[《如何写出高质量的前端代码》](https://juejin.cn/book/7351005935306801188) 已经上线啦，欢迎前端同学关注，希望能够提升大家的前端编码水平。

小册已售 890+份，收到前端同学的高度评价，对提升日常开发效率和质量，有非常大的帮助。



<a id="日本語"></a>
## 日本語

複数のファイル形式（**docx、excel、pdf、pptx**）のプレビューをサポートするVueコンポーネントライブラリで、Vue2/3に対応。Vue以外のフレームワークでのプレビューもサポートしています。

[デモ](https://501351981.github.io/vue-office/examples/dist/)

[Vue以外のフレームワーク（ネイティブJS、Reactなど）の使用、またはVueでのエラー対処法はこちら](https://501351981.github.io/vue-office/examples/docs/guide/js-preview.html)

### 特徴
- オールインワン：Word（.docx）、PDF、Excel（.xlsx、.xls）、PowerPoint（.pptx）など様々な文書タイプのオンラインプレビューソリューションを提供
- シンプル：文書のsrc（URL）を提供するだけで文書プレビューが完了
- 優れた体験：各文書タイプに最適なプレビューソリューションを選択し、最適なユーザー体験とパフォーマンスを保証
- 高性能：大量データに対して最適化済み

### インストール
```shell
# docx文書プレビューコンポーネント
npm install @vue-office/docx vue-demi@0.14.6

# excel文書プレビューコンポーネント
npm install @vue-office/excel vue-demi@0.14.6

# pdf文書プレビューコンポーネント
npm install @vue-office/pdf vue-demi@0.14.6

# pptx文書プレビューコンポーネント
npm install @vue-office/pptx vue-demi@0.14.6
```
Vue 2.6以下のバージョンを使用している場合は、@vue/composition-apiも追加でインストールする必要があります
```shell
npm install @vue/composition-api
```

### 使用例

文書プレビューのシナリオは大きく3つに分けられます：
- 文書のCDN URLを使用する場合（例：https://***.docx）、文書URLの文字列をコンポーネントのsrcプロパティに渡します
- APIを通じてファイルコンテンツをリクエストする場合、ファイルのArrayBufferまたはBlob形式のデータを取得してコンポーネントのsrcプロパティに渡します
- ファイルアップロード時にプレビューする場合、ファイルのArrayBufferまたはBlob形式のデータを取得してコンポーネントのsrcプロパティに渡します

<details>
<summary>docxファイルプレビューの例（3つの使用方法すべての例）</summary>

**1. ネットワークURLを使用したプレビュー**
```vue
<template>
    <vue-office-docx
        :src="docx"
        style="height: 100vh;"
        @rendered="rendered"
    />
</template>

<script>
// VueOfficeDocxコンポーネントをインポート
import VueOfficeDocx from '@vue-office/docx'
// 関連するスタイルをインポート
import '@vue-office/docx/lib/index.css'

export default {
    components:{
        VueOfficeDocx
    },
    data(){
        return {
            docx: 'http://static.shanhuxueyuan.com/test6.docx' // 文書のネットワークアドレスを設定、相対アドレス可
        }
    },
    methods:{
        rendered(){
            console.log("レンダリング完了")
        }
    }
}
</script>
```

**2. ファイルアップロードプレビュー**

ファイルのArrayBufferを読み込む
```vue
<template>
    <div>
        <input type="file" @change="changeHandle"/>
        <vue-office-docx :src="src"/>
    </div>
</template>

<script>
import VueOfficeDocx from '@vue-office/docx'
import '@vue-office/docx/lib/index.css'

export default {
    components: {
        VueOfficeDocx
    },
    data(){
        return {
            src: ''
        }
    },
    methods:{
        changeHandle(event){
            let file = event.target.files[0]
            let fileReader = new FileReader()
            fileReader.readAsArrayBuffer(file)
            fileReader.onload =  () => {
                this.src = fileReader.result
            }
        }
    }
}
</script>
```

**3. バイナリファイルプレビュー**

バックエンドがCDN URLではなくバイナリストリームを返すPOST APIを提供している場合、APIを呼び出してファイルのArrayBufferデータを取得し、srcプロパティに渡すことができます。

```vue
<template>
    <vue-office-docx
        :src="docx"
        style="height: 100vh;"
        @rendered="rendered"
    />
</template>

<script>
// VueOfficeDocxコンポーネントをインポート
import VueOfficeDocx from '@vue-office/docx'
// 関連するスタイルをインポート
import '@vue-office/docx/lib/index.css'

export default {
    components:{
        VueOfficeDocx
    },
    data(){
        return {
            docx: ''
        }
    },
    mounted(){
        fetch('あなたのAPIファイルアドレス', {
            method: 'post'
        }).then(res=>{
            // ファイルのarrayBufferを読み込む
            res.arrayBuffer().then(res=>{
                this.docx = res
            })
        })
    },
    methods:{
        rendered(){
            console.log("レンダリング完了")
        }
    }
}
</script>
```

</details>

<details>
<summary>excelファイルプレビューの例</summary>

ネットワークアドレスを介したプレビューの例は次のとおりです。ファイルArrayBufferを介したプレビューは、上記のdocxの使用方法と同じです。
```vue
<template>
    <vue-office-excel
        :src="excel"
        style="height: 100vh;"
        @rendered="renderedHandler"
        @error="errorHandler"
    />
</template>

<script>
// VueOfficeExcelコンポーネントをインポート
import VueOfficeExcel from '@vue-office/excel'
// 関連するスタイルをインポート
import '@vue-office/excel/lib/index.css'

export default {
    components: {
        VueOfficeExcel
    },
    data() {
        return {
            excel: 'http://static.shanhuxueyuan.com/demo/excel.xlsx' // 文書アドレスを設定
        }
    },
    methods: {
        renderedHandler() {
            console.log("レンダリング完了")
        },
        errorHandler() {
            console.log("レンダリング失敗")
        }
    }
}
</script>
```
</details>

<details>
<summary>pdfファイルプレビューの例</summary>

ネットワークアドレスを介したプレビューの例は次のとおりです。ファイルArrayBufferを介したプレビューは、上記のdocxの使用方法と同じです。
```vue
<template>
    <vue-office-pdf
        :src="pdf"
        style="height: 100vh"
        @rendered="renderedHandler"
        @error="errorHandler"
    />
</template>

<script>
// VueOfficePdfコンポーネントをインポート
import VueOfficePdf from '@vue-office/pdf'

export default {
    components: {
        VueOfficePdf
    },
    data() {
        return {
            pdf: 'http://static.shanhuxueyuan.com/test.pdf' // 文書アドレスを設定
        }
    },
    methods: {
        renderedHandler() {
            console.log("レンダリング完了")
        },
        errorHandler() {
            console.log("レンダリング失敗")
        }
    }
}
</script>
```

</details>

<details>
<summary>pptxファイルプレビューの例</summary>

ネットワークアドレスを介したプレビューの例は次のとおりです。ファイルArrayBufferを介したプレビューは、上記のdocxの使用方法と同じです。
```vue
<template>
    <vue-office-pptx
        :src="pdf"
        style="height: 100vh"
        @rendered="renderedHandler"
        @error="errorHandler"
    />
</template>

<script>
import VueOfficePptx from '@vue-office/pptx'

export default {
    components: {
        VueOfficePptx
    },
    data() {
        return {
            pdf: 'http://****/test.pptx' // 文書アドレスを設定
        }
    },
    methods: {
        renderedHandler() {
            console.log("レンダリング完了")
        },
        errorHandler() {
            console.log("レンダリング失敗")
        }
    }
}
</script>
```

</details>

### プロジェクトが依存するサードパーティライブラリ

- docx: docx-previewライブラリに基づいて実装、関連する問題は現在対応していません
- pdf: pdfjsライブラリに基づいて実装、パフォーマンス向上のための仮想リスト実装
- excel: exceljsとx-data-spreadsheetに基づいて実装、ウェブ全体でより良いスタイルサポート
- pptx: 自社開発の[pptx-preview](https://github.com/501351981/pptx-preview)ライブラリに基づいて実装、ソースコードは作者から別途有料で入手可能
 
