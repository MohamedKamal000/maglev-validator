<script lang="ts">
	import JsonTree from './JsonTree.svelte';

	interface Props {
		data: unknown;
		otherData?: unknown;
		focusPath?: string;
		ignoredKeys?: string[];
		mode?: 'local' | 'production';
	}

	let { data, otherData, ignoredKeys = [], mode = 'local' }: Props = $props();

	let globalExpand = $state<boolean | null>(null);

	function expandAll() {
		globalExpand = true;
	}

	function collapseAll() {
		globalExpand = false;
	}
</script>

<div class="h-full bg-white font-sans text-sm dark:bg-slate-900">
	{#if data !== undefined}
		<div
			class="flex items-center justify-end gap-2 border-b border-slate-200 bg-white px-4 py-2 dark:border-slate-700 dark:bg-slate-900"
		>
			<button
				onclick={expandAll}
				class="flex items-center gap-1.5 rounded-lg border border-emerald-200 bg-emerald-50 px-3 py-1.5 text-xs font-medium text-emerald-700 transition-colors hover:bg-emerald-100 dark:border-emerald-800 dark:bg-emerald-900/30 dark:text-emerald-400 dark:hover:bg-emerald-900/50"
				title="Expand all nodes"
			>
				<svg class="h-3.5 w-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"
					><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"
					></path></svg
				>
				Expand All
			</button>
			<button
				onclick={collapseAll}
				class="flex items-center gap-1.5 rounded-lg border border-amber-200 bg-amber-50 px-3 py-1.5 text-xs font-medium text-amber-700 transition-colors hover:bg-amber-100 dark:border-amber-800 dark:bg-amber-900/30 dark:text-amber-400 dark:hover:bg-amber-900/50"
				title="Collapse all nodes"
			>
				<svg class="h-3.5 w-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"
					><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 15l7-7 7 7"
					></path></svg
				>
				Collapse All
			</button>
		</div>
	{/if}
	<div class="min-h-full p-4 font-mono">
		{#if data !== undefined}
			<JsonTree
				value={data}
				otherValue={otherData}
				side={mode === 'local' ? 'left' : 'right'}
				isReference={mode === 'production'}
				{ignoredKeys}
				level={0}
				{globalExpand}
			/>
		{:else}
			<div class="py-8 text-center text-slate-400 italic">No JSON data</div>
		{/if}
	</div>
</div>
