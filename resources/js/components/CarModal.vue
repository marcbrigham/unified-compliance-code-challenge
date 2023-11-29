<template>
    <div class="modal" v-if="showModal">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Car Details</h5>
                    <button class="close" @click="closeModal">&times;</button>
                </div>
                <div class="modal-body">
                    <p>
                        <strong>Owner:</strong>
                        {{ car.owner_name }}
                    </p>
                    <p>
                        <strong>Address:</strong>
                        <br /> {{ car.address }}
                        <br /> {{ car.city }}
                        <br /> {{ car.country }}
                        <br /> {{ car.postal_code }}
                    </p>
                </div>
                <div class="modal-footer">
                    <button class="btn btn-primary" @click="closeModal">Close</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            car: Object,
            showModal: Boolean,
        },
        methods: {
            closeModal() {
                this.$emit('close-modal');
            },
        },
    };
</script>

<style scoped>
    .modal {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.5);
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 999;
        overflow: hidden;
    }
    .modal-open {
        overflow: hidden;
    }

    .modal-dialog {
        background: white;
        border-radius: 5px;
        box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
        min-width: 320px;
    }

    .modal-content {
        padding: 20px;
    }

    .modal-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .close {
        font-size: 24px;
        cursor: pointer;
    }

    .modal-body {
        padding: 20px;
    }

    .modal-footer {
        text-align: right;
    }
</style>
