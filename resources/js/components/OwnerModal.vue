<template>
    <div class="modal" v-if="showModal">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Owner Details</h5>
                    <button class="close" @click="closeModal">&times;</button>
                </div>
                <div class="modal-body">
                    <div v-if="owner && !edit">
                        <strong>Address:</strong>
                        <div v-if="owner.addresses != ''">
                            <div>
                                <div v-for="(address, index) in owner.addresses" :key="index">
                                    <div class="modal-address">
                                        {{ address.address }}
                                        <br /> {{ address.city }}
                                        <br /> {{ address.country }}
                                        <span>{{ address.postal_code }}</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div v-else>
                            No address on file
                        </div>
                        <div>
                            <strong>Cars:</strong>
                            <div v-if="owner.cars != ''">
                                <ul v-for="(cars, index) in owner.cars" :key="index" class="modal-car">
                                    <li>{{ cars.make }} {{ cars.model }}</li>
                                </ul>
                            </div>
                            <div v-else>
                                No cars on file
                            </div>
                        </div>
                    </div>
                    <div v-if="owner && edit">
                        <form @submit.prevent="submitForm(owner)">
                            <div>
                                <strong>Addresses:</strong>
                                <div v-if="owner.addresses != ''">
                                    <ul v-for="(address, index) in owner.addresses" :key="index" class="modal-address">
                                        <li>
                                            <input type="text" v-model="address.address" :placeholder="address.address" @input="changeAddress(index, 'address', $event)"/>
                                            <input type="text" v-model="address.city" :placeholder="address.city" @input="changeAddress(index, 'city', $event)" />
                                            <input type="text" v-model="address.country" :placeholder="address.country" @input="changeAddress(index, 'country', $event)" />
                                            <input type="text" v-model="address.postal_code" :placeholder="address.postal_code" @input="changeAddress(index, 'postal_code', $event)" />
                                        </li>
                                    </ul>
                                </div>
                                <div v-else>
                                    No address on file
                                </div>
                            </div>
                            <div>
                                <strong>Cars:</strong>
                                <div v-if="owner.cars != ''">
                                    <ul v-for="(car, index) in owner.cars" :key="index" class="modal-car">
                                        <li>
                                            <input type="text" v-model="car.make" :placeholder="car.make" @input="changeCar(index, 'make', $event)"/>
                                            <input type="text" v-model="car.model" :placeholder="car.model" @input="changeCar(index, 'model', $event)"/>
                                        </li>
                                    </ul>
                                </div>
                                <div v-else>
                                    No cars on file
                                </div>
                            </div>
                            <button v-if="owner.cars != ''" type="submit">Submit</button>
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
            owner: Object,
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
            changeAddress(index, field, event) {
                const value = event.target.value;
                console.log('Changing address:', field, value);
                this.owner.addresses[index][field] = value;
            },
            changeCar(index, field, event) {
                const value = event.target.value;
                console.log('Changing Car:', field, value);
                this.owner.cars[index][field] = value;
            },
            submitForm(owner) {
                console.log('Put request successful:', JSON.stringify(this.owner));
                axios.put(`/owner/${owner.id}`, this.owner)
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

    .modal-address, .modal-car{
        list-style: none;
        padding-left:0;
        margin-bottom:0;
        padding-bottom:12px;
    }

    .modal-footer {
        text-align: right;
    }
</style>
