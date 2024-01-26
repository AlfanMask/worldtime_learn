<script lang="ts">
    export let showModal: boolean = false;

    const closeHandler = () => {
        showModal = false;
    }
</script>

<div class="w-1/2 min-h-40">
    {#if showModal}
        <div class="backdrop"></div>
        <div class="content-wrapper">
            <div class="flex justify-end">
                <button on:click={ closeHandler }>x</button>
            </div>
            <slot />
        </div>
    {/if}
</div>

<style lang="postcss">
    .backdrop {
        @apply absolute -z-50 w-screen h-screen bg-black bg-opacity-50 left-0 top-0
    }
    .content-wrapper {
        @apply flex flex-col gap-4 rounded-md bg-white p-4
    }
    button {
        @apply w-6 h-4 flex justify-center items-center font-bold text-end cursor-pointer;
    }
</style>