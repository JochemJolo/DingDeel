<script>
    import Navbar from "$lib/components/Navbar.svelte";
    import Products from "$lib/components/Products.svelte";
    import FInfo from "$lib/components/footerInfo.svelte";
</script>

<h1>Welcome to SvelteKit</h1>
<p class="text-red-500">
    Visit <a href="about.svelte">svelte.dev/docs/kit</a> to read the documentation
</p>

<Products />
<FInfo />
