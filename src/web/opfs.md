# OPFS

get OPFS root directory handle

```js
const root = await navigator.storage.getDirectory();
```

```js
create a file

const fileHandle = await opfsRoot
    .getFileHandle('my first file', {create: true});
const directoryHandle = await opfsRoot
    .getDirectoryHandle('my first folder', {create: true});
const nestedFileHandle = await directoryHandle
    .getFileHandle('my first nested file', {create: true});
const nestedDirectoryHandle = await directoryHandle
    .getDirectoryHandle('my first nested folder', {create: true});
```

get the file from file handle

```js
const readableFile = await fileHandle.getFile();
console.log(readableFile.name);
console.log(readableFile.size); // size in bytes
console.log(readableFile.type);
console.log(await readableFile.text()); // text contents
console.log(await readableFile.stream()); 
console.log(await readableFile.arrayBuffer()); 
```

Writing to file

```js
const writableFile = await fileHandle.createWritable(); // writable stream
await writableFile.write("Hello world");
awaut writableFile.close();
```

Delete file

```js
const folder = await root.getDirectoryHandle('folderName');
await folder.removeEntry('fileName');
// or
await fileHandle.remove();
```

delete everything in folder

```js
const folder = await root.getDirectoryHandle('folderName');
await folder.remove({ recursive: true });
```

