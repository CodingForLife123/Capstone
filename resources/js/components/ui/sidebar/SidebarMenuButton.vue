<script setup lang="ts">
import { Tooltip, TooltipContent, TooltipTrigger } from '@/components/ui/tooltip'
import { type Component, computed } from 'vue'
import SidebarMenuButtonChild, { type SidebarMenuButtonProps } from './SidebarMenuButtonChild.vue'
import { useSidebar } from './utils'

defineOptions({
  inheritAttrs: false,
})

const props = withDefaults(
  defineProps<SidebarMenuButtonProps & { tooltip?: string | Component; isActive?: boolean }>(),
  {
    as: 'button',
    variant: 'default',
    size: 'default',
    isActive: false,
  }
)

const { isMobile, state } = useSidebar()

const delegatedProps = computed(() => {
  const { tooltip, isActive, ...delegated } = props
  return delegated
})

// Only active class; no hover
const activeClass = computed(() => props.isActive ? 'bg-gray-900 text-white' : '')
</script>

<template>
  <!-- No tooltip -->
  <SidebarMenuButtonChild
    v-if="!props.tooltip"
    v-bind="{ ...delegatedProps, ...$attrs }"
    :class="[activeClass, $attrs.class]"
  >
    <slot />
  </SidebarMenuButtonChild>

  <!-- With tooltip -->
  <Tooltip v-else>
    <TooltipTrigger as-child>
      <SidebarMenuButtonChild
        v-bind="{ ...delegatedProps, ...$attrs }"
        :class="[activeClass, $attrs.class]"
      >
        <slot />
      </SidebarMenuButtonChild>
    </TooltipTrigger>
    <TooltipContent
      side="right"
      align="center"
      :hidden="state !== 'collapsed' || isMobile"
    >
      <template v-if="typeof props.tooltip === 'string'">
        {{ props.tooltip }}
      </template>
      <component :is="props.tooltip" v-else />
    </TooltipContent>
  </Tooltip>
</template>
