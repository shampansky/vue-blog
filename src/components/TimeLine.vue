<script setup lang="ts">
import { ref } from 'vue';
// import type { Post } from '@/posts';
import { today, thisWeek, thisMonth } from '@/posts';
import { DateTime } from 'luxon';

const periods = ['Today', 'This Week', 'This Month'] as const;

type Period = (typeof periods)[number];

const selectedPeriod = ref<Period>(periods[0]);

const posts = [today, thisWeek, thisMonth].map((post) => {
  return {
    ...post,
    created: DateTime.fromISO(post.created)
  };
});

function selectPeriod(period: Period) {
  selectedPeriod.value = period;
}
</script>

<template>
  <nav class="is-primary panel">
    <span class="panel-tabs">
      <a
        v-for="period of periods"
        :key="period"
        :class="{ 'is-active': period === selectedPeriod }"
        @click="selectPeriod(period)"
      >
        {{ period }}
      </a>
    </span>

    <a v-for="post of posts" :key="post.id" class="panel-block">
      <a>{{ post.title }}</a>
      <div>{{ post.created.setLocale('ru').toFormat('d MMMM') }}</div>
    </a>
  </nav>
</template>
