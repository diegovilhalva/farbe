<script setup lang="ts">
import type { Content } from "@prismicio/client";
import SlideIn from "~/components/SlideIn.vue";
import DescriptionList from "./DescriptionList.vue";
import PassThrough from "./PassThrough.vue";
import Div from "./Div.vue";
import DescriptionTerm from "./DescriptionTerm.vue";
import DescriptionDetails from "./DescriptionDetails.vue";


const props = defineProps(
  getSliceComponentProps<Content.ProductSlice>(),
);
const prismic = usePrismic()
const product = computed(() => {
  const prismicProduct = props.slice.primary.product
  if (!prismic.isFilled.contentRelationship(prismicProduct) || !prismicProduct.data?.stripe_id) {
    return undefined
  }
  return prismicProduct
})

const quantity = ref(1)
function setQuantity(value: number) {
  quantity.value = Math.max(1, value)
}

function onSubmit(event:Event) {
  event.preventDefault()
}
</script>

<template>
  <SlideIn v-if="product" as="article" class="bounded rich-text min-h-[150vh] flex flex-col justify-center">
    <header :id="product.uid" class="rich-text pt-[25h]">
      <PrismicRichText :field="product.data?.name" />
      <p aria-label="Price">TODO / roll</p>
    </header>
    <section class="rich-text">
      <h3>Description</h3>
      <PrismicRichText :field="product.data?.description" />
    </section>
    <section class="rich-text">
      <h2 class="sr-only">Characteristics</h2>
      <PrismicTable :field="product.data?.characteristics" :components="{
        table: DescriptionList,
        tbody: PassThrough,
        tr: Div,
        th: DescriptionTerm,
        td: DescriptionDetails
      }" />
    </section>
    <form class="mt-16 text-sm max-w-[calc(40ch+1rem)] -ml-4 flex items-start" @submit="onSubmit">
      <div class="flex-1 flex items-center">
        <button class="cta" type="button" @click="setQuantity(quantity - 1)">
          -
        </button>
        <div class="flex-1 text-center" aria-live="polite">
          {{ quantity }}
        </div>
        <button class="cta" type="button" @click="setQuantity(quantity + 1)">
          +
        </button>
      </div>
      <div class="flex-1">
        <button class="w-full cta primary" type="submit">
          Add to cart
        </button>
      </div>
    </form>
  </SlideIn>
  <SlideIn v-else as="article" class="bounded rich-text min-h-[150vh] flex flex-col justify-center">
    <p>Product not found</p>
  </SlideIn>
</template>
