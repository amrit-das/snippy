<script lang="ts">
    import Snippets from '$lib/components/Snippets.svelte';
    import { noteStore } from "$lib/stores";
	import { Modal } from '@skeletonlabs/skeleton';
    import Fuse from "fuse.js";
	import { writable } from 'svelte/store';

    const searchResultsStore = writable([]);
    let searchQuery = '';
    let fuse;

    noteStore.subscribe((notes) => {
        fuse = new Fuse(notes, {
            keys: ['title', 'content', 'tags'],
            includeScore: true,
        });
    })();

    function searchNotes(): void {
        const results = fuse.search(searchQuery).map(result => result.item);
        searchResultsStore.set(results);
    }
</script>

<div class="container h-full mx-auto gap-8 flex flex-col">
    <div class="flex items-center justify-between">
        <h2 class="h2">Code Snippets</h2>
        <a href="/new" class="btn variant-ghost-primary">Add</a>
    </div>
    <div class="flex gap-8">
        <input bind:value={searchQuery} on:input={() => searchNotes()} class="input" type="text" placeholder="Filter..." />
    </div>
    <div class="grid gap-4 grid-cols-1">
        {#if searchQuery}
            {#each $searchResultsStore as note}
                <Snippets {note} />
            {/each}
        {:else}
            {#each $noteStore as note}
                <Snippets {note} />
            {/each}
        {/if}
    </div>
</div>
