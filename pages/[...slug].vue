<template>
  <div class="max-w-6xl grid grid-flow-col auto-cols-auto px-4 py-10 m-auto bg-white sm:px-8 sm:shadow dark:bg-gray-800 sm:rounded">
    <div class="col-auto hidden lg:block pr-6">

      <PageNavigation v-if="toc" :toc="toc"/>
    </div>

    <!-- Fetch and display the Markdown document from current path -->
    <ContentDoc class="prose prose-gray dark:prose-invert max-w-none">
      <!-- Slot if document is not found -->
      <template #not-found>
        <h1 class="text-3xl">
          Page not found
        </h1>
      </template>
      <template #empty>
        <h1 class="text-3xl">
          Page noch nicht gefüllt
        </h1>
      </template>
    </ContentDoc>

  </div>
</template>

<script setup>
const slug = useRoute().params.slug.toString().replace(/,/g, "/");
	const { data: blog } = await useAsyncData(slug, () => {
		return queryContent(slug).findOne();
	});
	const toc = computed(() => {
		if (!blog.value) return [];
		const items = blog.value.excerpt?.children;
		if (!items) return [];
		const toc = [];
		const tags = ["h2", "h3", "h4", "h5", "h6"];
		items.forEach((item) => {
			if (tags.includes(item.tag)) {
				toc.push({
					id: item.props.id,
					title: item.props.id.toString().replace(/-/g, " "),
					depth: Number(item.tag.replace(/h/g, "")),
				});
			}
		});
		return toc;
	});


</script>

<style lang="postcss">
/* Customize headers to remove default underline */
.prose h2 a,
.prose h3 a {
  @apply no-underline;
  &:hover {
    @apply border-b border-neutral-600 dark:border-neutral-300;
  }
}
</style>
