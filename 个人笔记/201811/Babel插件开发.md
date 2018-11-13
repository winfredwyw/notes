## Babel 插件开发指南

### 参考
https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md#toc-replacing-a-node

### AST

https://astexplorer.net/

### 开发第一个babel插件

```
/*
 * 正式代码去除特点属性的值（name + custom）
 */

module.exports = function ({ types: t }) {
    return {
        visitor: {
            ObjectProperty(path) {
                if (
                    path.node.key.type === "Identifier" &&
                    (path.node.key.name === "name" || path.node.key.name === "custom")
                ) {
                    path.node.value.value = ''
                }
            }
        }    
    }
}
```