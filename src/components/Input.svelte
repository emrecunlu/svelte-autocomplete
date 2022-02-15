<script>
    export let value
</script>

<style>
    input {
        width: 100%;
        height: 40px;
    }
</style>

<input type="text" placeholder="type to search country." on:keyup bind:value={value}>