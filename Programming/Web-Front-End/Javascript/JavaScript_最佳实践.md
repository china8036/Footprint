- [JavaScript 最佳实践](#javascript-%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5)
  - [命名规范](#%E5%91%BD%E5%90%8D%E8%A7%84%E8%8C%83)
  - [Refer Links](#refer-links)

# JavaScript 最佳实践

## 命名规范

- JavaScript 局部变量命名采用首字母小写，其它单词首字母大写的方式；

- 命名应采用有意义的单词命名，不推荐使用标识变量类型的前缀命名，如 int、str 等；

- 在 JavaScript 面向对象变成中，公有接口的命名为首字母大写，私有接口的命名为首字母小写；

- 在使用 jQuery 的项目中，推荐给 jQuery 类型变量添加“$”为前缀，如：`var $tocTitle = $('.reader-toc-title');`；

- JavaScript 中单引号与双引号几乎没有区别，但推荐在 JavaScript 中使用单引号，便于在 JavaScript 中包含惯用双引号的 HTML 代码；

- 若注释为未占多行，建议使用 //，不推荐使用 /**/；注释应单独占一行，而不是在代码同一行直接添加；



## Refer Links
https://zoomzhao.github.io/code-guide/

《Web 前端开发最佳实践》 党建 著