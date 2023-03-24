# 用 NodeJS 将文件和文件夹结构转换成 Bash 脚本

> 原文：<https://medium.com/nerd-for-tech/convert-files-and-folders-structures-to-bash-scripts-with-nodejs-de2c29d036cd?source=collection_archive---------17----------------------->

这是一个简单的 NodeJS 应用程序，它将一个源文件夹作为输入，并生成一个 Bash 脚本。Bash 脚本在源文件夹中有所有文件的内容和文件夹的结构，它可以在执行时重新创建它们。

这里有源代码:[https://github.com/alexadam/folders-to-script](https://github.com/alexadam/folders-to-script)

第一步，遍历源文件夹中的所有文件:

```
const fs = require("fs")
const path = require("path")const listFiles = (dirPath, result) => {
 files = fs.readdirSync(dirPath) result = result || ['#!/bin/sh'] for (const file of files) {
   ///...
 } return result
}const allFiles = listFiles(rootPath)fs.writeFileSync('./result.sh', allFiles.join('\n'))
```

如果**文件**是一个目录，添加一个`mkdir -p`命令:

```
for (const file of files) {
 if (fs.statSync(dirPath + "/" + file).isDirectory()) {
 result.push(`mkdir -p ${path.join(dirPath, "/",  file).replace(rootPath, '.')}`)
 result = listFiles(dirPath + "/" + file, result)
 } 
}
```

否则，根据扩展名测试**文件**是二进制还是文本:

```
const textExt = ['txt', 'md', 'html', 'json', 'js', 'jsx', 'ts', 'tsx'];
const binaryExt = ['jpg', 'png', 'gif', 'pdf', 'mp3', 'mp4'];const getFileExt = (filePath) => filePath.split('.').pop()...
    else {
      const filePath = path.join(dirPath, "/", file);
      const fileExt = getFileExt(filePath);
      const fileContent = fs.readFileSync(filePath); if (textExt.includes(fileExt)) {
        result.push(`
cat > ${path.join(dirPath, "/", file).replace(rootPath, '.')} << "EOF"
${fileContent}
EOF
`)
      } else if (binaryExt.includes(fileExt)) {
        const bindata = fileContent.toString('binary');
        const hexdata = new Buffer(bindata, 'ascii').toString('hex');
        result.push(`echo '${hexdata}' | xxd -r -p > ${path.join(dirPath, "/", file).replace(rootPath, '.')}`)
      }
    }
...
```

二进制文件存储为十六进制字符串。

您可以在 *textExt* 或 *binaryExt* 数组中添加/删除文件扩展名。

这是完整的 NodeJS 脚本:

```
const fs = require("fs")
const path = require("path")const rootPath = process.argv[2]const textExt = ['txt', 'md', 'html', 'json', 'js', 'jsx', 'ts', 'tsx'];
const binaryExt = ['jpg', 'png', 'gif', 'pdf', 'mp3', 'mp4'];const getFileExt = (filePath) => filePath.split('.').pop()const listFiles = (dirPath, result) => {
  files = fs.readdirSync(dirPath) result = result || ['#!/bin/sh'] for (const file of files) {
    if (fs.statSync(dirPath + "/" + file).isDirectory()) {
      result.push(`mkdir -p ${path.join(dirPath, "/", file).replace(rootPath, '.')}`)
      result = listFiles(dirPath + "/" + file, result)
    } else {
      const filePath = path.join(dirPath, "/", file);
      const fileExt = getFileExt(filePath);
      const fileContent = fs.readFileSync(filePath); if (textExt.includes(fileExt)) {
        result.push(`
cat > ${path.join(dirPath, "/", file).replace(rootPath, '.')} << "EOF"
${fileContent}
EOF
`)
      } else if (binaryExt.includes(fileExt)) {
        const bindata = fileContent.toString('binary');
        const hexdata = new Buffer(bindata, 'ascii').toString('hex');
        result.push(`echo '${hexdata}' | xxd -r -p > ${path.join(dirPath, "/", file).replace(rootPath, '.')}`)
      }
    }
  } return result
}const allFiles = listFiles(rootPath)fs.writeFileSync('./result.sh', allFiles.join('\n'))
```

用`node index.js test` - >测试它，它会生成一个 Bash 脚本文件，名为 [*result.sh*](http://result.sh)

然后，在一个空目录中，用`sh result.sh`执行脚本。