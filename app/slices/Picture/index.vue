<script setup lang="ts">
import type { Content } from "@prismicio/client";
import SlideIn from "~/components/SlideIn.vue";

// The array passed to `getSliceComponentProps` is purely optional.
// Consider it as a visual hint for you when templating your slice.
defineProps(
  getSliceComponentProps<Content.PictureSlice>(),
);
</script>

<template>
  <section :data-slice-type="slice.slice_type" :data-slice-variation="slice.variation"
    class="grid xl:grid-cols-[3fr_2fr] ">
    <figure class="contents">
      <PrismicImage :field="slice.primary.picture" class="z-20 w-full row-span-2" loading="lazy" />
      <SlideIn class="px-4 pt-4 pb-16 rich-text" :class="{'xl:self-start':slice.variation === 'default' || slice.variation === 'button',
        'xl:order-last xl:self-end':slice.variation === 'top',
      }">
        <PrismicRichText :field="slice.primary.caption" />
      </SlideIn>
    </figure>
    <figure
    v-if="$prismic.isFilled.image(slice.primary.secondary_picture)"
    class="flex flex-col "
    :class="{'xl:self-end':slice.variation === 'default' || slice.variation === 'button',
        'xl:order-start xl:self-end':slice.variation === 'top',
      }"

    >
      <PrismicImage :field="slice.primary.secondary_picture" class="z-20 w-full" loading="lazy" />
      <SlideIn class="px-4 pt-4 p-6 rich-text"
      :class="{
        'xl:order-first': slice.variation === 'button'
      }">
        <PrismicRichText :field="slice.primary.secondary_caption" />
      </SlideIn>
    </figure>
  </section>
</template>
