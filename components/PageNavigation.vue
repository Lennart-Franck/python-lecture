<template>
  <div class="w-[200px] p-5 sticky top-3 border rounded-md bg-white dark:bg-gray-900 dark:text-white hidden lg:block">
  
					<h2 class="text-sm font-bold mb-4">Inhalt</h2>
					<ul class="space-y-2">
						<template v-for="(t, k) in toc" :key="`toc-item-${k}`">
							<li>
								<NuxtLink
									:class="{
										'text-sm ml-4': t.depth == 2,
										'text-[13px] ml-6': t.depth > 2,
									}"
									class="capitalize"
									:to="`#${t.id}`"
									>{{ t.title }}</NuxtLink
								>
							</li>
						</template>
					</ul>
				
  </div>
</template>
<script>
export default defineComponent({
  props: ['toc'],
})
</script>
<style lang=""></style>