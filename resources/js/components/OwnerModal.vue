<template>
    <div class="modal" v-if="showModal">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Owner Details</h5>
                    <button class="close" @click="closeModal">&times;</button>
                </div>
                <div class="modal-body">
                    <div v-if="owner">
                        <strong>Address:</strong>
                        <div v-for="(address, index) in owner.addresses" :key="index">
                            <div class="modal-address">
                                {{ address.address }}
                                <br /> {{ address.city }}
                                <br /> {{ address.country }}
                                <span>{{ address.postal_code }}</span>
                            </div>
                        </div>
                        <div>
                            <strong>Cars:</strong>
                            <ul v-for="(cars, index) in owner.cars" :key="index" class="modal-car">
                                <li>{{ cars.make }} {{ cars.model }}</li>
                            </ul>
                        </div>
                    </div>

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
            owner: Object,
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
        width: 50%;
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

    .modal-address {
        padding-bottom:12px;
    }
    .modal-car{
        list-style: none;
        padding-left:0;
        margin-bottom:0;
    }

    .modal-footer {
        text-align: right;
    }
</style>
