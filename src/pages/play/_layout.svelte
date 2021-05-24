<!-- routify:options preload="proximity" -->

<script>
    import Navbar from '../../components/Navbar.svelte'
</script>

<Navbar />

<div>
    <slot />

</div>
<style>
    div {
        height: 88vh;
        overflow: hidden;

    }
</style>    