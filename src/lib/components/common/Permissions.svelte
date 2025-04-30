<script lang="ts">
	import { getContext, onMount } from 'svelte';
	const i18n = getContext('i18n');

	import Switch from '$lib/components/common/Switch.svelte';
	import Tooltip from '$lib/components/common/Tooltip.svelte';
	import PlusIcon from "$lib/components/icons/Plus.svelte";
	import Modal from "$lib/components/common/Modal.svelte";

	// Default values for permissions (fallback)
	const defaultPermissions = {
		workspace: {
			models: false,
			knowledge: false,
			prompts: false,
			tools: false
		},
		sharing: {
			public_models: false,
			public_knowledge: false,
			public_prompts: false,
			public_tools: false
		},
		chat: {
			controls: true,
			file_upload: true,
			delete: true,
			edit: true,
			stt: true,
			tts: true,
			call: true,
			multiple_models: true,
			temporary: true,
			temporary_enforced: false
		},
		features: {
			direct_tool_servers: false,
			web_search: true,
			image_generation: true,
			code_interpreter: true
		}
	};

	let showAddPermissionModal = [false, false, false, false];

	export let permissions = {};
	export let allowNewPermissions = false

	// Reactive statement to ensure all fields are present in `permissions`
	$: {
		permissions = fillMissingProperties(permissions, defaultPermissions);
	}

	function fillMissingProperties(obj: any, defaults: any) {
		return {
			...defaults,
			...obj,
			workspace: { ...defaults.workspace, ...obj.workspace },
			sharing: { ...defaults.sharing, ...obj.sharing },
			chat: { ...defaults.chat, ...obj.chat },
			features: { ...defaults.features, ...obj.features }
		};
	}

	function formatPermissionName(key: string): string {
		return key
			.split('_')
			.map(word => word.charAt(0).toUpperCase() + word.slice(1))
			.join(' ');
	}

	function formatPermissionCategory(key: string): string {
		return key.charAt(0).toUpperCase() + key.slice(1) + ' Permissions';
	}


	onMount(() => {
		permissions = fillMissingProperties(permissions, defaultPermissions);
	});
</script>

<div>
	<!-- Move this into modal, multi level modal don't work currently-->
	{#each Object.entries(permissions) as [categoryName, category], index}
		{#if showAddPermissionModal[index]}
			<Modal bind:show={showAddPermissionModal} class="flex flex-col w-full mt-1">
				<div class=" mb-1 text-xs text-gray-500">{$i18n.t('Name')}</div>

				<div class="flex-1">
					<input
						class="w-full text-sm bg-transparent disabled:text-gray-500 dark:disabled:text-gray-500 outline-hidden"
						type="text"
						placeholder={$i18n.t('Role name, should be lower case and without spaces')}
						autocomplete="off"
						required
					/>
				</div>
			</Modal>
		{/if}
		{#if index !== 0}
			<hr class=" border-gray-100 dark:border-gray-850 my-2" />
		{/if}
		<div>
			<div class=" mb-2 text-sm font-medium" on:mousedown|stopPropagation on:click|stopPropagation>
				{$i18n.t(formatPermissionCategory(categoryName))}
				{#if allowNewPermissions}
					<button	on:click={() => {
						showAddPermissionModal[index] = !showAddPermissionModal[index];
					}}>
						<Tooltip content={$i18n.t('Add New Permission')}>
							<PlusIcon className="ml-1 w-4 h-4 inline-block"/>
						</Tooltip>
					</button>
				{/if}
			</div>

			{#each Object.entries(category) as [key, value]}
				<div class="flex w-full justify-between my-2 pr-2">
					<div class="self-center text-xs font-medium">
						{$i18n.t(formatPermissionName(key))}
					</div>

					<Switch bind:state={permissions[categoryName][key]} />
				</div>
			{/each}
		</div>
	{/each}
</div>
