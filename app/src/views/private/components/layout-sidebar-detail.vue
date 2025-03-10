<template>
	<sidebar-detail icon="layers" :title="t('layout_options')">
		<div class="layout-options">
			<div class="field">
				<div class="type-label">{{ t('layout') }}</div>
				<v-select v-model="layout" :items="layouts" item-text="name" item-value="id" item-icon="icon">
					<template v-if="currentLayout.icon" #prepend>
						<v-icon :name="currentLayout.icon" />
					</template>
				</v-select>
			</div>

			<slot />
		</div>
	</sidebar-detail>
</template>

<script lang="ts" setup>
import { useI18n } from 'vue-i18n';
import { computed } from 'vue';
import { useSync } from '@directus/composables';
import { useExtensions } from '@/extensions';
import { useExtension } from '@/composables/use-extension';

const props = withDefaults(
	defineProps<{
		modelValue?: string;
	}>(),
	{
		modelValue: 'tabular',
	}
);

const emit = defineEmits<{
	(e: 'update:modelValue', value: string): void;
}>();

const { t } = useI18n();

const { layouts } = useExtensions();

const selectedLayout = useExtension('layout', props.modelValue);
const fallbackLayout = useExtension('layout', 'tabular');
const currentLayout = computed(() => selectedLayout.value ?? fallbackLayout.value);

const layout = useSync(props, 'modelValue', emit);
</script>

<style lang="scss" scoped>
@import '@/styles/mixins/form-grid';

:deep(.layout-options) {
	--form-vertical-gap: 20px;

	margin-bottom: 4px;

	@include form-grid;
}
</style>
