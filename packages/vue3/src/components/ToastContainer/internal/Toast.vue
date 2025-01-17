<script setup lang="ts">
import { useSlots } from 'vue';
import type { ToastProps } from '../types';
import { Alert } from '~/components';

withDefaults(
  defineProps<ToastProps>(),
  {
    position: 'top-right',
    canClose: true,
    type: 'info',
  },
);
const emit = defineEmits(['close']);

const { title, description } = useSlots();
</script>

<template>
  <Transition :name="position">
    <div v-if="show" class="toast-block" :class="[position, { relative }]">
      <Alert :type="type" :can-close="canClose" :close-after-duration="closeAfterDuration" @close="() => emit('close')">
        <template v-if="title" #title>
          <slot name="title" />
        </template>
        <template v-if="description" #description>
          <slot name="description" />
        </template>
      </Alert>
    </div>
  </Transition>
</template>

<style scoped lang="scss">
$animation-duration: 0.25s;

.top-right-enter-active,
.bottom-right-enter-active {
  animation: right $animation-duration ease-in-out;
}

.top-right-leave-active,
.bottom-right-leave-active {
  animation: right $animation-duration reverse ease-in-out;
}

@keyframes right {
  0% {
    left: 100%;
    opacity: 0;
  }

  100% {
    left: 0;
    opacity: 1;
  }
}

.top-left-enter-active,
.bottom-left-enter-active {
  animation: left $animation-duration ease-in-out;
}

.top-left-leave-active,
.bottom-left-leave-active {
  animation: left $animation-duration reverse ease-in-out;
}

@keyframes left {
  0% {
    left: -100%;
    opacity: 0;
  }

  100% {
    left: 0;
    opacity: 1;
  }
}
</style>
