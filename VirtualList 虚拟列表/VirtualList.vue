<template>
    <div class="container" @scroll="handleScroll" ref="containerElement" :style="{ height: `${sumRenderlistHeight}px` }">
        <div class="space" :style="{ height: `${sumListHeight}px` }"></div>
        <div class="list" ref="listElement">
            <div class="item" v-for="val in renderList" :key="val.id" :style="{ height: `${itemHeight}px` }">
                {{ val.content }}
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from 'vue';
const { list, size, itemHeight } = defineProps<{
    list: Array<any>;
    size: number;
    itemHeight: number;
}>()
let singleHeight = 1;
const bufferSize = 3; // 缓冲个数
const start = ref(0);
const sumRenderlistHeight = ref(100)
const sumListHeight = ref(100)
const listElement = ref<HTMLElement|null>()
const containerElement = ref<HTMLElement | null>();

// 按size分割得到需要渲染的列表
const renderList = computed(() => {
    return list.slice(start.value, start.value + size + bufferSize)
})

// 处理滚动，获取当前元素滚动顶部位置，设置list的相对位置
function handleScroll(){
    if(!containerElement.value || !listElement.value) return;
    const st = containerElement.value.scrollTop;
    start.value = Math.floor(st / singleHeight)
    listElement.value.style.top = `${start.value * singleHeight}px`;
    
}

// 根据list总高度设置窗口高度
onMounted(() => {
    if (listElement.value && list.length > 0) {
        const child = listElement.value.firstElementChild as HTMLElement | null;
        if (child) {
            const margin = parseFloat(getComputedStyle(child).marginTop) || 0;
            const padding = parseFloat(getComputedStyle(child).paddingTop) || 0;
            singleHeight = itemHeight + margin + padding * 2
            sumRenderlistHeight.value = (size-1) * singleHeight + margin;
            sumListHeight.value = list.length * singleHeight + margin;
        }
    }
});
</script>

<style scoped>

.container {
    display: flex;
    justify-content: center;
    width: 800px;
    background-color: antiquewhite;
    overflow-y: scroll;
    position: relative;
}

.list {
    width: 100%;
    position: absolute;
    top: 0px;
    left: 0px;
}

.item {
    background-color: pink;
    border-radius: 1em;
    padding: 10px;
    margin: 10px;
}
</style>