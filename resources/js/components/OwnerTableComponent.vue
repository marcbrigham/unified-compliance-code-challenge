<template>
    <div>
        <bootstrap-4-datatable
            :columns="columns"
            :data="rows"
            :filter="filter"
            :per-page="perPage"
            @view-clicked-row="viewRow"
            @edit-clicked-row="editRow"
            @delete-clicked-row="deleteRow"
            class="table-bordered"
        />
        <bootstrap-4-datatable-pager
            v-model="page"
            type="abbreviated"
        />
    </div>
</template>

<script>
import TableButtonsComponent from "./TableButtonsComponent";

export default {
    data() {
        return {
            columns: [
                {
                    label: 'ID',
                    field: 'id',
                    align: 'center'
                },
                {
                    label: 'First Name',
                    field: 'first_name',
                    headerAlign: 'left',
                    align: 'left'
                },
                {
                    label: 'Last Name',
                    field: 'last_name',
                    headerAlign: 'left',
                    align: 'left'
                },
                {
                    label: '# of Addresses',
                    field: 'addresses_count',
                    headerAlign: 'left',
                    align: 'left',
                },
                {
                    label: '# of Cars',
                    field: 'cars_count',
                    headerAlign: 'left',
                    align: 'left',
                },
                {
                    label: 'Actions',
                    headerAlign: 'right',
                    align: 'right',
                    component: TableButtonsComponent
                },
            ],
            rows: [],
            page: 1,
            filter:  '',
            perPage: 20,
            loading: true
        }
    },
    methods: {
        showOwners: function () {
            axios.get('/owner').then((res) => {
                this.rows = res.data.map(o => ({...o, 'type': 'owner'}));
            });
        },
        deleteRow(rows) {
            const response = confirm("Are you sure you want to delete?");
            const rowIndex = this.rows.findIndex(item => item.id === rows.id);
            if (rowIndex !== -1 && response) {
                this.rows.splice(rowIndex, 1);
            }
        }
    },
    created: function () {
        this.showOwners()
    }
}
</script>
