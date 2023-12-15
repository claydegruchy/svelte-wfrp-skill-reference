<script lang="js">
	import skills from './skills.json';

	import EntryBox from './EntryBox.svelte';
	import { slide } from 'svelte/transition';

	// import {clickOutside} from './clickOutside.js';

	let tags;
	let mainAttrib;
	let selectedItem;

	let searchBox;
	let searchValue;

	$: if (tags) {
		mainAttrib = skills.attributes.items.find((a) => a.tags == tags)?.title;
	}

	const setSelected = (item) => {
		tags = item.tags;
		selectedItem = item;
	};

	const resetFilters = (e) => {
		tags = null;
		selectedItem = null;
		searchValue = null;
	};

	$: if (!searchValue && searchValue == '') searchValue = null;

	let runSearch = () => true;
	$: runSearch = (item) => {
		let res =
			(item?.title?.toLowerCase() + item?.description?.toLowerCase()).includes(
				searchValue?.toLowerCase()
			) || searchValue == null;
		return res;
	};
</script>

<div class="h-screen border-8 p-1  flex flex-col">
	<div class="flex-1 p-1 m-1">
		<input
			bind:this={searchBox}
			bind:value={searchValue}
			placeholder="Search"
			type="text"
			class=" border-2 p-0.5 m-1 w-full"
		/>
	</div>

	<div class="flex-1 m-1 p-1 2">
		{#if tags != null || searchValue != null}
			<div transition:slide>
				Current filter:

				<div class="flex space-x-2">
					{#if tags != null}
						<div class="p-1 border-2">{mainAttrib}</div>
						<div class={'p-1 border-2 ' + selectedItem?.tags}>{selectedItem?.title}</div>
					{/if}
					{#if searchValue != null}
						<div class="p-1 border-2">
							Search [{searchValue}]
						</div>
					{/if}
				</div>

				<!-- slightly bigger text, padding -->
				<button class="p-1 border-2 text-2xl " on:click={resetFilters}>Clear</button>
			</div>
		{/if}
	</div>
	<!-- <div use:clickOutside on:click_outside={e=>console.log("clicked outside!")}></div> -->

	<!-- hide overflow in this grid -->
	<div
		class="flex-1 basis-5/6 grid grid-cols-4 border-2 p-1 m-1 overflow-y-auto"
		on:keypress={null}
		on:click|self={resetFilters}
	>
		{#each Object.entries(skills) as [key, { title, items }]}
			<div class="p-1 m-1 border-2 " on:keypress={null} on:click|self={resetFilters}>
				<h2>{title} - ({items.length})</h2>
				<div class="object-contain">
					{#each items as item}
						{#if (tags == null || tags == item.tags) && runSearch(item)}
							<EntryBox class={tags} {item} {setSelected} itemSelected={selectedItem == item} />
						{/if}
					{/each}
				</div>
			</div>
		{/each}
	</div>
</div>
