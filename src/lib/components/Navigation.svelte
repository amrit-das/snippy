<script lang="ts">
	import { getDrawerStore } from "@skeletonlabs/skeleton";

	const drawerStore = getDrawerStore();
	function drawerClose(): void {
		drawerStore.close();
	}
</script>
<nav class="list-nav p-4">
    <ul>
        <li><a href="/">Snippets</a></li>
        <li><a href="/?test=1" on:click={drawerClose}>Page 1</a></li>
        <li><a href="/?test=2" on:click={drawerClose}>Page 2</a></li>
        <li><a href="/?test=3" on:click={drawerClose}>Page 3</a></li>
    </ul>
</nav>