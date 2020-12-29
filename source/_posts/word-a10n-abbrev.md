---
title: Word a10n (abbreviation)
date: 2020-3-10 15:57:59
tags: Codewars
---
The word `i18n` is a common abbreviation of in the developer community, used instead of typing the whole word and trying to spell it correctly. Similarly, `a11y` is an abbreviation of `accessibility`.

Write a function that takes a string and turns any and all "words" (see below) within that string of **length 4 or greater** into an abbreviation, following these rules:

- A "word" is a sequence of alphabetical characters. By this definition, any other character like a space or hyphen (eg. "elephant-ride") will split up a series of letters into two words (eg. "elephant" and "ride").
- The abbreviated version of the word should have the first letter, then the number of removed characters, then the last letter (eg. "elephant ride" => "e6t r2e").

单词`i18n`是开发人员社区中 `internationalization`的常见缩写。同样的 `a11y`是`accessibility`缩写。

编写一个函数，该函数接受一个字符串，并按照以下规则将所有长度为4或更长的字符串中单词转换为缩写：

- 单词是按字母顺序排列的字符序列。根据此定义，任何其他字符，如空格或连字符将把一系列字母分成两个词。例如elephant-ride 则看做 elephant ride 去解析
- 单词的缩写版本应该包含第一个字母，然后是删除的字符数，然后是最后一个字母。

## 解题思路

题目叽里呱啦说了一大堆，其实主要是对字符串中的满足单词做如下处理。

- 满足条件： 单词大于等于 4个字符。由空个或者连字符隔开的单词。
- 操作处理：保留首位字符，中间为由字符串长度减 2 的数字，拼接成的字符串

我的思路：

1. 写一个专门处理单词的方法
   
```js
const abbrev = (word) => `${word[0]}${word.length}${string[word.length - 1]}`
```

2. 然后用正则去匹配符合条件的单词
   - 单词大于四个字符
   - 由空格或者连字符分开的单词
  
```js
const reg = /[a-zA-Z]{4,}/g
```

3. 然后一次替换下字符中匹配到的字符，所有的代码如下：
   
```js
const abbreviate = (string) =>
  string.replace(
	/[a-zA-Z]{4,}/g,
	(word) => `${word[0]}${word.length - 2}${word[word.length - 1]}`
  )

```

也算是一行代码，虽然很长。。。

那么换种思路，使用正则直接匹配到需要替换成数字的位置，那代码量是不是更加少了？

一行代码解决战斗！

这里活用了`/B` 单词边界，非常之骚，细品。


```js
const abbreviate = string => string.replace(/\B\w{2,}\B/g, match=> match.length)
```

## 参考资料

[Training on Word a10n (abbreviation) | Codewars](https://www.codewars.com/kata/5375f921003bf62192000746/train/javascript)
