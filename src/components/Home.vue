<template>
    <div class="w-20 mb-3">
      <img alt="Dudu e daurinha Logo" src="https://raw.githubusercontent.com/mayllonvrs/duduedaurinha-assets/master/logo-duduedaurinha-240.png"/>
    </div>
    <div class="home w-full md:w-30 text-gray-700">
        <div class="w-full">
            <Paginate :count="count" :limit="limit" :currentPage="page"/>
            <table class="w-full">
                <thead class="bg-gray-200">
                    <tr>
                        <th>Código</th> 
                        <th class="text-left">Cliente</th> 
                        <th class="text-left">Data</th> 
                        <th class="text-left">Total</th> 
                    </tr>
                </thead>
                <tr v-for="order in orders" :key="order._id" class="border-b border-gray-300 cursor-pointer hover:bg-white" v-on:click="showOrder(order._id)">
                    <td>{{order._id.substr(19)}}</td>
                    <td class="text-left py-1">{{order.client.name}}</td>
                    <td class="text-left py-1">{{ format_date(order.createdAt) }}</td>
                    <td class="text-left py-1">R$ {{ format_price(order.total)}}</td>
                </tr>
            </table>
        </div>
    </div>
</template>
<script>
    import axios from "axios"
    import moment from 'moment'
    import {useRouter} from "vue-router";
    import Paginate from "./Paginate.vue";
    // import router from '@/router';

    export default {
    name: "Orders",
    data() {
        return {
            orders: null,
            count: null,
            page: null,
            limit: null
        };
    },
    created() {
        this.getOrders();
    },
    methods: {
        getOrders() {
            var page = 1;
            var limit = 10;
            const queryString = window.location.search;
            const urlParams = new URLSearchParams(queryString);
            if (urlParams.get("limit")) {
                limit = urlParams.get("limit");
            }
            if (urlParams.get("page")) {
                page = urlParams.get("page");
            }
            axios
                .get(`http://45.80.153.95:30000/orders?limit=${limit}&page=${page}`)
                .then((res) => {
                this.orders = res.data.orders;
                this.count = res.data.count;
                this.limit = limit;
                this.page = page;
            })
                .catch((error) => {
                console.log(error);
            });
        },
        format_date(value) {
            if (value) {
                return moment(String(value)).format("DD/MM/YYYY");
            }
        },
        format_price(value) {
            let val = (value / 1).toFixed(2).replace(".", ",");
            return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        },
        showOrder(id) {
            window.history.pushState({}, "", `/?limit=${this.limit}&page=${this.page}`);
            console.log(id);
            // \\router.replace(`/order?id=${id}`)
        }
    },
    components: { Paginate }
}
</script>