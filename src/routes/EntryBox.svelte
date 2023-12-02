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
		class={`group border-2 p-1 m-1 ` + item.tags}
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

<style>
	.tag-WS {
		/* Weapon Skill (WS)  */
		background-color: #ff7f7f10;
	}

	.tag-BS {
		/* Ballistic Skill (BS)  */
		background-color: #7fbfff10;
	}

	.tag-S {
		/* Strength (S)  */
		background-color: #ffbf7f10;
	}

	.tag-T {
		/* Toughness (T)  */
		background-color: #7fff7f10;
	}

	.tag-I {
		/* Initiative (I)  */
		background-color: #ffff7f10;
	}

	.tag-Ag {
		/* Agility (Ag)  */
		background-color: #bfff7f10;
	}

	.tag-Dex {
		/* Dexterity (Dex)  */
		background-color: #ff7fbd10;
	}

	.tag-Int {
		/* Intelligence (Int)  */
		background-color: #7f7fff10;
	}

	.tag-WP {
		/* Willpower (WP)  */
		background-color: #ffbfbf10;
	}

	.tag-Fel {
		/* Fellowship (Fel)  */
		background-color: #7fffff10;
	}
</style>
