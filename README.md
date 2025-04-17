#NudeNet:适用于TF JS和Node JS的NSFW对象检测

##注释

-包含在 中的模型`/模型/`被转换成了**TF JS图形模型**格式来自原始存储库  
为了可读性，还对模型描述符和签名进行了额外解析
-模型包括两者**违约**和**基础**变更  
注意，每个变体的类列表都不同。
-模型使用动态输入尺寸
-结果解析实现与原始实现不一致  
并使用原生TF JS操作实现，并针对JavaScript执行进行了优化
-代码还包括对输入图像中暴露的身体部位的简单褪色功能。
-示例实现**NodeJS**: `src/nudenet.js`  
-示例实现**浏览器**: `src/index.ts`  

<br>

##输出

返回对象的结构:

```js
{
输入:{
文件:字符串，
宽度:数字，
身高:数字，
  },
人:布尔， // 检测到人了吗？
性感:布尔，// 人被认为性感吗？
裸体:布尔，// 人被认为是裸体的吗？
部件：数组<{//检测到的身体部件数组
分数:数字，//检测的信心
身份证：号码，
类：字符串，//主体部分的标签
框:数字[]，//[x，y，宽度，高度]
  }>,
}
```

上课地点可以是:

```js
类: { // 类标签
基础: [ // 基础模型变异
"暴露的肚子"
"裸露臀部"
"裸露的乳房"
"外露的阴道"
"暴露的阴茎"
      'male breast',
    ],
    default: [ // default model variation
      'exposed anus',
      'exposed armpits',
      'belly',
      'exposed belly',
      'buttocks',
      'exposed buttocks',
      'female face',
      'male face',
      'feet',
      'exposed feet',
      'breast',
      'exposed breast',
      'vagina',
      'exposed vagina',
      'male breast',
      'exposed penis',
    ],
  },
```

<br>

## Example

![Example Image](samples/nude-out-default.jpg)

There are three examples:

- NodeJS Image processing: `src/nudenet.js`  
- NodeJS Video processing: `src/node-video.js`  
  this demo has additional dependencies which are not installed by default  
  `@tensorflow/tfjs-node-gpu` and `pipe2jpeg`  
  and requires functional copy of `ffmpeg`  
- Browser: `src/index.html`  
  written in TypeScript `src/index.ts` and transpiled to JavaScript `dist/index.js`  

<br>

> node src/nudenet.js -i samples/nude.jpg -o samples/nude-out.jpg

```js
2021-10-20 11:11:11 INFO:  nudenet version 0.0.1
2021-10-20 11:11:11 INFO:  User: vlado Platform: linux Arch: x64 Node: v16.8.0
2021-10-20 11:11:11 INFO:  tfjs version: 3.9.0 backend: tensorflow
2021-10-20 11:11:11 INFO:  options: { debug: true, modelPath: 'file://model/model.json', minScore: 0.3, maxResults: 50, iouThreshold: 0.5, outputNodes: [ 'output1', 'output2', 'output3' ], blurNude: true, blurRadius: 25 }
2021-10-20 11:11:11 STATE: loaded graph model: file://model/model.json
2021-10-20 11:11:11 INFO:  loaded image: samples/nude.jpg width: 801 height: 1112
2021-10-20 11:11:13 DATA:  result: {
  input: { file: 'samples/nude.jpg', width: 801, height: 1112 },
  person: true,
  sexy: true,
  nude: true,
  parts: [
    { score: 0.8839950561523438, id: 3, class: 'exposed belly', box: [ 194, 639, 244, 221 ] },
    { score: 0.7332422137260437, id: 11, class: 'exposed breast', box: [ 371, 450, 142, 154 ] },
    { score: 0.566450834274292, id: 6, class: 'female face', box: [ 282, 164, 169, 155 ] },
    { score: 0.5646520256996155, id: 11, class: 'exposed breast', box: [ 202, 430, 134, 156 ] },
    { score: 0.5579367876052856, id: 12, class: 'vagina', box: [ 187, 908, 92, 96 ] }
  ]
}
2021-10-20 11:11:13 STATE: created output image: samples/nude-out.jpg
2021-10-20 11:11:13 STATE: done: model:file://model/model.json input:samples/nude.jpg output:samples/nude-out.jpg objects: 5
```

<br>

## Credits

- Original implementation: <https://github.com/notAI-tech/NudeNet>
- Model checkpoints: <https://github.com/notAI-tech/NudeNet/releases/tag/v0>
