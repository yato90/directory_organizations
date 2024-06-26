<script>
import { ref, computed } from 'vue';
import OrganizationTable from './components/OrganizationTable.vue';
import AddOrganizationModal from './components/AddOrganizationModal.vue';

export default {
  components: {
    OrganizationTable,
    AddOrganizationModal,
  },
  setup() {
    const organizations = ref([]);
    const searchQuery = ref('');
    const showModal = ref(false);
    const sortKey = ref('');
    const sortOrder = ref(1);//1-возрастание -1-убывание
    const currentPage = ref(1);
    const itemsPerPage = 4;

    const filteredOrganizations = computed(() => {
      return organizations.value
        .filter((org) =>
          org.director.toLowerCase().includes(searchQuery.value.toLowerCase())
        )
        .sort((a, b) => {
          if (sortKey.value) {
            let result = 0;
            if (a[sortKey.value] > b[sortKey.value]) result = 1;
            if (a[sortKey.value] < b[sortKey.value]) result = -1;
            return result * sortOrder.value;
          }
          return 0;
        });
    });

    const totalPages = computed(() => {
      return Math.ceil(filteredOrganizations.value.length / itemsPerPage);
    });

    const paginatedOrganizations = computed(() => {
      const start = (currentPage.value - 1) * itemsPerPage;
      const end = start + itemsPerPage;
      return filteredOrganizations.value.slice(start, end);
    });

    const addOrganization = (org) => {
      organizations.value.push(org);
      showModal.value = false;
    };

    const deleteOrganization = (index) => {
      const orgIndex = (currentPage.value - 1) * itemsPerPage + index;
      organizations.value.splice(orgIndex, 1);
    };

    const sortOrganizations = (key) => {
      if (sortKey.value === key) {
        sortOrder.value *= -1;
      } else {
        sortKey.value = key;
        sortOrder.value = 1;
      }
    };

    const prevPage = () => {
      if (currentPage.value > 1) {
        currentPage.value--;
      }
    };

    const nextPage = () => {
      if (currentPage.value < totalPages.value) {
        currentPage.value++;
      }
    };

    return {
      organizations,
      searchQuery,
      showModal,
      sortKey,
      sortOrder,
      currentPage,
      totalPages,
      paginatedOrganizations,
      addOrganization,
      deleteOrganization,
      sortOrganizations,
      prevPage,
      nextPage,
    };
  },
};
</script>

<template>
  <div id="app">
    <h1>Справочник организаций</h1>
    <div class="app-search">
      <input v-model="searchQuery" placeholder="Найти по ФИО" />
      <button @click="showModal = true">Добавить</button>
    </div>
    <OrganizationTable
      :organizations="paginatedOrganizations"
      @delete-organization="deleteOrganization"
      @sort="sortOrganizations"
      :sort-key="sortKey"
      :sort-order="sortOrder"
    />
    <div class="app-page">
      <button @click="prevPage" :disabled="currentPage === 1"><-</button>
      Страница {{ currentPage }}
      <button @click="nextPage" :disabled="currentPage === totalPages">-></button>
    </div>
    <AddOrganizationModal
      v-if="showModal"
      @close="showModal = false"
      @add-organization="addOrganization"
    />
  </div>
</template>

<style>
input{
  padding: 10px;
}
.app-search{
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}
.app-page{
  display: flex;
  justify-content: end;
  margin-top: 10px;
  align-items:center;
  gap: 5px;
}
.app-page button{
  border: none;
}
</style>
