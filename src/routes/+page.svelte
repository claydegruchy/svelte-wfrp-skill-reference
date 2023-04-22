<script lang="js">
	import skills from './skills.json';

	// import {clickOutside} from './clickOutside.js';

	let tags;
	let mainAttrib;

	$: if (tags) {
		mainAttrib = skills.attributes.items.find((a) => a.tags == tags)?.title;
	}

	let search = null;
	const selectTag = (item) => {
		tags = item;
	};

	let isFocused = false;

	$: tags && console.log(tags);

	const resetTags = (e) => (tags = null);
	const resetSearch = (e) => (e.target.value.length > 0 ? null : (search = null));
	const setSearch = (e) => {
		const v = e.target && e.target.value;
		if (v.length > 0) {
			search = v;
			return;
		}
		search = null;
	};
</script>

<div>
	<input on:keyup={setSearch} on:blur={resetSearch} on:focus={resetSearch} type="text" />
</div>

<div>
	{#if search != null}
		Filtering for: {search}
	{/if}

	Current filter:
	{#if tags != null}
		{mainAttrib}
		<button on:click={resetTags}>Clear</button>
	{/if}

	<!-- <div use:clickOutside on:click_outside={e=>console.log("clicked outside!")}></div> -->

	{#each Object.entries(skills) as [key, { title, items }]}
		<div class={key}>
			<h2>{title}</h2>
			<ul>
				{#each items as item}
					{#if tags == null || tags == item.tags}
						<li class={item.tags}>
							<div on:keydown={null} on:click={() => selectTag(item.tags)}>
								<div>
									{item.title}
								</div>
								<div>
									{item.description}
								</div>
							</div>
						</li>
					{/if}
				{/each}
			</ul>
		</div>
	{/each}
</div>

<style lang="scss">
	$font-stack: Helvetica, sans-serif;
	$primary-color: #333;
    

</style>
