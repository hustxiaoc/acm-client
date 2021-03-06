# acm-client

> Aliyun acm client for Node.js. 阿里云 acm Node.js 客户端 https://help.aliyun.com/document_detail/60137.html

## Install

```bash
$ npm i acm-client --save
```

## Usage

```js
const ACMClient = require('acm-client');
const co = require('co');
const acm = new ACMClient({
  endpoint: 'acm.aliyun.com', // acm 控制台查看
  namespace: '***************', // acm 控制台查看
  accessKey: '***************', // acm 控制台查看
  secretKey: '***************', // acm 控制台查看
  requestTimeout: 6000, // 请求超时时间，默认6s
});

// 主动拉取配置
co(function*() {
  const content= yield acm.getConfig('test', 'DEFAULT_GROUP');
  console.log('getConfig = ',content);
});

// 监听数据更新
acm.subscribe({
  dataId: 'test',
  group: 'DEFAULT_GROUP',
}, content => {
  console.log(content);
});

// 发布配置接口
co(function*() {
  const content= yield acm.publishSingle('test', 'DEFAULT_GROUP', '测试');
  console.log('getConfig = ',content);
});

// 删除配置
co(function*() {
  yield acm.remove('test', 'DEFAULT_GROUP');
});

// 批量获取配置
co(function*() {
  const content = yield amc.batchGetConfig(['test', 'test1'], 'DEFAULT_GROUP');
});
```

### Error Events 异常处理

```js
acm.on('error', function (err) {
  // 可以在这里统一进行日志的记录
  // 如果不监听错误事件，所有的异常都将会打印到 stderr
});
```

### API
#### 获取配置
* `function* getConfig(dataId, group)`
- {String} dataId - 配置id
- {String} group - 配置分组

#### 发布配置
* `function* publishSingle(dataId, group, content)`
- {String} dataId - 配置id
- {String} group - 配置分组
- {String} content - 发布内容

#### 删除配置
* `function* remove(dataId, group)`
- {String} dataId - 配置id
- {String} group - 配置分组

#### 批量获取多个配置
* `function* batchGetConfig(dataIds, group)`
- {Array} dataIds - 配置id
- {String} group - 配置分组

#### 订阅配置
* `function subscribe(info, listener)`
  - {Object} info
    - {String} dataId - 配置id
    - {String} group - 配置分组
  - {Function} listener - 回调函数

#### 取消订阅  
* `function unSubscribe(info, [listener])`
  - {Object} info
    - {String} dataId - 配置id
    - {String} group - 配置分组
  - {Function} listener - 回调函数（可选，不传就移除所有监听函数）


## Contacts

* [@hustxiaoc](https://github.com/hustxiaoc)

