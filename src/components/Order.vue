<template>
    <div class="order w-full">
        <table class="text-left text-sm md:text-md w-full mx-5 mb-2">
            <tr>
                <td rowspan="5" class="w-1/6">
                    <img src="https://raw.githubusercontent.com/mayllonvrs/duduedaurinha-assets/master/logo-duduedaurinha-240.png" alt="Logo dudu e Daurinha" class="w-20">
                </td>
            </tr>
            <tr>
                <td class="w-2/6 pb-2"><span class="font-bold">Pedido</span></td>
                <td class="pb-2"><span class="font-bold">Cliente</span></td>
            </tr>
            <tr>
                <td><span class="font-bold">Código:</span> {{order._id.substr(19)}}</td>
                <td><span class="font-bold">Nome:</span> {{order.client.name}}</td>
            </tr>
            <tr>
                <td><span class="font-bold">Data:</span> {{format_date(order.createdAt)}}</td>
                <td><span class="font-bold">Telefone:</span> {{order.client.phone_number}}</td>
            </tr>
            <tr>
                <td><span class="font-bold">Hora:</span> {{format_hour(order.createdAt)}}</td>
                <td>
                    <span class="font-bold">Cidade: </span>{{order.client.city}}/{{order.client.state}} <br>
                    <span class="font-bold">CEP: </span> {{order.client.cep}}
                </td>
            </tr>

            <tr v-if="order.observations">
                <td colspan="3">
                    <span class="font-bold">Observações: </span> {{ order.observations }}
                </td>
            </tr>
        </table>
        <div class="font-bold pb-3">Itens do pedido</div>
        <div v-for="product in order.products" :key="product._id" class="border border-gray-400 rounded-md mb-2">
            <p class="pl-2 font-bold text-left">{{ product.reference }} -  {{ product.name }}</p>
            <table class="w-full text-center e">
                <thead>
                    <tr class="border-y border-gray-400">
                        <th class="w-5/12">Código - Tamanho</th>
                        <th class="w-4/12">Quantidade</th>
                        <th class="w-3/12 text-left">Subtotal</th>
                    </tr>
                </thead>
                <tr v-for="size in product.sizes" :key="size.size" class="border-b border-gray-300" >
                    <td class="w-4/12">
                    {{ product.reference }} - {{ size.size }}
                    </td>
                    <td class="w-4/12">
                        {{ size.quantity }}
                    </td>
                    <td class="w-4/12 text-left">
                        R$ {{format_price(size.quantity * size.value)}}
                    </td>
                </tr>
            </table>
        </div>
        <div class="total text-right p-5 font-bold text-lg">
            Total do pedido: R$ {{ format_price(order.total)}}
        </div>
    </div>
</template>
<script>
    import moment from 'moment'

    export default {
        name: "Order",
        props:{ 
            order: Object,
        },
        methods: {
            format_date(value) {
                if (value) {
                    return moment(String(value)).format("DD/MM/YYYY");
                }
            },
            format_hour(value) {
                if (value) {
                    return moment(String(value)).format("HH:MM");
                }
            },
            format_price(value) {
                let val = (value / 1).toFixed(2).replace(".", ",");
                return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
            },
        }
    }
</script>
<style>
    @media print {
    .nobreak { 
        display: block;
        width: 100%;
     }

    .avoid{ page-break-inside: avoid; }
}
</style>