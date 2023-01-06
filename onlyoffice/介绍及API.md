# 介绍及api

包括：文本文档、电子表格和演示文稿的在线编辑器
支持格式：DOC, DOCX, TXT, ODT, RTF, ODP, EPUB, ODS, XLS, XLSX, CSV, PPTX, HTML
文档系统交互
(文档地址： <https://api.onlyoffice.com/editors/howitworks>)
需要客户端和服务端

## 客户端

 • 文档管理器  Document manager
 用户浏览器中显示的文档列表，用户可以在其中选择必要的文档并对其执行一些操作（根据提供的权限，用户可以打开文档以查看或编辑，与其他用户共享文档） .
 •  ​文档编辑器（Document editor）​
 文档查看和编辑界面，具有大部分的office文档编辑功能，用作用户和文档编辑服务之间的媒介。

## 服务端

 • 文档存储服务（Document storage service）
 文档存储服务，它存储具有适当访问权限的用户可用的所有文档。 它将文档 ID 和指向这些文档的链接提供给用户在浏览器中看到的文档管理器。
 •  ​文档编辑服务（Document editing service）​
 允许执行文档查看和编辑的服务器服务（如果用户具有执行此操作的适当权限）。文档编辑器界面用于访问所有文档编辑服务功能。
 •  ​文档指令服务（Document command service）​
 允许使用文档编辑服务执行附加命令的服务  (文档地址： <https://api.onlyoffice.com/editors/command/>)
 •  ​文档转换服务（Document conversion service）​
 将文档文件转换为适当的 Office Open XML 格式（文本文档为 docx，电子表格为 xlsx，演示文稿为 pptx）以供编辑或下载。 (文档地址： <https://api.onlyoffice.com/editors/conversionapi>)
 •  ​文档构建服务（Document builder service）​
 无需实际运行文档处理编辑器即可轻松构建文档的服务。 (文档地址： <https://api.onlyoffice.com/editors/documentbuilderapi>)

> 请注意，ONLYOFFICE 文档服务器包括文档编辑器(提供到前端渲染的API)、文档编辑服务、文档指令服务、文档转换服务和文档生成器服务。
文档管理器和文档存储服务要么包含在社区服务器中，要么必须在自己的应用中实现。

## 文档编辑服务配置 API

(文档地址： <https://api.onlyoffice.com/editors/advanced>)

```json
{
  "documentType": "word",
  "height": "100%",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e30.t-IDcSemACt8x4iTMCda8Yhe3iZaWbvV5XKSTbuAn0M",
  "type": "desktop",
  "width": "100%",
  "document": {},
  "editorConfig": {},
  "events": {}
}
```

### documentType

 • word - text document (.doc, .docm, .docx, .docxf, .dot, .dotm, .dotx, .epub, .fodt, .fb2, .htm, .html, .mht, .odt, .oform, .ott, .oxps, .pdf, .rtf, .txt, .djvu, .xml, .xps),
 • cell - spreadsheet (.csv, .fods, .ods, .ots, .xls, .xlsb, .xlsm, .xlsx, .xlt, .xltm, .xltx),
 • slide - presentation (.fodp, .odp, .otp, .pot, .potm, .potx, .pps, .ppsm, .ppsx, .ppt, .pptm, .pptx).

### document

包含与文档有关的所有参数（标题、url、文件类型等）

```json
"document": {
  "fileType": "docx",
  "key": "Khirz6zTPdfd7",
  "title": "Example Document Title.docx",
  "url": "https://example.com/url-to-example-document.docx",
  "info": {},
  "permissions": {},
},
```

• fileType
可取的值：.csv, .djvu, .doc, .docm, .docx, .docxf, .dot, .dotm, .dotx, .epub, .fb2, .fodp, .fods, .fodt, .htm, .html, .mht, .odp, .ods, .odt, .oform, .otp, .ots, .ott, .oxps, .pdf, .pot, .potm, .potx, .pps, .ppsm, .ppsx, .ppt, .pptm, .pptx, .rtf, .txt, .xls, .xlsb, .xlsm, .xlsx, .xlt, .xltm, .xltx, .xml, .xps.
• key
定义服务用来识别文档的唯一文档标识符。  ​如果key的值是已知密钥，则将从缓存中获取文档​ 。 每次编辑和保存文档时，都必须重新生成密钥。 文档 url 可以用作键，但不能使用特殊字符，长度限制为 128 个字符。
key会作为task_result表的id列的值
• title
为查看或编辑的文档定义所需的文件名，该文件名也将在下载文档时用作文件名。 长度限制为 128 个字符。
• url
定义存储查看或编辑的源文档的绝对 URL。

### info

包含文档的附加参数（文档所有者、存储文档的文件夹、上传日期、共享设置）；

```json
"document": {
  "info": {
    "favorite": true,  // 定义收藏夹图标的高亮状态。 当用户单击图标时，将调用 onMetaChange 事件。 如果参数未定义，则收藏夹图标不会显示在编辑器窗口标题处。
    "folder": "Example Files", // 定义存储文档的文件夹（如果文档存储在根文件夹中，则可以为空）。此处存疑，还需要研究使用场景
    "owner": "John Smith", // 定义文档作者/创建者的名称
    "sharingSettings": [// 显示有关允许与其他用户共享文档的设置的信息：
      {
        "permissions": "Full Access",
        "user": "John Smith"
      },
      {
        "isLink": true, // 将用户图标更改为链接图标
        "permissions": "Read Only", // user的访问权限，可配置的值：Full Access, Read Only 和 Deny Access
        "user": "External link" // 文档共享的目标用户
      },
    ],
    "uploaded": "2010-07-07 3:46 PM" // 定义文档创建日期
  }
}
```

### permissions

文档权限部分允许更改要编辑和下载的文档的权限。

```json
"document": {
  "permissions": {
    "chat": true, // 定义是否在文档中启用聊天功能 默认是true。
    "comment": true, // 定义是否可以评论文档
    "commentGroups": {
      "edit": ["Group2", ""],
      "remove": [""],
      "view": ""
    },
    "copy": true, // 定义是否可以将内容复制到剪贴板 默认是true。
    "deleteCommentAuthorOnly": false, //  定义用户是否只能删除他/她的评论 默认是false
    "download": true, // 定义文档是可以下载还是只能在线查看或编辑 默认是true
    "edit": true, // 定义文档是可以编辑还是只能查看，如果是false，即使 edit -> mode为edit，也不可编辑；默认是true
    "editCommentAuthorOnly": false, // 定义用户是否只能编辑他/她的评论。 默认值是false
    "fillForms": true, // 定义是否可以填写表格
    "modifyContentControl": true, // 定义是否可以更改内容控制设置
    "modifyFilter": true, // 定义过滤器是否可以全局应用（值为true）影响所有其他用户，或本地应用（值为false），即仅适用于当前用户
    "print": true, // 定义是否可以打印文档
    "protect": true, // 定义工具栏上的保护选项卡和左侧菜单中的保护按钮是显示 (true) 还是隐藏 (false)。 默认值是true。
    "review": true,
    "reviewGroups": ["Group1", "Group2", ""],
    "userInfoGroups": ["Group1", ""]
  },
},
```

### editorConfig

editorConfig 部分允许更改与编辑器界面有关的参数：打开模式（查看器或编辑器）、界面语言、附加按钮等）。

```json
"editorConfig": {
    "actionLink": ACTION_DATA,
    "callbackUrl": "https://example.com/url-to-callback.ashx", // 指定文档存储服务的绝对 URL，使用onlyoffice的应用来实现
    "coEditing": {
      "mode": "fast", // 协同编辑模式 fast | strict
      "change": true // 定义是否可以在编辑器界面中更改共同编辑模式
    },
    "createUrl": "https://example.com/url-to-create-document/",
    "lang": "en",
    "location": "",
    "mode": "edit", // 定义编辑器打开模式。 可以是view, 打开文档进行查看;也可以是edit在编辑模式下打开文档，从而允许对文档数据进行更改。 默认值为“edit”。
    "recent": [
      {
        "folder": "Example Files",
        "title": "exampledocument1.docx",
        "url": "https://example.com/exampledocument1.docx"
      },
      {
        "folder": "Example Files",
        "title": "exampledocument2.docx",
        "url": "https://example.com/exampledocument2.docx"
      },
    ],
    "region": "en-US",
    "templates": [
      {
        "image": "https://example.com/exampletemplate1.png",
        "title": "exampletemplate1.docx",
        "url": "https://example.com/url-to-create-template1"
      },
      {
        "image": "https://example.com/exampletemplate2.png",
        "title": "exampletemplate2.docx",
        "url": "https://example.com/url-to-create-template2"
      },
    ],
    "user": {
      "group": "Group1",
      "id": "78e1e841",
      "name": "John Smith"
    }
},
```

### customization

允许自定义编辑器界面，使其看起来像您的其他产品（如果有），并更改附加按钮、链接、更改徽标和编辑器所有者详细信息的存在或不存在。

```json
"editorConfig": {
  "customization": {
    "anonymous": { // 添加对匿名名称的请求
      "request": true, // 是否弹出一个需要填写用户名的弹框
      "label": "Guest"
    },
    "autsave": true,  
    "comments": true,
    "compactHeader": false,
    "compactToolbar": false,
    "compatibleFeatures": false,
    "customer": {
        "address": "My City, 123a-45",
        "info": "Some additional information",
        "logo": "https://example.com/logo-big.png",
        "logoDark": "https://example.com/dark-logo-big.png",
        "mail": "john@example.com",
        "name": "John Smith and Co.",
        "www": "example.com"
    },
    "features": {
        "spellcheck": {
            "mode": true,
        }
    },
    "feedback": {
        "url": "https://example.com",
        "visible": true
    },
    "forcesave": false,
    "goback": {
        "blank": true,
        "requestClose": false,
        "text": "Open file location",
        "url": "https://example.com"
    },
    "help": true,
    "hideNotes": false,
    "hideRightMenu": false,
    "hideRulers": false,
    "logo": {
        "image": "https://example.com/logo.png",
        "imageDark": "https://example.com/dark-logo.png",
        "url": "https://www.onlyoffice.com"
    },
    "macros": true,
    "macrosMode": "warn",
    "mentionShare": true,
    "plugins": true,
    "review": {
        "hideReviewDisplay": false,
        "showReviewChanges": false,
        "reviewDisplay": "original",
        "trackChanges": true,
        "hoverMode": false
    },
    "toolbarHideFileName": false,
    "toolbarNoTabs": false,
    "uiTheme": "theme-dark",
    "unit": "cm",
    "zoom": 100
  },
},
```

### embedded

仅适用于嵌入文档类型（请参阅配置部分以了解如何定义嵌入文档类型）。 它允许更改定义嵌入模式中按钮行为的设置。

```json
"editorConfig": {
  "embedded": {
    "embedUrl": "https://example.com/embedded?doc=exampledocument1.docx",
    "fullscreenUrl": "https://example.com/embedded?doc=exampledocument1.docx#fullscreen",
    "saveUrl": "https://example.com/download?doc=exampledocument1.docx",
    "shareUrl": "https://example.com/view?doc=exampledocument1.docx",
    "toolbarDocked": "top"
  },
},
```

### plugins

允许将特殊插件连接到文档服务器，帮助向文档编辑器添加附加功能。

```json
"editorConfig": {
    "plugins": {
      "autostart": [
        "asc.{0616AE85-5DBE-4B6B-A0A9-455C4F1503AD}",
        "asc.{FFE1F462-1EA2-4391-990D-4CC84940B754}",
      ],
      "pluginsData": [
        "https://example.com/plugin1/config.json",
        "https://example.com/plugin2/config.json",
      ]
    },
},
```

events
允许更改与事件有关的所有功能。
(文档地址： <https://api.onlyoffice.com/editors/config/events>)

 1. onAppReady 当应用程序加载到浏览器时调用的函数。
 2. onCollaborativeChanges  当文档被其他用户在严格的共同编辑模式下共同编辑时调用的函数。
 3. onDocumentReady  当文档加载到文档编辑器完成时调用的函数。

## Command Service 指令服务

使用post请求，请求参数以JSON格式传输，请求url： ​<https://documentserver/coauthoring/CommandService.ashx>​ ，documentserver 是安装了 ONLYOFFICE 文档服务器的服务器的名称。
key是文档的标识

• drop
断开在 users 参数中指定的标识符的用户与文档编辑服务的连接。 这些用户将能够查看文档，但不允许对其进行更改。
请求报文：

```json
{
    "c": "drop",
    "key": "Khirz6zTPdfd7", // 定义用于明确标识文档文件的文档标识符。
    "users": [ "6d5a81d0" ]
}
```

• forcesave
强制保存正在编辑的文档而不关闭它。 执行此命令后可能会继续编辑文档，因此这不是最终保存的文档版本。

```json
{
    "c": "forcesave",
    "key": "Khirz6zTPdfd7",
    "userdata": "sample userdata"
}
```

• info
请求文档状态和打开文档进行编辑的用户的标识符列表。 响应将被发送到回调处理程序

```json
{
  "c": "info",
  "key": "Khirz6zTPdfd7"
}
```

• license
向 Document Server 请求许可证以及有关服务器和用户配额的信息。
(文档地址： <https://api.onlyoffice.com/editors/command/license>)
• meta
为所有协作编辑者更新文档的元信息。

```json
{
  "c": "meta",
  "key": "Khirz6zTPdfd7",
  "meta": {
      "title": "Example Document Title.docx"
  }
}
```

• version
当前Document Server的版本

```json
{
  "c": "version"
}
```

## 文档转换服务  Conversion API

POST 请求，JSON格式请求报文  请求url： <https://documentserver/ConvertService.ashx>
，其中 documentserver 是安装了 ONLYOFFICE 文档服务器的服务器的名称
文档地址： <https://api.onlyoffice.com/editors/conversionapi>
请求参数：
 • async 定义转换请求是是否为异步
 • codePage
  >定义从 csv 或 txt 格式转换时的文件编码。可配置的值：
  932 - Japanese (Shift-JIS),
  950 - Chinese Traditional (Big5),
  1250 - Central European (Windows),
  1251 - Cyrillic (Windows),
  65001 - Unicode (UTF-8).

 • delimiter
 定义从 csv 格式转换时用于分隔值的分隔符。可配置的值：
 > 0 - no delimiter, 没有分隔符
  1 - tab,
  2 - semicolon, 分号
  3 - colon, 冒号
  4 - comma, 逗号
  5 - space. 空格

## 文档生成器服务

POST 请求，JSON格式请求报文  请求url： <https://documentserver/docbuilder>
，其中 documentserver 是安装了 ONLYOFFICE 文档服务器的服务器的名称
 • async
 • key
 • token
 • url
