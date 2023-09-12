---
title: 解决el-table无限增高问题
date: 2023-09-12 11:42:48
categories:
- 前端
---
# 大纲

- 问题原因
- 解决方案
- 回顾反思

<!-- more -->

# 问题原因

```html
<template>
  <div class="el-table-demo">
    <div class="demo-header"></div>
    <!-- 问题原因：表格容器中若有一个容器，如demo-table-header，再设置el-table为height:100%，则el-table会无限增高 -->
    <!-- 解决：将el-table的height设置为 calc(100% - 30px); -->
    <div class="demo-table">
      <div class="demo-table-header"></div>
      <!-- height="calc(100% - 30px)" -->
      <!-- height="100%" -->
      <el-table :data="tableData" height="calc(100% - 30px)" style="width: 100%">
        <el-table-column prop="date" label="日期" width="180"></el-table-column>
        <el-table-column prop="name" label="姓名" width="180"></el-table-column>
        <el-table-column prop="address" label="地址"></el-table-column>
      </el-table>
    </div>
    <div class="demo-footer"></div>
  </div>
</template>

<script>
import { tableDataDemo } from './data/tableData';

export default {
  name: 'ElTableDemo',
  data() {
    return {
      tableData: [],
    }
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      setTimeout(() => {
        this.tableData = tableDataDemo;
      }, 1000);
    },
  },
}
</script>

<style>
.el-table-demo {
  height: 700px;
  display: flex;
  flex-direction: column;
}
.demo-header {
  height: 260px;
  background-color: darkgray;
}
/* table 部分布局 */
.demo-table-header {
  height: 30px;
  background-color: brown;
}
.demo-table {
  flex: 1;
  /* height: 360px; */
  outline: 2px solid green;
  /* overflow: auto; */
}

.demo-footer {
  height: 60px;
  background-color: grey;
}
</style>
```

div.demo-table 设置为flex: 1，并且在此div中，包含两部分，div.demo-table-header 和 el-table，其中 div.demo-table-header 设置高度为30px，若 el-table 设置 height 属性为 100%，则 el-table 渲染的时候，会无限制的自动增高。

# 解决方案

```html
<el-table :data="tableData" height="calc(100% - 30px)" style="width: 100%">
```

将 el-table 的高度设置为：100% - 30px，即：容器总高度 - div.demo-table-header高度。

# 回顾反思

此类现象在开发中遇到过几次，每次都不太清楚问题的根原在那里。只是反复修改样式尝试解决，不求深入刨析原因。改进方案：遇到问题先求快速度解决，但应把需要深究的问题点记录到TODO中，待合适的时间将问题研究透彻，并总结要点。