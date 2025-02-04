<!-- <script>
    import { writable } from 'svelte/store';
    let isOpen = writable(true);
</script> -->


<p class="font-bold">filteren</p>
<hr class="h-px my-8 bg-gray-200 border-0 dark:bg-gray-700">

<div class="p-4 border-r">
    <p>category</p>
    <hr class="h-px my-4 bg-gray-200 border-0 dark:bg-gray-700">
    <ul class="transition-all duration-300">
        <li class="py-1 flex items-center">
          <input type="checkbox" class="mr-2"> Auto
        </li>
        <li class="py-1 flex items-center">
          <input type="checkbox" class="mr-2"> Kleding
        </li>
        <li class="py-1 flex items-center">
          <input type="checkbox" class="mr-2"> Gereedschap
        </li>
        <li class="py-1 flex items-center">
            <input type="checkbox" class="mr-2"> Mensen
          </li>
    </ul>
</div>
<div class="p-4 border-r">
    <p>soort</p>
    <hr class="h-px my-4 bg-gray-200 border-0 dark:bg-gray-700">
    <ul class="transition-all duration-300">
        <li class="py-1 flex items-center">
          <input type="checkbox" class="mr-2"> Ruilen
        </li>
        <li class="py-1 flex items-center">
          <input type="checkbox" class="mr-2"> Kopen
        </li>
        <li class="py-1 flex items-center">
          <input type="checkbox" class="mr-2"> Lenen
        </li>
    </ul>
</div>

<hr class="h-px my-8 bg-gray-200 border-0 dark:bg-gray-700">
<button class="bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded">
    filteren
</button>
<!-- ook de catogorie kunnen minimizen^ -->
<!--  class:hidden={!$isOpen} in de eerste UL -->