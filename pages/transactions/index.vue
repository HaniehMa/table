<template>
    <div class="px-4 py-4 bg-gray-950">
        <data-table :fields="fields" :items="data.body"></data-table> 

    </div>
</template>
<script setup>
const fields = [
  { key: 'name', label: 'Name'},
  { key: 'date', label: 'Date', sortable: true },
  { key: 'title', label: 'Title' },
  { key: 'field', label: 'Field' },
  { key: 'old_value', label: 'oldValue' },
  { key: 'new_value', label: 'newValue' },

];
const { data } = await useLazyAsyncData('records', () => queryContent('records').findOne())
</script>