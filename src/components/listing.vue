<template>
    <div class="container">
        <template v-for="items in data" v-bind:key="items.id">
            <div class="card mb-3" style="max-width: 540px;">
                <div class="row g-0">
                    <div class="col-md-4">
                        <img :src="items.logo" class="img-fluid rounded-start">
                    </div>
                    <div class="col-md-8">
                        <div class="card-body">
                            <h5 class="card-title">{{ items.name }}</h5>
                            <p class="card-text">{{ items.description }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </template>
        <v-pagination
            v-model="currentPage"
            :pages="pages"
            :range-size="limit"
            active-color="#DCEDFF"
            @update:modelValue="updateHandler"
        />
    </div>
</template>
<script>
import axios from 'axios';
import VPagination from "@hennge/vue3-pagination";
import "@hennge/vue3-pagination/dist/vue3-pagination.css";

export default {
    components: {
        VPagination
    },
    data() {
        return {
            currentPage: 1,
            limit: 5,
            data: [],
            total: 0,
            pages: 0
        }
    },

    created(){
        this.getData(this.currentPage);
    },

    methods: {
        getData: function(page) {
            let me = this;
            axios.get('https://api.staging.klinikpintar.id/hc/open/health-center', 
                {
                    headers: {
                        'X-Medigo-Client-Key': '994e23e67b6cb551a2951f8abaf2aa5265d5c2fb63a5679457a596efb8444270'
                    },
                    params: {
                        page: page,
                        limit: me.limit
                    }
                }
            )
            .then(function(result) {
                me.data = result.data.data;
                me.total = result.data.total;
                me.currentPage = result.data.page;
                me.pages = result.data.pages;

                console.log(result.data)
            })
        },

        updateHandler: function(event) {
            this.getData(event);
        }
    }
}
</script>