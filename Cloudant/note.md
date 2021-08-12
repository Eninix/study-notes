# 前置工作

```
https://cloud.ibm.com/apidocs/cloudant?code=node#postdocument
```

```
npm install --save @ibm-cloud/cloudant
```

# 连接服务器

```javascript
// 导包
const {
	CloudantV1
} = require('@ibm-cloud/cloudant');
const {
	IamAuthenticator
} = require('ibm-cloud-sdk-core');

//验证apikey ,创建IAM验证器 authenticator
const authenticator = new IamAuthenticator({
	apikey: me.apikey
})

//创建服务，设置IAM验证器 authenticator
const service = new CloudantV1({
	authenticator: authenticator
})

//设置服务url
service.setServiceUrl(me.url);
```

# 数据库操作

## //查询实例中所有数据库名称的列表

```
service.getAllDbs().then(response => {
  console.log(response.result);
  console.log("-------------------------");
});
```



## //查询多个数据库的信息

```
service.postDbsInfo({
  keys: ['jsondb_test'] // keys中为数据库名称，可查询多个
}).then(response => {
  console.log(response.result);
  console.log("-------------------------");
});
```



## //删除数据库

```
service.deleteDatabase({
  db: 'test123' // 此处为数据库名称
}).then(response => {
  console.log(response.result);
  console.log("-------------------------");
});


```



## //获取数据库信息

```
service.getDatabaseInformation({
  db: 'test'
}).then(response => {
  console.log(response.result);
  console.log("-------------------------");
});
```



## //检索数据库的 HTTP 标头

```
service.headDatabase({
  db: 'test'
}).then(response => {
  console.log(response.status);
  console.log("-------------------------");
});
```



## //创建数据库

```
service.putDatabase({
  db: 'products',
  partitioned: true
}).then(response => {
  console.log(response.result);
  console.log("-------------------------");
});
```



## //查询数据库文档更改提要

```
service.postChanges({
  db: 'test'
}).then(response => {
  console.log(response.result);
});
```

# 文档操作

## 在数据库中创建或修改文档

```js
const productsDoc = {
  _id: 'hello:123',
  name: 'hello world',
};

service.postDocument({
  db: 'products',
  document: productsDoc
}).then(response => {
  console.log(response.result);
});
```

## 查询数据库中所有文档的列表

```js
service.postAllDocs({
  db: 'orders',
  includeDocs: true,
  startkey: 'abc',
  limit: 10
}).then(response => {
  console.log(response.result);
});
```

## 多查询数据库中所有文档的列表

```js
const allDocsQueries: CloudantV1.AllDocsQuery[] = [{
    keys: ['small-appliances:1000042', 'small-appliances:1000043'],
  },
  {
    limit: 3,
    skip: 2
}];

service.postAllDocsQueries({
  db: 'products',
  queries: allDocsQueries
}).then(response => {
  console.log(response.result);
});
```

## 批量修改数据库中的多个文档

```js
const eventDoc1: CloudantV1.Document = {
  _id: '0007241142412418284',
  type: 'event',
  userid: 'abc123',
  eventType:'addedToBasket',
  productId: '1000042',
  date: '2019-01-28T10:44:22.000Z'
}
const eventDoc2: CloudantV1.Document = {
  _id: '0007241142412418285',
  type: 'event',
  userid: 'abc234',
  eventType: 'addedToBasket',
  productId: '1000050',
  date: '2019-01-25T20:00:00.000Z'
}

const bulkDocs: CloudantV1.BulkDocs = {  docs: [eventDoc1, eventDoc2]
}

service.postBulkDocs({
  db: 'events',
  bulkDocs: bulkDocs
}).then(response => {
  console.log(response.result);
});
```

## 批量查询多个文档的修订信息

```js
const bulkGetDoc1: CloudantV1.BulkGetQueryDocument = {
  id: docId,
  rev: '3-917fa2381192822767f010b95b45325b'
};
const bulkGetDoc2: CloudantV1.BulkGetQueryDocument = {
  id: docId,
  rev: '4-a5be949eeb7296747cc271766e9a498b'
};

const bulkGetDocs: CloudantV1.BulkGetQueryDocument[] = [bulkGetDoc1, bulkGetDoc2];

const postBulkGetParams: CloudantV1.PostBulkGetParams = {
  db: 'orders',
  docs: bulkGetDocs,
};

service.postBulkGet(postBulkGetParams)
  .then(response => {
    console.log(response.result);
  });
```

## 删除文档

```js
service.deleteDocument({
  db: 'events',
  docId: '0007241142412418284',
  rev: '2-9a0d1cd9f40472509e9aac6461837367'
}).then(response => {
  console.log(response.result);
});
```

## 检索文档

```js
service.getDocument({
  db: 'products',
  docId: 'small-appliances:1000042'
}).then(response => {
  console.log(response.result);
});
```

## 检索文档的 HTTP 标头

```js
service.headDocument({
  db: 'events',
  docId: '0007241142412418284'
}).then(response => {
  console.log(response.status);
  console.log(response.headers['etag']);
});
```

## 创建或修改文档

```js
const eventDoc: CloudantV1.Document = {
  type: 'event',
  userid: 'abc123',
  eventType: 'addedToBasket',
  productId: '1000042',
  date: '2019-01-28T10:44:22.000Z'
};

service.putDocument({
  db: 'events',
  docId: '0007241142412418284',
  document: eventDoc
}).then(response => {
  console.log(response.result);
});
```