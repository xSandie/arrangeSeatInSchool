<template>
  <div class="op_container">
    <el-divider content-position="left">批量创建座位</el-divider>
    <!-- <p :style="`font-size: var(--el-font-size-extra-small);margin-bottom:10px;`"></p> -->
    <el-row class="sizeInput">
      <el-col
        :span="4"
        class="size_input_title"
        :style="`font-size: var(--el-font-size-extra-small);`"
        >行</el-col
      >
      <el-col :span="20" style="display: flex; justify-content: end"
        ><el-input-number v-model="batchCreateData.rows" size="small" controls-position="right"
      /></el-col>
    </el-row>
    <el-row class="sizeInput">
      <el-col
        :span="4"
        class="size_input_title"
        :style="`font-size: var(--el-font-size-extra-small);`"
        >列</el-col
      >
      <el-col :span="20" style="display: flex; justify-content: end"
        ><el-input-number v-model="batchCreateData.lines" size="small" controls-position="right"
      /></el-col>
    </el-row>
    <el-row class="sizeInput">
      <el-col
        :span="4"
        class="size_input_title"
        :style="`font-size: var(--el-font-size-extra-small);`"
      >
        <el-tooltip
          effect="dark"
          content="多少列合并成一组，若为0则不会合并。可以和行组共同生效，形成网格的块组"
          placement="top-start"
          >列组</el-tooltip
        >
      </el-col>
      <el-col :span="20" style="display: flex; justify-content: end"
        ><el-input-number v-model="batchCreateData.groupLines" size="small" controls-position="right"
      /></el-col>
    </el-row>
    <el-row class="sizeInput">
      <el-col
        :span="4"
        class="size_input_title"
        :style="`font-size: var(--el-font-size-extra-small);`"
      >
        <el-tooltip
          effect="dark"
          content="多少行合并成一组，若为0则不会合并。可以和列组共同生效，形成网格的块组"
          placement="top-start"
          >行组</el-tooltip
        >
      </el-col>
      <el-col :span="20" style="display: flex; justify-content: end"
        ><el-input-number v-model="batchCreateData.groupRows" size="small" controls-position="right"
      /></el-col>
    </el-row>
    <el-row class="sizeInput">
      <el-col
        :span="6"
        class="size_input_title"
        :style="`font-size: var(--el-font-size-extra-small);`"
      >
        <el-tooltip
          effect="dark"
          content="组内成员之间的间距，0则为没间距"
          placement="top-start"
          >邻座距</el-tooltip
        >
      </el-col>
      <el-col :span="18" style="display: flex; justify-content: end"
        ><el-input-number v-model="batchCreateData.marginBetween" size="small" controls-position="right"
      /></el-col>
    </el-row>
    <el-row>
      <el-col :span="24"
        ><el-button @click="handleBatchCreateSeats" type="primary" class="full_btn" size="small">创建</el-button></el-col
      >
    </el-row>
    <arrange-seat-area :graph="graph"></arrange-seat-area>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'
import { generateSeats } from './batchCreate/utils';
import type { Graph } from '@antv/x6';
import ArrangeSeatArea from './arrangeSeat/index.vue'

const batchCreateData = ref({
  lines: 0,
  rows: 0,
  groupLines: 0,
  groupRows: 0,
  marginBetween: 10
})

const props = defineProps<{
  graph: Graph
}>()

const handleBatchCreateSeats = () => {
  const seat = props.graph.addNode({
    x:-300,
    y:-300,
    shape: 'seat-node',
    attrs: {
      seatKey: {
        text: "样例",
        wordSpacing: '-5px',
        letterSpacing: 0
      },
      name: {
        text: "生成后删除",
        fontSize: 16,
        // fontFamily: 'Arial',
        letterSpacing: 0
      }
    }
  })
  generateSeats(props.graph, batchCreateData.value, seat, batchCreateData.value.marginBetween)
  props.graph.removeCell(seat)
}

</script>

<style lang="scss" scoped>
.op_container {
  box-sizing: border-box;
  width: 100%;
  height: auto;
  // padding-top: 10px;
  padding-left: calc(var(--el-menu-base-level-padding));
  padding-right: 10px;
  padding-bottom: 10px;
}
.sizeInput {
  margin-bottom: 10px;
}
.size_input_title {
  box-sizing: border-box;
  width: 100%;
  // height: 100%;
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
.full_btn {
  width: 100%;
}
</style>
