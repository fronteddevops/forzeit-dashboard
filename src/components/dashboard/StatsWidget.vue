<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue';

const active5s = ref(152);
const active24h = ref(45);
const cards24h = ref(152);

// Track previous values for "Before vs Now"
const prevActive5s = ref(active5s.value);
const prevActive24h = ref(active24h.value);
const prevCards24h = ref(cards24h.value);

// Deltas and percent changes
const deltaActive5s = computed(() => active5s.value - prevActive5s.value);
const deltaActive24h = computed(() => active24h.value - prevActive24h.value);
const deltaCards24h = computed(() => cards24h.value - prevCards24h.value);

const pctActive5s = computed(() => prevActive5s.value ? (deltaActive5s.value / prevActive5s.value) * 100 : 0);
const pctActive24h = computed(() => prevActive24h.value ? (deltaActive24h.value / prevActive24h.value) * 100 : 0);
const pctCards24h = computed(() => prevCards24h.value ? (deltaCards24h.value / prevCards24h.value) * 100 : 0);

let interval5s = null;
let interval24hUsers = null;
let interval24hCards = null;

function randomChange(min, max) {
  return Math.round(Math.random() * (max - min) + min);
}

onMounted(() => {
  interval5s = setInterval(() => {
    const next = Math.max(0, active5s.value + randomChange(-10, 20));
    prevActive5s.value = active5s.value;
    active5s.value = next;
  }, 5000);

  interval24hUsers = setInterval(() => {
    const next = Math.max(0, active24h.value + randomChange(-5, 15));
    prevActive24h.value = active24h.value;
    active24h.value = next;
  }, 10000);

  interval24hCards = setInterval(() => {
    const next = Math.max(0, cards24h.value + randomChange(-5, 15));
    prevCards24h.value = cards24h.value;
    cards24h.value = next;
  }, 10000);
});

onUnmounted(() => {
  if (interval5s) clearInterval(interval5s);
  if (interval24hUsers) clearInterval(interval24hUsers);
  if (interval24hCards) clearInterval(interval24hCards);
});
</script>

<template>
  
      <div class="col-span-12 lg:col-span-6 xl:col-span-4">
        <div class="card mb-0 flex flex-col items-center text-center p-6">
          <div
            class="flex items-center justify-center bg-cyan-100 dark:bg-cyan-400/10 rounded-full mb-3"
            style="width: 3rem; height: 3rem"
          >
            <i class="pi pi-users text-cyan-500 !text-2xl"></i>
          </div>
          <div class="text-cyan-600 dark:text-cyan-400 font-bold text-3xl mb-1 flex items-center gap-2">
            <span>{{ active5s.toLocaleString() }}</span>
            <span class="text-xs font-semibold flex items-center gap-1" :class="deltaActive5s >= 0 ? 'text-green-500' : 'text-red-500'">
              +{{ Math.abs(deltaActive5s).toLocaleString() }} ({{ Math.abs(pctActive5s).toFixed(2) }}%)
              <span class="inline-flex items-center justify-center h-3 w-3 rounded-full" :class="deltaActive5s >= 0 ? 'bg-green-500' : 'bg-red-500'">
                <svg v-if="deltaActive5s >= 0" class="h-2.5 w-2.5 text-white" viewBox="0 0 20 20" fill="currentColor"><path d="M10 6l4 6H6l4-6z"/></svg>
                <svg v-else class="h-2.5 w-2.5 text-white rotate-180" viewBox="0 0 20 20" fill="currentColor"><path d="M10 6l4 6H6l4-6z"/></svg>
              </span>
            </span>
          </div>
       
          <span class="text-gray-500 dark:text-gray-400">
            The number of active users live in the last 5 seconds
          </span>
        </div>
      </div>
  
      <div class="col-span-12 lg:col-span-6 xl:col-span-4">
        <div class="card mb-0 flex flex-col items-center text-center p-6">
          <div
            class="flex items-center justify-center bg-green-100 dark:bg-green-400/10 rounded-full mb-3"
            style="width: 3rem; height: 3rem"
          >
          <svg xmlns="http://www.w3.org/2000/svg" class="text-green-500 !text-2xl"
     width="30" height="30" viewBox="0 0 24 24" 
     fill="none" stroke="currentColor" stroke-width="2" 
     stroke-linecap="round" stroke-linejoin="round">
  <line x1="12" y1="1" x2="12" y2="3"></line>
  <line x1="16.5" y1="4.5" x2="18" y2="6"></line>
  <circle cx="12" cy="14" r="8"></circle>
  <line x1="12" y1="14" x2="15" y2="11"></line>
</svg>
          </div>
          <div class="text-green-600 dark:text-green-400 font-bold text-3xl mb-1 flex items-center gap-2">
            <span>{{ active24h.toLocaleString() }}</span>
            <span class="text-xs font-semibold flex items-center gap-1" :class="deltaActive24h >= 0 ? 'text-green-500' : 'text-red-500'">
              +{{ Math.abs(deltaActive24h).toLocaleString() }} ({{ Math.abs(pctActive24h).toFixed(2) }}%)
              <span class="inline-flex items-center justify-center h-3 w-3 rounded-full" :class="deltaActive24h >= 0 ? 'bg-green-500' : 'bg-red-500'">
                <svg v-if="deltaActive24h >= 0" class="h-2.5 w-2.5 text-white" viewBox="0 0 20 20" fill="currentColor"><path d="M10 6l4 6H6l4-6z"/></svg>
                <svg v-else class="h-2.5 w-2.5 text-white rotate-180" viewBox="0 0 20 20" fill="currentColor"><path d="M10 6l4 6H6l4-6z"/></svg>
              </span>
            </span>
          </div>
         
          <span class="text-gray-500 dark:text-gray-400">
            The number of users active in the last 24 hours
          </span>
        </div>
      </div>
  
      <div class="col-span-12 lg:col-span-6 xl:col-span-4">
        <div class="card mb-0 flex flex-col items-center text-center p-6">
          <div
            class="flex items-center justify-center bg-purple-100 dark:bg-purple-400/10 rounded-full mb-3"
            style="width: 3rem; height: 3rem"
          >
   
<svg xmlns="http://www.w3.org/2000/svg" 
     width="30" height="30" viewBox="0 0 24 24" 
     fill="currentColor" class=" text-purple-500">
  <rect x="3" y="5" width="18" height="14" rx="2" ry="2"/>
  <circle cx="8" cy="12" r="2.5" fill="white"/>
  <rect x="13" y="10" width="6" height="2" rx="1" fill="white"/>
  <rect x="13" y="14" width="6" height="2" rx="1" fill="white"/>
  <circle cx="19" cy="5" r="4" fill="white" stroke="currentColor" stroke-width="2"/>
  <path d="M19 3v4M17 5h4" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
</svg>


          </div>
          <div class="text-purple-600 dark:text-purple-400 font-bold text-3xl mb-1 flex items-center gap-2">
            <span>{{ cards24h.toLocaleString() }}</span>
            <span class="text-xs font-semibold flex items-center gap-1" :class="deltaCards24h >= 0 ? 'text-green-500' : 'text-red-500'">
              +{{ Math.abs(deltaCards24h).toLocaleString() }} ({{ Math.abs(pctCards24h).toFixed(2) }}%)
              <span class="inline-flex items-center justify-center h-3 w-3 rounded-full" :class="deltaCards24h >= 0 ? 'bg-green-500' : 'bg-red-500'">
                <svg v-if="deltaCards24h >= 0" class="h-2.5 w-2.5 text-white" viewBox="0 0 20 20" fill="currentColor"><path d="M10 6l4 6H6l4-6z"/></svg>
                <svg v-else class="h-2.5 w-2.5 text-white rotate-180" viewBox="0 0 20 20" fill="currentColor"><path d="M10 6l4 6H6l4-6z"/></svg>
              </span>
            </span>
          </div>
        
          <span class="text-gray-500 dark:text-gray-400">
            The number of cards created in the last 24 hours
          </span>
        </div>
  
    </div>
  </template>
  