<script lang="js">
	import skills from './skills.json';

	import EntryBox from './EntryBox.svelte';
	import { slide } from 'svelte/transition';

	// import {clickOutside} from './clickOutside.js';

	let tags;
	let mainAttrib;
	let selectedItem;

	$: if (tags) {
		mainAttrib = skills.attributes.items.find((a) => a.tags == tags)?.title;
	}

	let search = null;
	const setSelected = (item) => {
		tags = item.tags;
		selectedItem = item;
	};

	const resetFilters = (e) => (tags = null) && (selectedItem = null) 
	const resetSearch = (e) => (e.target.value.length > 0 ? null : (search = null));
	const setSearch = (e) => {
		const v = e.target && e.target.value;
		if (v.length > 0) {
			search = v;
			return;
		}
		search = null;
	};

	$: console.log(search);
	let runSearch = () => true;
	$: runSearch = (item) => {
		let res =
			(item?.title?.toLowerCase() + item?.description?.toLowerCase()).includes(
				search?.toLowerCase()
			) || search == null;
		console.log(res);
		return res;
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
		{#if tags != null || search != null}
			<div transition:slide>
				Current filter:

				<div class="flex space-x-2">
					{#if tags != null}
						<div class="p-1 border-2">{mainAttrib}</div>
						<div class="p-1 border-2">{selectedItem?.title}</div>
					{/if}
					{#if search != null}
						<div class="p-1 border-2">
							Search [{search}]
						</div>
					{/if}
				</div>

				<button on:click={resetFilters}>Clear</button>
			</div>
		{/if}
	</div>
	<!-- <div use:clickOutside on:click_outside={e=>console.log("clicked outside!")}></div> -->

	<div
		class="flex-1 basis-5/6 grid grid-cols-4 border-2 p-1 m-1"
		on:keypress={null}
		on:click|self={resetFilters}
	>
		{#each Object.entries(skills) as [key, { title, items }]}
			<div class="p-1 m-1 border-2 max-h-screen" on:keypress={null} on:click|self={resetFilters}>
				<h2>{title}</h2>
				<div class="max-h-screen overflow-y-auto">
					{#each items as item}
						{#if (tags == null || tags == item.tags) && runSearch(item)}
							<EntryBox {item} {setSelected} itemSelected={selectedItem == item} />
						{/if}
					{/each}
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
</style>
