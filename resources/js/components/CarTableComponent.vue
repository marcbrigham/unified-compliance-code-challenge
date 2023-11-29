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
        <car-modal :car="selectedCar" :show-modal="showModal" @close-modal="closeModal" />
    </div>
</template>

<script>
import TableButtonsComponent from "./TableButtonsComponent";
import CarModal from "./CarModal.vue";

export default {
    components: {
        CarModal,
    },
    data() {
        return {
            columns: [
                {
                    label: 'ID',
                    field: 'id',
                    align: 'center'
                },
                {
                    label: 'Make',
                    field: 'make',
                    headerAlign: 'left',
                    align: 'left'
                },
                {
                    label: 'Model',
                    field: 'model',
                    headerAlign: 'left',
                    align: 'left'
                },
                {
                    label: 'Year',
                    field: 'year',
                    headerAlign: 'left',
                    align: 'left'
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
                    align: 'left'
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
            loading: true,
            selectedCar: null,
            showModal: false,
        }
    },
    methods: {
        showCars: function () {
            axios.get('/car').then((res) => {
                this.rows = res.data.map((item) => ({
                    id: item.id,
                    make:item.make,
                    model: item.model,
                    year: item.year,
                    city: item.address.city,
                    country: item.address.country,
                    postal_code: item.address.postal_code,
                    address: item.address.address,
                    owner_name: item.owner.first_name + ' ' + item.owner.last_name,
                }));
            });
        },
        editRow(car) {
            this.selectedCar = car;
            this.showModal = true;
            document.body.classList.add('modal-open');
        },
        viewRow(car) {
            this.selectedCar = car;
            this.showModal = true;
            document.body.classList.add('modal-open');
        },
        deleteRow(rows) {
            const response = confirm("Are you sure you want to delete?");
            const rowIndex = this.rows.findIndex(item => item.id === rows.id);
            if (rowIndex !== -1 && response) {
                this.rows.splice(rowIndex, 1);
            }
        },
        closeModal() {
            this.showModal = false;
            document.body.classList.remove('modal-open');
        },
    },
    created: function () {
        this.showCars()
    }
}
</script>
