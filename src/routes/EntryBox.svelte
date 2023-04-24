<script>
	import { slide } from 'svelte/transition';

	export let item;
	export let setSelected;
	export let itemSelected;

	let open = false;

	const enter = () => (open = true);
	const leave = () => (open = false);
</script>

<div class={item.tags} transition:slide>
	<div
		class="group border-2 p-1 m-1"
		on:keydown={null}
		on:click={() => setSelected(item)}
		on:mouseenter={enter}
		on:mouseleave={leave}
	>
		<div class="font-semibold cursor-pointer">
			{item.title}
		</div>
		{#if open == true || itemSelected}
			<div transition:slide>
				{item.description}
			</div>
		{/if}
	</div>
</div>
