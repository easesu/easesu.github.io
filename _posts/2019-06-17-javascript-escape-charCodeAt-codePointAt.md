---
title: escape、charCodeAt、codePointAt在获取字符编码时的区别
---

# `escape`、`charCodeAt`、`codePointAt`在获取字符编码时的区别

在JavaScript中，要想获取字符字符编码值，可能会用到`escape`、`charCodeAt`、`codePointAt`，这三个方法都与字符编码有关，但是具体行为存在差异。

<!-- ES3中规定支持基本平面的Unicode字符集，即0x0000到0xFFFF范围的字符。ES5之后默认支持全部Unicode字符集。

在内存中存储时使用的是UTF16编码。 -->

## 测试字符说明
 | 字符    | 字符码点 | UCS-2编码值   |
 | :-:    | :-:     | :-:          |
 | A      | U+0041  | 0x41         |
 | 一     | U+4E00  | 0x4E00       |
 | 😊     | U+1F60A | 0xD83D0xDE0A |

## 方法说明

### `escape`

`escape`会将部分字符的码元替换为十六进制的转移序列。具体转换规则如下：

 1. 当字符是 `ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789@*_+-./` 其中之一时，则不进行转义，保留原样输出。
 
 2. 当字符的码点大于 `255`（`0xFF`）时，字符码点表示为 `wxyz`，则输出`%uwxyz`。

 3. 其他情况，即字符码点小于或者等于 `255`（`0xFF`）时，字符码点表示为 `xy`，则输出`%uxy`。

具体参考 [B.2.1.1escape ( string )](https://tc39.es/ecma262/#sec-escape-string)


```javascript
escape('A');
escape('一');
escape('😊');
```

输出：

```
A
%u4E00
%uD83D%uDE0A
```

### `charCodeAt`

用法：`string.charCodeAt(index)`

获取指定位置`index`的字符编码。限于历史原因，该方法只能准确获取编码范围在`0x0000`到`0xFFFF`的字符。

```javascript
'A'.charCodeAt(0).toString(16);
'一'.charCodeAt(0).toString(16);
'😊'.charCodeAt(0).toString(16);
```

输出：

```
41
4e00
d83d
```

### `codePointAt`

用法：`string.codePointAt(index)`

获取指定位置`index`的字符码点。此方法是为了弥补上述`charCodeAt`不能准确获取编码高于`0xFFFF`字符的缺陷。

```javascript
'A'.codePointAt(0).toString(16);
'一'.codePointAt(0).toString(16);
'😊'.codePointAt(0).toString(16);
```

输出：

```
41
4e00
1f60a
```

## 异同

 ### 同

 + 三种方法都能正确识别`0x0000`到`0xFFFF`范围的字符编码。

### 异

 + `escape`和`charCodeAt`不能正确识别大于`0xFFFF`范围的字符编码，`codePointAt`可以识别全部Unicode字符。

 + `charCodeAt`和`codePointAt`都是返回字符的十六进制码点值。
 
   `escape`的返回是转义后的字符串，根据输入的字符范围不同，有三种输出格式，还存在部分不会转义的字符，所以`escape`不能安全获取字符码点值。
