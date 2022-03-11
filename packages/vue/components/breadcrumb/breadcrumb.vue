<script lang="ts" setup>
import { breadcrumb as _styles} from 'cosmic-ui';
import type { Component } from 'vue';
import { provide, ref, onMounted } from 'vue';

// defineOptions({
//     name: 'ElBreadcrumb',
// });

const props = withDefaults(defineProps<{
    separator: string;
    separatorIcon: Component;
    styles: object,
}>(), {
    separator: '/',
    styles: _styles,
});
const breadcrumb = ref<HTMLDivElement>();
provide('breadcrumb', props);
onMounted(() => {
    const items = breadcrumb.value!.querySelectorAll('.item');
    if (items.length) {
        items[items.length - 1].setAttribute('aria-current', 'page');
    }
});
</script>
<template>
    <div
        ref="breadcrumb"
        aria-label="Breadcrumb"
        role="navigation"
    >
        <slot />
    </div>
</template>
