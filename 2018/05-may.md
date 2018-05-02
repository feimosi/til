<h1 align="center">02.05.2018</h1>

## node.js: Reading a file asynchronously via async iteration

Starting with Node.js v10. Readable streams have a property whose key is `Symbol.asyncIterator`, 
which enables the `for-await-of` loop to iterate over their chunks. 

```js
async function main(inputFilePath) {
  const readStream = fs.createReadStream(inputFilePath,
    { encoding: 'utf8', highWaterMark: 1024 });

  for await (const chunk of readStream) {
    console.log('>>> '+chunk);
  }
  console.log('### DONE ###');
}
```

:arrow_right: http://2ality.com/2018/04/async-iter-nodejs.html
