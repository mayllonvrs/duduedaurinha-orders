<template>
    <div class="text-xs  w-full py-1 flex grid justify-items-center md:justify-items-end pt-2">
        <div class="flex  md:pr-5">
            Total: {{ count }} |
            Pedidos por página:
                <select id="limit" class="bg-white border-b border-gray-400 w-10" @change="changeLimit($event)">
                    <option v-for="quantity in quantities" :key="quantity" :value="quantity" :selected="quantity == limit">{{quantity}} </option>
                </select>
            &nbsp;Página:
            <select id="page" class="bg-white border-b border-gray-400 w-10" @change="changePage($event)">
                <option v-for="page in range(1, Math.ceil(count/limit)+1)" :key="page" :value="page" :selected="page == currentPage">{{page}}</option>
            </select>
            &nbsp;
            <div v-on:click="previous()" class="flex cursor-pointer" v-if="currentPage > 1">
            <div class="w-4">
                <svg xmlns="http://www.w3.org/2000/svg" className="h-1 w-1" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
                    <path strokeLinecap="round" strokeLinejoin="round" d="M11 19l-7-7 7-7m8 14l-7-7 7-7" />
                </svg>
            </div>
            <p>Anterior</p> 
        </div>
        <div class="flex cursor-not-allowed text-gray-400" v-if="currentPage == 1">
            <div class="w-4">
                <svg xmlns="http://www.w3.org/2000/svg" className="h-1 w-1" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
                    <path strokeLinecap="round" strokeLinejoin="round" d="M11 19l-7-7 7-7m8 14l-7-7 7-7" />
                </svg>
            </div>
            <p>Anterior</p> 
        </div>
        <div v-on:click="next()" class="flex cursor-pointer" v-if="currentPage < Math.ceil(count/limit)">
            <p>&nbsp;|&nbsp;Próxima</p>
            <div class="w-4">
                <svg xmlns="http://www.w3.org/2000/svg" className="h-1 w-1" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
                    <path strokeLinecap="round" strokeLinejoin="round" d="M13 5l7 7-7 7M5 5l7 7-7 7" />
                </svg>
            </div>
        </div>
        <div class="flex cursor-not-allowed text-gray-400" v-if="currentPage == Math.ceil(count/limit)">
            <p>&nbsp;|&nbsp;Próxima</p>
            <div class="w-4">
                <svg xmlns="http://www.w3.org/2000/svg" className="h-1 w-1" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
                    <path strokeLinecap="round" strokeLinejoin="round" d="M13 5l7 7-7 7M5 5l7 7-7 7" />
                </svg>
            </div>
        </div>
        </div>
        
    </div>
</template>
<script>
    export default {
        name: "Paginate",
        data() {
            return {
                quantities: [1,3,5,10,25,50,100],
                count: this.count,
                limit: this.limit,
            }
        },
        props:{ 
            count: Number,
            currentPage: Number,
            limit: Number 
        },
        created() {
            this.getValues();
        },
        methods: {
            getValues(){                
            },
            range(start, end){
                const length = end - start;
                return Array.from({ length }, (_, i) => start + i);
            },
            changeLimit(event){
                window.location.href = '?limit='+event.target.value
            },
            changePage(event){
                window.location.href = '?limit='+document.querySelector("#limit").value+
                                        '&page='+event.target.value
            },
            previous(){
                const page = document.querySelector("#page").value-1
                window.location.href = '?limit='+document.querySelector("#limit").value+
                                        '&page='+page
            },
            next(){
                const page = parseInt(document.querySelector("#page").value)+1
                window.location.href = '?limit='+document.querySelector("#limit").value+
                                        '&page='+page
            }
        },
        
    }
</script>