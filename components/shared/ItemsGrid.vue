<template>
        <v-row class="flex-grow-1">

            <template v-if="!loading">
                <v-col cols="12" md="4" sm="6" v-if="$slots.default">
                    <slot/>
                </v-col>
                <v-col cols="12" md="4" sm="6" v-for="product in products" height="500px">
                    <sharedItem :id="product.id" :name="product.name" :description="product.description" :img="product.img" :title="product.title" :price="product.price" :quantity="product.quantity" :is_available="product.is_available" >
                    
                    </sharedItem>
                </v-col>
            </template>
            <template v-else>
                <span class="flex-grow-1 flex-shrink-1 d-flex justify-center align-center">
                    <v-progress-circular indeterminate size="50"/>
                </span>
            </template>
        </v-row>

</template>

<script setup lang="ts">
    interface productInterface {
        products: {
            id: string,
            createdAt?: string,
            updatedAt?:string,
            name: string,
            description: string,
            title: string
            price: number,
            img?: string,
            quantity: number
            is_available: boolean,
            category?: string,
        }[]
    }


    const props = defineProps<{
        storeFunc: () => Promise<any>,
        watchVariable?: any,
        take: number,
        skip: number,
        loading: boolean,
    } & productInterface>()
    const watchVariableProp = ref(props.watchVariable)

    watchVariableProp.value && watch(watchVariableProp.value, async () => {
        await props.storeFunc(watchVariableProp.value, props.take, props.skip)
    })
</script>
