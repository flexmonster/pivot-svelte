<script>
    import { onMount } from "svelte";
    import "../styles.css";
    import SideMenu from "../components/SideMenu.svelte";
    import TopMenu from "../components/TopMenu.svelte";
</script>

<section id="app">
    <TopMenu></TopMenu>
    <div class="wrap">
        <SideMenu></SideMenu>
        <div class="pivot-example-container">
            <slot />
        </div>
    </div>
</section>

