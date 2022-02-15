<script>
    import Input from "./components/Input.svelte";
    import {countries} from "./components/stores";

    let value = ''

    let filtered = []

    $: if (value === '') filtered = []
    $: if ($countries.length === filtered.length) filtered = []

    const searchCountry = () => {
        filtered = $countries.filter(country => {
            return country.name.toLowerCase().includes(value.toLowerCase())
        }).map(country => {
            let starting = country.name.toLowerCase().indexOf(value.toLowerCase())
            let founding = country.name.slice(starting, starting + value.length)

            return country.name.replace(founding, `<b>${country.name.slice(starting, starting + value.length)}</b>`)
        })

    }

    const changeInput = (country) => {
        value = country.replace(/<[/]?(b)>/gi, '')

        filtered = []
    }


</script>

<style>
    :global(*) {
        margin: 0;
        padding: 0;
        list-style-type: none;
        text-decoration: none;
        border: 0;
        outline: 0;
    }

    :global(.container) {
        width: 100%;
        max-width: 556px;
        margin: 30px auto;
        box-sizing: border-box;
    }

    :global(body) {
        background: #333333;
    }

    .countries ul {
        display: flex;
        flex-direction: column;
        row-gap: 10px;
    }

    .countries {
        background: #fff;
        border-radius: 4px;
        overflow: hidden;
        height: 100%;
        max-height: 300px;
        overflow-y: auto;
    }

    .countries ul li {
        display: block;
        color: #000;
        padding: 12px 15px;
        border-radius: 4px;
        cursor: pointer;
    }

    .countries ul li:hover {
        background: #eeee;
    }
</style>

<div class="container">
    <Input bind:value={value} on:keyup={searchCountry}/>
    <div class="countries">
        {#if filtered.length > 0}
            <ul>
                {#each filtered as country, index (index)}
                    <li on:click={changeInput(country)}>{@html country}</li>
                {/each}
            </ul>
        {/if}
    </div>
</div>