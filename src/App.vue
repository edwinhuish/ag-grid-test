<script setup lang="ts">
import 'ag-grid-community/styles/ag-grid.css';
import 'ag-grid-community/styles/ag-theme-alpine.css';

import { AgGridVue } from 'ag-grid-vue3';
import { reactive } from 'vue';
import type { GridOptions, ValueGetterParams } from 'ag-grid-community';


interface DemoAttribute {
  idx: number;
  quantity: number;
}


const rowData = reactive<DemoAttribute[]>([
  {
    idx: 1,
    quantity: 100
  },
  {
    idx: 2,
    quantity: 0
  },
  {
    idx: 3,
    quantity: 300
  },
  {
    idx: 4,
    quantity: 0
  },
  {
    idx: 5,
    quantity: 500
  }
]);



const gridOpts = reactive<GridOptions<DemoAttribute>>({
  columnDefs: [
    {
      field: 'idx',
      minWidth: 150,
      filter: 'agNumberColumnFilter',
    },
    {
      field: 'quantity',
      editable: params => {
        if (params.data?.idx === 2 || params.data?.idx === 4) {
          return false;
        }
        return true;
      },
      filter: 'agNumberColumnFilter',
      valueGetter: params => quantitySetter(params)
    },
  ],
  
  rowData

});


function quantitySetter(params: ValueGetterParams<DemoAttribute>) {
  if (params.data?.idx === 2) {
    const row1 = rowData.find(row => row.idx === 1)!;
    const row4 = rowData.find(row => row.idx === 4)!;
    return row1.quantity + row4.quantity;
  }

  if (params.data?.idx === 4) {
    const row1 = rowData.find(row => row.idx === 1)!;
    const row5 = rowData.find(row => row.idx === 5)!;
    return row1.quantity + row5.quantity;
  }

  return params.data?.quantity;
}


</script>

<template>
<AgGridVue v-bind="gridOpts" class="ag-theme-alpine" style="width:500px; height:400px;"></AgGridVue>
</template>
