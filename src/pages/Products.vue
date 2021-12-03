<script lang="ts" setup>
import { useAsyncData } from '/src/helpers/useAsyncData';
import Card from '/src/components/Card.vue';
import { useHead } from '@vueuse/head';

useHead({
   title: 'Products'
});

interface Product {
   id: number;
   title: string;
   price: number;
   description: string;
   category: string;
   image: string;
}

const productData = await useAsyncData<Product[]>(
   'productsData',
   // 'https://fakestoreapi.com/products/',
   'https://api.larsonelectronicglass.com/jsonapi/commerce_product_variation/default?filter[product_id.id]=7dde085d-2ad2-4887-9fbc-24ffabdb7c9e&filter[status]=1&sort=sku&include=field_variant_images&page[limit]=20&page[offset]=0',
   {
      axiosConfig: {},
      awaitSetup: false
   }
);
const productDataTransformed = productData?.value?.data.map(data => {
   // console.log({data})
   return {
      id: data.id,
      title: data.attributes.title,
      price: data.attributes.price.formatted,
      description: data.attributes.sku,
      category: 'foo',
      image: '',
   }
}) || []

</script>

<template>
   <div class="flex flex-row flex-wrap px-10 justify-center">
      <template v-if="productDataTransformed">
         <Card v-for="product in productDataTransformed" :key="product.id" :product="product" />
      </template>
      <div v-else>product data will be here</div>
   </div>
</template>
