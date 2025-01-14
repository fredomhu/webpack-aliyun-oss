# webpack-aliyun-oss
A webpack(>=4) plugin to upload assets to aliyun oss, u can use it with or without webpack.

一个webpack(>=4)插件，上传资源到阿里云oss。可以作为webpack插件使用，也可独立使用(从0.1.0开始支持)

- 默认按output.path (webpack.config.js) 里的文件路径上传到oss，需要指定上传根目录(dist)。
- 也可以通过`setOssPath`来配置不同的上传路径。
- 独立使用时请通过`setOssPath`指定上传路径, 否则将上传到`dist`指定的路径下。

Install
------------------------
```shell
$ npm i webpack-aliyun-oss -D
```

Options
------------------------

- `from`: 上传哪些文件，支持类似gulp.src的glob方法，如'./build/**', 可以为glob字符串或者数组。
    - 作为插件使用时：可选，默认为output.path下所有的文件。
    - 独立使用时：必须，否则不知道从哪里取图片：）
- `dist`: 上传到oss哪个目录下，默认为oss根目录。可作为路径前缀使用。
- `region`: 阿里云上传区域
- `accessKeyId`: 阿里云的授权accessKeyId
- `accessKeySecret`: 阿里云的授权accessKeySecret
- `bucket`: 上传到哪个bucket
- `timeout`: oss超时设置，默认为30秒(30000)
- `verbose`: 是否显示上传日志，默认为true
- `deletOrigin`: 上传完成是否删除原文件，默认false
- `deleteEmptyDir`: 如果某个目录下的文件都上传到cdn了，是否删除此目录。deleteOrigin为true时候生效。默认false。
- `setOssPath`: 自定义上传路径的函数。接收参数为当前文件路径。不传，或者所传函数返回false则按默认路径上传。(默认为output.path下文件路径)
- `setHeaders`: 配置headers的函数。接收参数为当前文件路径。不传，或者所传函数返回false则不设置header。
- `test`: 测试，仅显示要上传的文件，但是不执行上传操作。默认false

#### 注意: `accessKeyId, accessKeySecret` 很重要，注意保密!!!

Example
------------------------

##### 作为webpack插件使用
```javascript
const WebpackAliyunOss = require('webpack-aliyun-oss');
const webpackConfig = {
  // ... 省略其他
  plugins: [new WebpackAliyunOss({
    from: ['./build/**', '!./build/**/*.html'],
    dist: 'path/in/alioss',
    region: 'your region',
    accessKeyId: 'your key',
    accessKeySecret: 'your secret',
    bucket: 'your bucket',
    setOssPath(filePath) {
      // some operations to filePath
      return '/new/path/to/file.js';
    },
    setHeaders(filePath) {
      // some operations to filePath
      return {
        'Cache-Control': 'max-age=31536000'
      }
    }
  })]
}
```

##### 独立使用

```javascript
const WebpackAliyunOss = require('webpack-aliyun-oss');
new WebpackAliyunOss({
    from: ['./build/**', '!./build/**/*.html'],
    dist: 'path/in/alioss',
    region: 'your region',
    accessKeyId: 'your key',
    accessKeySecret: 'your secret',
    bucket: 'your bucket',
    setOssPath(filePath) {
      // some operations to filePath
      return '/new/path/to/file.js';
    },
    setHeaders(filePath) {
      // some operations to filePath
      return {
        'Cache-Control': 'max-age=31536000'
      }
    }
}).apply(); 
```   
