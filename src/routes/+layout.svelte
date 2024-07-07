<script lang="ts">

</script>

<main class="hero">
	<div class="page">
		<slot />
	</div>
</main>

<style>
    .hero {
        position: relative;
        height: 100vh;
        background: url('/images/internet.jpg') no-repeat center center;
        background-size: cover;
        align-items: center;
        justify-content: center;
        color: white;
        text-align: center;
        transition: opacity 1s;
        opacity: 1;
        width: 100%;
        overflow: hidden;
    }

    .hero::before {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(30, 33, 72, 0.3); /* オーバーレイの色と透明度 */
    }

    .page {
        position: relative;
        transition: opacity 1.5s;

    }
</style>
