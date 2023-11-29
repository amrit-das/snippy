<script lang="ts">
    export let note;
    import { noteStore } from "$lib/stores";
    import { CodeBlock, getModalStore, Modal, type ModalSettings } from "@skeletonlabs/skeleton";
    import { initializeStores } from "@skeletonlabs/skeleton";

    initializeStores();
    const modalStore = getModalStore();

    function deleteNote(): void {
        const confirmDelete: ModalSettings = {
            type: 'confirm',
            title: 'Delete Snippet',
            body: 'Are you sure to delete this snippet?',
            response: (r: boolean) => {
                if (r) {
                    noteStore.update((notes) => notes.filter((n) => n.id !== note.id));
                }
            }
        }
        modalStore.trigger(confirmDelete)
    }

    let editingContent = '';

    function editNote(): void {
        editingContent = note.content;
        const editModal: ModalSettings = {
            type: 'prompt',
            title: 'Edit Snippet',
            value: note.content, 
            valueAttr: {type: 'text'},
            response: (r: string) => {
                if (r.length > 0) {
                    editingContent=r;
                    saveNote();
                }
            },
        };
        modalStore.trigger(editModal);
    }

    function saveNote(): void {
        noteStore.update((notes) => notes.map((n) => {
            if (n.id === note.id) {
                return { ...n, content: editingContent };
            }
            return n;
        }));
        editingContent = '';
    }
</script>

<Modal />

<div class="card p-4 variant-ghost-surface flex flex-col gap-2 relative">
    <button on:click={deleteNote} class="btn-icon btn-icon-sm variant-ghost-error absolute right-4 top-2">X</button>
    <strong>{note.title}</strong>
    <CodeBlock language="shell" code={note.content}/>
    <div class="flex gap-1 flex-wrap">
        {#each note.tags as tag}
            <span class="badge bg-gradient-to-br variant-gradient-primary-secondary">{tag}</span>
        {/each}
    </div>
    <button on:click={editNote} class="btn-icon btn-icon-sm variant-ghost-warning absolute right-4 bottom-2">Edit</button>
</div>
