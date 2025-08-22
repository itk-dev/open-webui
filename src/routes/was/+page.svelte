<!-- PATCH REDIRECT TO WAS -->
<script>
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';
	import { config } from '$lib/stores';

	function IsJson(str) {
		try {
			JSON.parse(str);
		} catch (e) {
			return false;
		}
		return true;
	}

	function getUrl(str) {
		if (IsJson(str)) {
			return JSON.parse(str);
		} else {
			return str;
		}
	}

	onMount(async () => {
		const WAS_REDIRECT_URL = $config?.extended_features?.was_redirect_url || false;
		if (WAS_REDIRECT_URL) window.open(getUrl(WAS_REDIRECT_URL), '_blank');
		goto('/');
	});
</script>

<!-- /PATCH REDIRECT TO WAS -->
