<template>
    <div class="overflow-auto">
        <table class="w-full lg:table-fixed">
            <tr class="border border-black rounded-sm">
                <th
                v-for="(heading, index) in data.headings"
                :key="index"
                class="text-left font-montserrat text-lg text-blue-900 py-2 px-2">
                    {{ heading }}
                </th>
            </tr>
            <tr 
            v-for="(product, index) in data.products"
            :key="index"
            class="border border-black rounded-sm">
                <td class="text-left font-montserrat text-base text-slate-800 py-2 px-2">
                    {{ product.id }}
                </td>
                <td class="text-left font-montserrat text-base text-slate-800 py-2 px-2">
                    {{ product.title }}
                </td>
                <td class="text-left font-montserrat text-base text-slate-800 py-2 px-2">
                    {{ product.price }}
                </td>
                <td class="text-left font-montserrat text-xs leading-full text-slate-800 py-2 px-2">
                    {{ product.description }}
                </td>
                <td class="py-2 px-2">
                    <i class="fas fa-edit mr-4 cursor-pointer" @click="emitUpdateProduct(product.id)"></i>
                    <i class="fas fa-trash-alt cursor-pointer" @click="emitDeleteProduct(product.id)"></i>
                </td>
            </tr>
        </table>
    </div>
</template>
<script>
export default {
    props: {
        data: {
            type: Object,
            default: () => {
                return {}
            }
        }
    },
    methods: {
        emitUpdateProduct(id) {
            this.$emit('emitUpdateProduct', id);
        },
        emitDeleteProduct(id) {
            this.$emit('emitDeleteProduct', id);
        }
    }
}
</script>