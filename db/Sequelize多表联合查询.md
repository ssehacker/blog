# Sequelize多表联合查询

## Sequelize简介
[Sequelize](http://docs.sequelizejs.com)是一个node数据持久层框架，主要应用于关系型数据库，支持`MySQL`、`SQLite`、`PostgreSQL`、`MSSQL`等多种数据库。[官方文档](http://docs.sequelizejs.com)写的详细全面，但是使用的时候，对于多表联合查询的使用一直有点模糊，今天花时间研究了下，特此记录总结。

## 问题
`Sequelize`框架关于多表联合查询有很多概念，如：`BelongsTo`、`HasOne`、`hasMany`、`belongsToMany`，即用于解决`1:n`和`n:m`问题。但是这些如何使用？`BelongsTo`和`HasOne`有什么区别？有时候明明安装文档使用但为什么一直未生效？下面来详细解释下用法及注意事项。

### 





## 踩过的坑

### 多表查询中要定义keywords 和 keywords2两次才能查询出全部keywords字段。。。

### findAndCountAll统计不准确
这个count不准确，需要加入distinct。参考https://github.com/sequelize/sequelize/issues/4042


### `camelCase` vs `underscored`
有时候我们想`model`层使用`camelCase`，数据库表使用`underscored`，但是`sequelize`目前无法完美满足要求，可以参考[issue](https://github.com/sequelize/sequelize/issues/6423)

目前的解决方案：

Step1， 全局配置：

```
{
	dialect: 'mysql',
    dialectOptions: {
      charset: 'utf8mb4',
    },
    // ...
    // 全局配置这个underscored
    define: {
      underscored: true,
    },
}
```

Step2，单个表配置：

```
sequelize.define('demo_table', ..., {
	underscored: true,
})
```
> 这里表名`demo_table`需要手动写为`underscored`形式。

虽然上述可以解决很多字段，但是仍然有一些字段无法转化过来，如：`createdAt`、`updatedAt`、非自动生成的`foreignKeys`。`Sequelize 5.x`正在着手解决这个问题，具体讨论可以查看[underscore / camelcase option should control all fields and table name generation](https://github.com/sequelize/sequelize/issues/6423)。

### mysql charset 配置。
默认mysql可能会有中文乱码问题。需要设置为`utf8mb4`而非`utf8`，需要在两处地方设置：
Step1， 全局配置：
```
{
	dialect: 'mysql',
    dialectOptions: {
      charset: 'utf8mb4',
    },
    // ...
}
```

Step2，单个表配置：

```
sequelize.define('demo_table', ..., {
	charset: 'utf8mb4',
})
```
