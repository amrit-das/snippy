<script lang="ts">
	import '../app.postcss';
	import { AppBar, AppShell, Avatar, LightSwitch } from '@skeletonlabs/skeleton';
</script>

<AppShell slotSidebarLeft="w-0 md:w-52 bg-surface-500/10">
	<svelte:fragment slot="header">
		<AppBar>
			<svelte:fragment slot="lead">
				<a href="/"><strong class="text-xl">Snippy</strong></a>
			</svelte:fragment>
			<svelte:fragment slot="trail">
				<LightSwitch />
				<Avatar 
				initials="AD" width="w-12"
				border="border-4 border-surface-300-600-token hover:!border-primary-500"
				cursor="cursor-pointer"
				/>
			</svelte:fragment>
		</AppBar>
	</svelte:fragment>
	<div class="container p-10 mx-auto">
		<slot/>
	</div>
</AppShell>
