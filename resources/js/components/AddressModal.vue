<template>
    <div class="modal" v-if="showModal">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Address Details</h5>
                    <button class="close" @click="closeModal">&times;</button>
                </div>
                <div class="modal-body">
                    <div v-if="address && !edit">
                        <div v-if="address.owner">
                            <strong>Owner:</strong>
                            <div class="modal-address">
                                {{ address.owner.first_name }} {{ address.owner.last_name }}
                            </div>
                        </div>
                        <div>
                            <strong>Cars:</strong>
                            <ul v-for="(cars, index) in address.cars" :key="index" class="modal-car">
                                <li>{{ cars.make }} {{ cars.model }}</li>
                            </ul>
                        </div>
                    </div>
                    <div v-if="address && edit">
                        <form @submit.prevent="submitForm(address)">
                            <div>
                                <strong>Owner:</strong>
                                <div class="modal-address">
                                    <input type="text" v-model="address.owner.first_name" :placeholder="address.owner.first_name" @input="changeOwner('first_name', $event)"/>
                                    <input type="text" v-model="address.owner.last_name" :placeholder="address.owner.last_name" @input="changeOwner('last_name', $event)" />
                                </div>
                            </div>
                            <div>
                                <strong>Cars:</strong>
                                <ul v-for="(car, index) in address.cars" :key="index" class="modal-car">
                                <li>
                                    <input type="text" v-model="car.make" :placeholder="car.make" @input="changeCar(index, 'make', $event)"/>
                                    <input type="text" v-model="car.model" :placeholder="car.model" @input="changeCar(index, 'model', $event)" />
                                </li>
                                </ul>
                            </div>
                            <button type="submit">Submit</button>
                        </form>
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
            address: Object,
            showModal: Boolean,
            edit: Boolean,
        },
        data() {
            return {
                submitted: false,
            }
        },
        methods: {
            closeModal() {
                this.$emit('close-modal');
            },
            changeOwner(field, event) {
                const value = event.target.value;
                console.log('Changing Owner:', field, value);
                this.address[field] = value;
            },
            changeCar(index, field, event) {
                const value = event.target.value;
                console.log('Changing Car:', field, value);
                this.address.cars[index][field] = value;
            },
            submitForm(address) {
                axios.put(`/owner/${address.id}`, this.address)
                .then((response) => {
                    console.log('Put request successful:', response.data);
                })
                .catch((error) => {
                    console.error('Error:', error);
                });

                this.submitted = true;
            },
        }
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
