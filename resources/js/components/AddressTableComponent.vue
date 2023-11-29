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
                    label: 'Address',
                    field: 'address',
                    headerAlign: 'left',
                    align: 'left',
                    interpolate: true,
                    representedAs: function (r) {
                        return r.address + '<br>' + r.city + '<br>' + r.country + '<br>' + r.postal_code;
                    }
                },
                {
                    label: 'Owner Name',
                    field: 'owner_name',
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
                }
            ],
            rows: [],
            page: 1,
            filter:  '',
            perPage: 20,
            loading: true
        }
    },
    methods: {
        showAddresses: function () {
            axios.get('/address').then((res) => {
                this.rows = res.data.map((item) => ({
                    id: item.id,
                    address: item.address,
                    city: item.city,
                    country: item.country,
                    postal_code: item.postal_code,
                    owner_name: item.owner.first_name + ' ' + item.owner.last_name,
                    cars_count: item.cars_count,
                }));
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
        this.showAddresses()
    }
}
</script>
