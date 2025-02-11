<script>
    import { filterStore } from "$lib/store/Zoekfilter.store";

    let categories;
    let types;

    filterStore.subscribe((state) => {
        categories = state.categories;
        types = state.types;
    });

    function handleFilter() {
        filterStore.update((state) => ({
            ...state,
            categories,
            types,
        }));
    }
</script>

<div class="bg-blue-50 p-6 rounded-lg w-full max-w-sm">
    <p class="text-xl font-semibold text-blue-700 mb-4">Filteren</p>
    <hr class="border-blue-300 mb-6" />

    <div class="mb-6">
        <p class="text-lg text-blue-800 font-medium">Categorie</p>
        <hr class="border-blue-200 my-3" />
        <ul class="space-y-2">
            {#each Object.entries(categories) as [category, checked]}
                <li class="flex items-center">
                    <label class="flex items-center cursor-pointer">
                        <input
                            type="checkbox"
                            bind:checked={categories[category]}
                            on:change={handleFilter}
                            class="form-checkbox text-blue-600 mr-2"
                        />
                        {category}
                    </label>
                </li>
            {/each}
        </ul>
    </div>

    <div class="mb-6">
        <p class="text-lg text-blue-800 font-medium">Soort</p>
        <hr class="border-blue-200 my-3" />
        <ul class="space-y-2">
            {#each Object.entries(types) as [type, checked]}
                <li class="flex items-center">
                    <label class="flex items-center cursor-pointer">
                        <input
                            type="checkbox"
                            bind:checked={types[type]}
                            on:change={handleFilter}
                            class="form-checkbox text-blue-600 mr-2"
                        />
                        {type}
                    </label>
                </li>
            {/each}
        </ul>
    </div>

    <hr class="border-blue-300 mb-6" />

    <div class="flex justify-center">
        <button
            on:click={handleFilter}
            class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-6 rounded-lg shadow-md transition-all duration-300"
        >
            Filteren
        </button>
    </div>
</div>
