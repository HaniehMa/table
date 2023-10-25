<template>
  <form class="w-1/2">
    <label
      for="search"
      class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white"
      >Search</label
    >
    <div class="relative">
      <div
        class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none"
      >
        <svg
          class="w-4 h-4 text-gray-500 dark:text-gray-400"
          aria-hidden="true"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 20 20"
        >
          <path
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"
          />
        </svg>
      </div>
      <input
        v-model="search"
        type="search"
        id="search"
        class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
        placeholder="Search"
        required
      />
    </div>
  </form>
  <div class="relative overflow-x-auto rounded-lg">
    <table
      class="w-full text-sm text-left text-gray-500 dark:text-gray-400 rounded-lg my-4 overflow-hidden"
    >
      <thead
        class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
      >
        <tr>
          <template v-for="{ key, label, sortable } in fields" :key="key">
            <th
              class="px-6 py-3 flex content-center"
              v-if="sortable"
              @click="setSort(key)"
            >
              {{ label }}
              <template v-if="sortBy === key">
                <span
                  ><svg
                    class="w-3 h-3 ml-1.5"
                    aria-hidden="true"
                    xmlns="http://www.w3.org/2000/svg"
                    fill="currentColor"
                    viewBox="0 0 24 24"
                  >
                    <path
                      d="M8.574 11.024h6.852a2.075 2.075 0 0 0 1.847-1.086 1.9 1.9 0 0 0-.11-1.986L13.736 2.9a2.122 2.122 0 0 0-3.472 0L6.837 7.952a1.9 1.9 0 0 0-.11 1.986 2.074 2.074 0 0 0 1.847 1.086Zm6.852 1.952H8.574a2.072 2.072 0 0 0-1.847 1.087 1.9 1.9 0 0 0 .11 1.985l3.426 5.05a2.123 2.123 0 0 0 3.472 0l3.427-5.05a1.9 1.9 0 0 0 .11-1.985 2.074 2.074 0 0 0-1.846-1.087Z"
                    /></svg
                ></span>
              </template>
            </th>
            <th v-else class="px-6 py-3">
              {{ label }}
            </th>
          </template>
        </tr>
      </thead>
      <tbody class="rounded-lg">
        <tr
          class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 rounded-lg"
          v-for="item in paginatedItems"
          :key="item.id"
        >
          <td class="px-6 py-4" v-for="{ key } in fields" :key="key">
            {{ item[key] }}
          </td>
        </tr>
      </tbody>
    </table>
    <nav aria-label="Page navigation example">
      <ul class="flex items-center -space-x-px h-10 text-base">
        <li>
          <button
            @click="pagination.currentPage--"
            class="flex items-center justify-center px-4 h-10 ml-0 leading-tight text-gray-500 bg-white border border-gray-300 rounded-l-lg hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white"
          >
            <span class="sr-only">Previous</span>
            <svg
              class="w-3 h-3"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 6 10"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M5 1 1 5l4 4"
              />
            </svg>
          </button>
        </li>
        <li v-for="(page, index) in paginationButtons" :key="index">
          <button
            @click="pagination.currentPage = page"
            :class="
              pagination.currentPage == page
                ? 'z-10 flex items-center justify-center px-3 h-8 leading-tight text-blue-600 border border-blue-300 bg-blue-50 hover:bg-blue-100 hover:text-blue-700 dark:border-gray-700 dark:bg-gray-700 dark:text-white'
                : 'flex items-center justify-center px-4 h-10 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white'
            "
          >
            {{ page }}
          </button>
        </li>
        <li>
          <button
            @click="pagination.currentPage++"
            class="flex items-center justify-center px-4 h-10 leading-tight text-gray-500 bg-white border border-gray-300 rounded-r-lg hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white"
          >
            <span class="sr-only">Next</span>
            <svg
              class="w-3 h-3"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 6 10"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m1 9 4-4-4-4"
              />
            </svg>
          </button>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script setup>
const props = defineProps({
  fields: { type: Array, required: true },
  items: { type: Array, required: true },
});
const router = useRouter();
const route = useRoute();
const sortBy = ref(null);
const sortDesc = ref(null);
const pagination = reactive({
  currentPage: 1,
  perPage: 20,
  totalPages: computed(() =>
    Math.ceil(props.items.length / pagination.perPage)
  ),
});

const sortedItems = computed(() => {
  if (sortDesc.value === null) return searchedItems.value.value;
  if (sortDesc.value) {
    router.push({ query: { sort: "Desc" } });
    return searchedItems.value.value.sort((firstItem, secondItem) => {
      const dateA = new Date(firstItem.date);
      const dateB = new Date(secondItem.date);
      return dateB - dateA;
    });
    // return sort(props.items).desc(sortBy.value);
  } else {
    return searchedItems.value.value.sort((firstItem, secondItem) => {
      const dateA = new Date(firstItem.date);
      const dateB = new Date(secondItem.date);
      return dateA - dateB;
    });
    // return sort(props.items).asc(sortBy.value);
  }
});
const rangeArray = (a, b) => {
  // if only one argument supplied then return random number between 1 and argument
  if (b === undefined) {
    b = a;
    a = 1;
  }
  return [...Array(b - a + 1).keys()].map((x) => x + a);
};
const paginationButtons = computed(() => {
  const { currentPage } = pagination;
  if (currentPage > 6) return rangeArray(currentPage - 6, currentPage + 6);
  else return rangeArray(currentPage, currentPage + 12);
});
const paginatedItems = computed(() => {
  const { currentPage, perPage } = pagination;
  const start = (currentPage - 1) * perPage;
  const stop = start + perPage;
  return sortedItems.value.slice(start, stop);
});
const setSort = (key) => {
  if (sortBy.value === key) {
    if (sortDesc.value === true) sortDesc.value = null;
    else if (sortDesc.value === false) sortDesc.value = true;
    else sortDesc.value = false;
  } else {
    sortBy.value = key;
    sortDesc.value = false;
  }
};
let search = ref("");
let searchedItems = ref([]);
watch(
  search,
  (newValue) => {
    searchedItems.value = computed(() => {
      return props.items.filter((row) => {
        return (
          row.name.toLowerCase().includes(newValue.toLowerCase()) ||
          row.date.toLowerCase().includes(newValue.toLowerCase()) ||
          row.title.toLowerCase().includes(newValue.toLowerCase())
        );
      });
    });
  },
  { immediate: true }
);
onUpdated(() =>
  router.push({
    query: {
      sortDesc: sortDesc.value,
      page: pagination.currentPage,
      search: search.value,
    },
  })
);
onMounted(()=>{
search.value = route.query.search
pagination.currentPage =route.query.page
sortDesc.value =route.query.sortDesc
})
</script>
