<template>
    <div>
        <ViewButton @view-clicked="viewClicked" />
        <EditButton @edit-clicked="editClicked" />
        <DeleteButton @delete-clicked="deleteClicked" />
    </div>
</template>

<script>
import ViewButton from "./ViewButton.vue";
import EditButton from "./EditButton.vue";
import DeleteButton from "./DeleteButton.vue";
export default {
    components: {
        ViewButton,
        EditButton,
        DeleteButton,
    },
    props: {
        row: {
            type: Object
        }
    },
    methods: {
        viewClicked() {
            this.$parent.$parent.$emit('view-clicked-row', this.row);
        },
        editClicked() {
            this.$parent.$parent.$emit('edit-clicked-row', this.row);
        },
        deleteClicked() {
            this.$parent.$parent.$emit('delete-clicked-row', this.row);
        },
    },
}
</script>
