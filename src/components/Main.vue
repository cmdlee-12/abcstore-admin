<template>
    <section class="max-w-screen-xl mx-auto py-10 lg:py-20 px-4">
        <div class="w-full justify-between flex items-center mb-6">
            <h1 class="font-montserrat font-bold text-5xl text-blue-900 w-full">
                {{ heading }}
            </h1>
            <button 
            @click="openModal"
            class="border-0 bg-slate-900 text-white px-6 py-4 font-montserrat font-medium rounded-xl w-full lg:max-w-[300px]">Add items</button>  
        </div>
        <Lists
        :data="lists"
        @emitUpdateProduct="emitUpdateProduct"
        @emitDeleteProduct="emitDeleteProduct"
        />
        <Modal
        v-show="isModalOpen"
        :data="actionType === 'Update' ? activeProductDetails : ''"
        :lists="lists"
        :actionType="actionType"
        @emitCloseModal="emitCloseModal"
        @emitModalValues="emitModalValues"
        />
    </section>
</template>
<script>
import axios from 'axios';
import Lists from './Lists.vue';
import Modal from './Modal.vue';
export default {
    components: {
        Lists,
        Modal
    },
    data: () => ({
        heading: 'Product Listing',
        lists: {
            headings: ['ID', 'Item Name', 'Price', 'Description', 'Actions'],
            products: []
        },
        isModalOpen: false,
        activeProductDetails: {},
        actionType: ''
    }),
    methods: {
        /**
         * Get all products and shows it on the table
         */
        async getProducts() {
            await axios
                .get(`https://fakestoreapi.com/products`)
                .then(res => {
                    this.lists.products = res.data;
                }).catch( err => {
                    console.log('Error getting the products: ' +err);
                });
        },
        /**
         * Opens the modal and gets the specific value to be added on the fields
         */
        async emitUpdateProduct(id) {
            this.isModalOpen = true;
            this.actionType = 'Update';

            this.getProductByCall(id);
        },
        /**
         * Deletes the product on the API and on the table *for viewing purposes only
         */
        async emitDeleteProduct(id) {
            await axios
                .delete(`https://fakestoreapi.com/products/${id}`)
                .then(res => {
                    this.lists.products = this.lists.products.filter((product) => {
                        return product.id !== id;
                    });              
                }).catch( err => {
                    console.log('Error deleting the product: ' +err);
                });
        },
        /**
         * Calls a specific product
         */
        async getProductByCall(id) {
            await axios
                .get(`https://fakestoreapi.com/products/${id}`)
                .then(res => {
                    this.activeProductDetails = res.data;
                }).catch( err => {
                    console.log('Error getting the specific product: ' +err);
                });
        },
        /**
         * Updates an item
         */
        async emitModalValues(data) {
            if(data.action === 'Update') {
                let _formData = JSON.stringify({
                    title: data.title,
                    price: data.price,
                    description: data.description
                });
    
                await axios.put(`https://fakestoreapi.com/products/${data.id}`, {
                    body: _formData
                    }).then(res => {
                        console.log(res) 
                    }).catch( err => {
                        console.log('Error updating the specific product: ' +err);
                    });
            }else {
                let _formData = JSON.stringify({
                    title: data.title,
                    price: data.price,
                    description: data.description
                });

                await axios.post(`https://fakestoreapi.com/products`, {
                    body: _formData
                    }).then(res => {
                        this.lists.products.push({
                            id: data.id,
                            title: data.title,
                            price: data.price,
                            description: data.description,
                            category: '',
                            image: ''
                        });
                    }).catch( err => {
                        console.log('Error adding the specific product: ' +err);
                    });
            }
        },
        /**
         * Opens the modal when the users adds an item
         */
        openModal() {
            this.isModalOpen = true;
            this.actionType = 'Add';
        },
        /**
         * Closes the modal
         */
        emitCloseModal(value) {
            this.isModalOpen = value;
        },
    },
    created() {
        this.getProducts();
    }
}
</script>
