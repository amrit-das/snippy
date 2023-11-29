<script lang="ts">
	import { goto } from "$app/navigation";
	import { noteStore } from "$lib/stores";
    import { InputChip, Toast, getToastStore, initializeStores, type ToastSettings } from "@skeletonlabs/skeleton";
    let title: string;
    let content: string;
    let tags: string[] = [];
    
    initializeStores();
    const toastStore = getToastStore();
    const t: ToastSettings = {
	    message: 'Snippet added successfully',
        timeout: 2000,
    };
    
    function createNote(): void{
        noteStore.update((notes) => [...notes, {
            id: crypto.randomUUID(),
            title,
            content,
            tags
        }])
        title = '';
        content = '';
        tags = [];
        toastStore.trigger(t);
    }
</script>

<Toast />
<div class="container h-full mx-auto gap-8 flex flex-col">
    <form class="card p-4 flex flex-col gap-3">
        <h2>Code Snippet</h2>
        <input bind:value={title} class="input" type="text" placeholder="Title"/>
        <textarea bind:value={content} class="textarea" rows="5" placeholder="Snippet"/>
        <InputChip bind:value={tags} name="tags" placeholder="Tags"/>
        <button type="button" on:click={createNote} class="btn self-end variant-ghost-primary">Add</button>
    </form>
</div>
