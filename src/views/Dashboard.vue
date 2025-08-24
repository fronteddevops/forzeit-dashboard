<script setup>
import RevenueStreamWidget from '@/components/dashboard/PaidSubConv.vue';
import PaidSubscription from '@/components/dashboard/PaidSubscription.vue';
import RecentSalesWidget from '@/components/dashboard/RecentSalesWidget.vue';
import RevenueMrr from '@/components/dashboard/RevenueMrr.vue';
import StatsWidget from '@/components/dashboard/StatsWidget.vue';
import TrialSignups from '@/components/dashboard/TrialSignups.vue';
import TrialSignupsign from '@/components/dashboard/TrialSignupsign.vue';
</script>

<template>
  <div class="grid grid-cols-12 gap-8">
    
      <StatsWidget />
   

    <div class="col-span-12 xl:col-span-6">
      <RecentSalesWidget />
    </div>
    <div class="col-span-12 xl:col-span-6">
      <RevenueStreamWidget />

    </div>
    <div class="col-span-12 xl:col-span-6">
  <div > <!-- Fixed height -->
    <TrialSignups class="w-full h-full" />
  </div>
</div>

<div class="col-span-12 xl:col-span-6">
  <div > <!-- Same fixed height -->
    <PaidSubscription class="w-full h-full" />
  </div>
</div>


    <div class="col-span-12">
      <TrialSignupsign />
    </div>
    
    <div class="col-span-12 xl:col-span-12">
      <RevenueMrr />

    </div>
  </div>
</template>
