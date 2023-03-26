<template>
    <div ref="lightbox" class="fixed top-0 z-[10] left-0 w-full h-full transition-all ease-in before:absolute before:top-0 before:left-0 before:w-full before:h-full before:bg-slate-800 before:bg-blend-multiply before:opacity-70">
        <div class="absolute top-[50%] left-[50%] translate-x-[-50%] translate-y-[-50%] w-full lg:w-[calc(100%-347px)]">
            <button @click="closeOverlay" class="absolute right-[-16px] top-[-48px] text-white">
                <i class="fas fa-times text-4xl"></i>
            </button>
            <div class="bg-white w-full rounded-2xl h-[500px] overflow-auto flex flex-wrap content-center items-center px-8 py-4">
                <h2 class="font-montserrat text-3xl mb-8 font-bold">
                    {{ actionType }}
                </h2>
                <div class="w-full">
                    <div class="flex flex-wrap lg:flex-nowrap mb-6 w-full">
                        <div class="lg:mr-6 w-full">
                            <label class="font-montserrat">Product Name</label>
                            <input required type="text" :value="data && data.title" ref="title" @input="title = $event.target.value" class="border-b border-b-slate-800 rounded-none px-4 font-montserrat text-xl text-slate-900 h-fit py-4 w-full outline-none">
                        </div>
                        <div class="w-full">
                            <label class="font-montserrat">Product Price</label>
                            <input required type="text" :value="data && data.price" ref="price" @input="price = $event.target.value" class="border-b border-b-slate-800 rounded-none px-4 font-montserrat text-xl text-slate-900 h-fit py-4 w-full outline-none">
                        </div>
                    </div>
                    <div class="w-full">
                        <label class="font-montserrat">Product Description</label>
                        <textarea required :value="data && data.description" ref="description" @input="description = $event.target.value" class="border-b border-b-slate-800 rounded-none px-4 font-montserrat text-xl text-slate-900 h-fit py-4 w-full outline-none"></textarea>
                    </div>
                </div>
                <button
                type="submit"
                @click="submitUpdatedValues"
                class="border-0 bg-slate-900 text-white px-6 py-6 font-montserrat font-medium rounded-xl mt-6">
                    Submit
                </button>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        data: {
            type: [Object, String],
            default: {}
        },
        actionType: {
            type: String,
            default: ''
        },
        lists: {
            type: Object,
            default: {}
        }
    },
    data: () => ({
        title: '',
        description: '',
        price: '',
        id: 20
    }),
    methods: {
        closeOverlay() {
            this.$refs.title.value = '';
            this.$refs.description.value = '';
            this.$refs.price.value = '';
            this.$emit('emitCloseModal', false);
        },
        submitUpdatedValues() {
            this.id += 1;
            this.$emit('emitModalValues', 
                {
                    id: this.data.id ? this.data.id : this.id,
                    title: this.title !== '' ? this.title : this.data.title,
                    price: this.price !== '' ? this.price : this.data.price,
                    description: this.description !== '' ? this.description : this.data.description,
                    action: this.actionType
                }
            )
            this.closeOverlay();
        }
    },
}
</script>