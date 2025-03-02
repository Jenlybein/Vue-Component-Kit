# Vue-Component-Kit

---

## Pagination 分页器

![image-20250302130250380](./assets/image-20250302130250380.png)

**预览地址**：[Vite App (jenlybein.github.io)](https://jenlybein.github.io/Vue-Component-Kit/Pagination 分页器/dist/)

**组件内容**：

- Pagination.vue
- Pagination.css

**使用示例**：

```vue
<template>
  <div>
    <Pagination :currentPage="PaginationData.currentPage" :totalPages="PaginationData.totalPages"
      :buttonCount="PaginationData.buttonCount" />
  </div>
</template>

<script setup lang="ts">
import Pagination from '@/components/Pagination.vue';
import { ref, watch } from 'vue';

// 绑定当前页和总页数
const PaginationData = ref({
  currentPage: 1,
  totalPages: 20,
  buttonCount: 6,
})

// 监听 currentPage 的变化
watch(() => PaginationData.value.currentPage, (newValue, oldValue) => {
  // ...
});
</script>
```
