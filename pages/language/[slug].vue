<template>
  <div class="p-4 w-full">
    <div v-if="repositories.length === 0" class="text-center text-gray-500 py-8">
      No repositories found
    </div>
    <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
      <RepoBox 
        v-for="repo in repositories" 
        :key="repo.id" 
        :repo="repo" 
      />
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import Repositories from '~/data/generated.json'
import Tags from '~/data/tags.json'

const route = useRoute()

const repositories = computed(() => 
  Repositories.filter(repository => repository.slug === route.params.slug)
)

const tag = computed(() => 
  Tags.find(t => t.slug === route.params.slug)
)

useHead({
  title: computed(() => `${tag.value?.language || 'Unknown'} | Good First Issue`),
  meta: [{
    name: 'description',
    content: computed(() => 
      `Curated list of issues in ${tag.value?.language || 'this language'} from popular open-source projects that you can easily fix.`
    )
  }]
})
</script>
