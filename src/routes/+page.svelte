<script>
    import LoginModal from "$lib/components/LoginModal.svelte";
    import Navbar from "$lib/components/Navbar.svelte";
    import Products from "$lib/components/Products.svelte";
    import FInfo from "$lib/components/footerInfo.svelte";

    let isLoginModalOpen = false;
</script>

{#if isLoginModalOpen}
    <LoginModal on:close={isLoginModalOpen = false}/>
{/if}

<div class="{isLoginModalOpen ? 'blur-sm' : ''}">

    <Navbar on:openLoginModal={isLoginModalOpen = true} />
    <Products />
    <FInfo />
    
</div>