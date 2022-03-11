<template>
    <span :class="ns.e('item')">
        <span
            ref="link"
            :class="[ns.e('inner'), ns.is('link', !!to)]"
            role="link"
        >
            <slot />
        </span>
        <el-icon
            v-if="separatorIcon"
        >
            <component :is="separatorIcon" />
        </el-icon>
        <span
            v-else
            role="presentation"
        >
            {{ separator }}
        </span>
    </span>
</template>

<script lang="ts" setup>
import { inject, ref, onMounted, getCurrentInstance } from 'vue';
import type { Router, RouteLocationRaw } from 'vue-router';
// defineOptions({
//     name: 'ElBreadcrumbItem',
// });
const props = withDefaults(defineProps<{
    to: RouteLocationRaw;
    replace: boolean;
}>(), {
    to: '',
    replace: false,
});
const instance = getCurrentInstance()!;
const router = instance.appContext.config.globalProperties.$router as Router;
const parent = inject('breadcrumb', undefined);
const { separator, separatorIcon } = parent ?? {};
const link = ref<HTMLSpanElement>();
onMounted(() => {
  link.value!.setAttribute('role', 'link');
  link.value!.addEventListener('click', () => {
      if (!props.to || !router) return;
      props.replace ? router.replace(props.to) : router.push(props.to);
  });
});
</script>