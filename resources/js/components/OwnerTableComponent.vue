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
        <owner-modal
            :owner="selectedOwner"
            :show-modal="showModal"
            :edit="editingMode"
            @close-modal="closeModal"
        />
    </div>
</template>

<script>
import TableButtonsComponent from "./TableButtonsComponent";
import OwnerModal from "./OwnerModal.vue";
export default {
    components: {
        OwnerModal,
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
            loading: true,
            selectedOwner: null,
            showModal: false,
            editingMode: false,
        }
    },
    methods: {
        showOwners: function () {
            axios.get('/owner').then((res) => {
                this.rows = res.data.map(o => ({...o, 'type': 'owner'}));
            });
        },
        editRow(row) {
            this.openModal(row, true);
        },
        viewRow(row) {
            this.openModal(row, false);
        },
        openModal(row, isEditing) {
            this.editingMode = isEditing;
            axios.get(`/owner/${row.id}`)
                .then((res) => {
                    this.selectedOwner = res.data;
                    this.showModal = true;
                    document.body.classList.add('modal-open');
                })
                .catch((error) => {
                    console.error('Error fetching owner data:', error);
                });

            this.showModal = true;
            document.body.classList.add('modal-open');
        },
        deleteRow(rows) {
            const response = confirm("Are you sure you want to delete?");
            const rowIndex = this.rows.findIndex(item => item.id === rows.id);
            if (rowIndex !== -1 && response) {
                axios.delete(`/owner/${rows.id}`);
                this.rows.splice(rowIndex, 1);
            }
        },
        closeModal() {
            this.showModal = false;
            document.body.classList.remove('modal-open');
        },
    },
    created: function () {
        this.showOwners()
    }
}
</script>
