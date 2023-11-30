<template>
    <div class="modal" v-if="showModal">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Car Details</h5>
                    <button class="close" @click="closeModal">&times;</button>
                </div>
                <div class="modal-body">
                    <div v-if="car && !edit">
                        <div v-if="car.owner">
                            <strong>Owner:</strong>
                            <div class="modal-address">
                                {{ car.owner.first_name }} {{ car.owner.last_name }}
                            </div>
                        </div>
                        <div>
                            <strong>Address:</strong>
                            <div class="modal-address">
                                {{ car.address.address }}
                                <br /> {{ car.address.city }}
                                <br /> {{ car.address.country }}
                                <span>{{ car.address.postal_code }}</span>
                            </div>
                        </div>
                    </div>
                    <div v-if="car && edit">
                        <form @submit.prevent="submitForm(car)">
                            <div>
                                <strong>Owner:</strong>
                                <div class="modal-address">
                                    <input type="text" v-model="car.owner.first_name" :placeholder="car.owner.first_name" @input="changeOwner('first_name', $event)"/>
                                    <input type="text" v-model="car.owner.last_name" :placeholder="car.owner.last_name" @input="changeOwner('last_name', $event)" />
                                </div>
                            </div>
                            <div>
                                <strong>Address:</strong>
                                <ul class="modal-car">
                                    <li>
                                        <input type="text" v-model="car.address.address" :placeholder="car.address.address" @input="changeAddress('address', $event)"/>
                                        <input type="text" v-model="car.address.city" :placeholder="car.address.city" @input="changeAddress('city', $event)" />
                                        <input type="text" v-model="car.address.country" :placeholder="car.address.country" @input="changeAddress('country', $event)" />
                                        <input type="text" v-model="car.address.postal_code" :placeholder="car.address.postal_code" @input="changeAddress('postal_code', $event)" />
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
            car: Object,
            showModal: Boolean,
            edit: Boolean,
        },
        methods: {
            closeModal() {
                this.$emit('close-modal');
            },
            changeOwner(field, event) {
                const value = event.target.value;
                console.log('Changing Owner:', field, value);
                this.car.owner[field] = value;
            },
            changeAddress(field, event) {
                const value = event.target.value;
                console.log('Changing address:', field, value);
                this.car.address[field] = value;
            },
            submitForm(car) {
                console.log('Put request successful:', JSON.stringify(this.car));
                axios.put(`/car/${car.id}`, this.car)
                .then((response) => {
                    console.log('Put request successful:', response.data);
                })
                .catch((error) => {
                    console.error('Error:', error);
                });
                this.submitted = true;
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

    .modal-address {
        padding-bottom:12px;
    }
    .modal-car{
        list-style: none;
        padding-left:0;
        margin-bottom:0;
    }

    .modal-body {
        padding: 20px;
    }

    .modal-footer {
        text-align: right;
    }
</style>
