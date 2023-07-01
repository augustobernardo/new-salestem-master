<script setup lang="ts">
// import Todo from './Todo.vue';
import Profile from "../../assets/img/people.png";

</script>

<template>
    <div class="table-data">
        <table class="table">
            <thead>
                <tr>
                    <th v-for="(column, index) in columns" :key="index">{{ column }}</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index) in displayedItems" :key="item.id" :class="{ 'row-editing': item.editavel }">
                    <td v-for="(column, colIndex) in columns" :key="colIndex">
                        <input v-model="item[column]" :disabled="!item.editavel" class="input-field" />
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import jsonData from '../vendas/VendaData.json'; // Importar o arquivo JSON com os dados

export default defineComponent({
    data() {
        return {
            items: [],
            itemsPerPage: 5,
            currentPage: 1,
            adicionarLinhaVisivel: false,
            novoItem: {},
            columns: [],
            linhasEmEdicao: [],
        };
    },
    computed: {
        displayedItems() {
            // const startIndex = (this.currentPage - 1) * this.itemsPerPage;
            // const endIndex = startIndex + this.itemsPerPage;

            const dadosLocalStorage = JSON.parse(localStorage.getItem('tabelaDadosVendas'));

            const storagelenght = dadosLocalStorage.length;

            const startIndex = storagelenght - 5;
            const endIndex = storagelenght;

            return this.items.slice(startIndex, endIndex);
        },
        totalPages() {
            return Math.ceil(this.items.length / this.itemsPerPage);
        },
    },
    created() {
        const dadosLocalStorage = localStorage.getItem('tabelaDadosVendas');

        // this.items = JSON.parse(dadosLocalStorage);

        if (dadosLocalStorage) {
            this.items = JSON.parse(dadosLocalStorage);
        } else {
            this.items = jsonData;
            this.salvarLocalStorage();
        }

        if (this.items.length > 0) {
            this.columns = Object.keys(this.items[0]).filter((column) => column !== 'id' && column !== 'editavel' && column !== 'copia');
        }
    },
    methods: {
    salvarLocalStorage() {
      localStorage.setItem('tabelaDadosVendas', JSON.stringify(this.items));
    },
    },

});
</script>
<style scoped>
 .table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 16px;
}

th,
td {
    padding: 12px;
    text-align: left;
    border-bottom: 1px solid #ddd;
}

.row-editing {
    background-color: #f9f9f9;
}

.input-field {
    width: 100%;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    color: var(--dark);
}
</style>
