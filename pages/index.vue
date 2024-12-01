<script setup lang="ts">
import { useWebsiteHomePage } from "~/domains/core/composable/useWebsiteHomePage";

const { loadCategoryList, categories } = useCategory();
const { getWebsiteHomepage, websiteHomepage } = useWebsiteHomePage();

const { list } = useRecentViewProducts();

await getWebsiteHomepage();
await loadCategoryList({
  filter: { parent: true, id: null },
});
useHead(websiteHomepageHead(websiteHomepage.value, ""));
</script>

<template>
  <MainBanner />
  <NuxtLazyHydrate when-visible>
    <LazyDisplay />
  </NuxtLazyHydrate>
  
  <section class="pb-16" v-if="list?.length > 0">
    <ClientOnly>
      <LazyProductSlider
        heading="Your recent views"
        :ids="list"
        key="recent-views"
        key-for-composable="recent-views"
      />
    </ClientOnly>
  </section>
  <CategoryCard :categories="categories" />
  <section class="pb-16">
    <NuxtLazyHydrate when-visible>
      <LazyProductSlider
        heading="Inspired ________by your picks  chacha!!"
        key="inspired-by-picks"
        key-for-composable="inspired-by-picks"
      />
    </NuxtLazyHydrate>
  </section>
</template>
