<script>
export default {
    props: {
        organizations: Array,
        sortKey: String,
        sortOrder: Number,
    },
    emits: ['delete-organization', 'sort'],
    setup(props, { emit }) {
        const sort = (key) => {
            emit('sort', key);
        };

        return {
            sort,
        };
    },
};
</script>

<template>
    <table>
        <thead>
            <tr>
                <th class="sort" @click="sort('name')">
                    Название
                    <span v-if="sortOrder == -1">↑</span>
                    <span v-if="sortOrder == 1">↓</span>
                </th>
                <th class="sort" @click="sort('director')">
                    ФИО директора
                    <span v-if="sortOrder == -1">↑</span>
                    <span v-if="sortOrder == 1">↓</span>
                </th>
                <th>Номер телефона</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(org, index) in organizations" :key="index">
                <td>{{ org.name }}</td>
                <td>{{ org.director }}</td>
                <td>{{ org.phone }}</td>
                <td><button class="close" @click="$emit('delete-organization', index)">☓</button></td>
            </tr>
        </tbody>
    </table>
</template>

<style scoped>
table{
    width: 100%;
}
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
.sort{
    cursor: pointer;
}
.close{
    border:none;
}
</style>
  