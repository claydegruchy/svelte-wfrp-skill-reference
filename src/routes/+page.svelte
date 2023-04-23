<script lang="js">
	import skills from './skills.json';

	// import {clickOutside} from './clickOutside.js';

	let tags;
	let mainAttrib;
	let selected;

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

<div class="h-screen border-8 p-1 overflow-hidden flex flex-col">
	<div class="flex-1 p-1 m-1">
		<input
			on:keyup={setSearch}
			on:blur={resetSearch}
			on:focus={resetSearch}
			placeholder="Search"
			type="text"
			class=" border-2 p-0.5 m-1 w-full"
		/>
	</div>

	<div class="flex-1 m-1 p-1 2">
		{#if search != null}
			Filtering for: {search}
		{/if}
		{#if tags != null}
			Current filter:
			{mainAttrib}
			<button on:click={resetTags}>Clear</button>
		{/if}
	</div>
	<!-- <div use:clickOutside on:click_outside={e=>console.log("clicked outside!")}></div> -->

	<div class="flex-1 basis-5/6 grid grid-cols-4 border-2 p-1 m-1">
		{#each Object.entries(skills) as [key, { title, items }]}
			<div class="p-1 m-1 border-2 max-h-screen">
				<h2>{title}</h2>
				<div class="max-h-screen overflow-y-auto">
					{#each items as item}
						{#if tags == null || tags == item.tags}
							<div class={item.tags}>
								<div
									class="group border-2 p-1 m-1"
									on:keydown={null}
									on:click={() => selectTag(item.tags)}
								>
									<div>
										{item.title}
									</div>
									<div
										class={`h-0 group-hover:h-max hidden group-hover:contents transition-all ${
											selected == item.title ? 'contents h-max' : ''
										}`}
									>
										{item.description}
									</div>
								</div>
							</div>
						{/if}
					{/each}
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
</style>
