<script lang="ts">
  import Modal from "$lib/components/Modal.svelte";
import { onMount } from "svelte";
  import Layout from "./+layout.svelte";

    let selectedCountry: string = "Asia/Jakarta";
    let datetime: string = "";

    let showModal: boolean = false;
    let inputCountry: string = "";
    let autocompleteCountries: Array<string> = [];

    const countries: Array<string> = [
        "Asia/Jakarta",
        "Europe/London",
        "America/New_York",
        "Australia/Sydney",
        "Pacific/Auckland",
        "Pacific/Honolulu",
        "America/Los_Angeles",
    ]

    const fetchWordTime = async () => {
        try {
            const result: any = await fetch(`http://worldtimeapi.org/api/timezone/${selectedCountry}`)
            const data = await result.json();
            datetime = data.datetime;
        } catch (e) {
            console.log(e)
        }
        
    }

    const shownModalHandler = () => {
        showModal = !showModal;
    }

    const autoCompleteHandler = (index: number) => {
        inputCountry = autocompleteCountries[index];
    }

    const updateHandler = () => {
        selectedCountry = inputCountry;
        inputCountry = "";

        fetchWordTime();
        shownModalHandler();
    }
    
    $: if (inputCountry) {
        autocompleteCountries = countries.filter(country => {
            if (country.toLowerCase() !== inputCountry.toLowerCase() && country.toLowerCase().includes(inputCountry.toLocaleLowerCase())) {
                return country;
            }
        })
    } else {
        autocompleteCountries = [];
    }

    onMount(() => {
        fetchWordTime();
    })

</script>

<div class="w-full flex flex-col items-center gap-4 mt-20">
    
    <Modal bind:showModal>
        <div class="form-group flex flex-col gap-2">
            <label for="country">Country</label>
            <input
                name="country"
                type="text"
                bind:value={ inputCountry }
                class="border-2 border-gray-700 rounded-md px-2"
            />
            {#each autocompleteCountries as country, i}
                <button class="w-full p-x2 cursor-pointer text-start" on:click={ () => autoCompleteHandler(i) }>{country}</button>
            {/each}
        </div>
        <button
            class="mt-4 px-4 py-2 rounded-md text-sm bg-blue-400 text-white hover:bg-blue-200 transition-colors"
            on:click={ updateHandler }
        >
            Update
        </button>
    </Modal>

    <!-- country -->
    <h2 class="text-center font-bold">{selectedCountry}</h2>
    
    <!-- worldtime -->
    <p>{datetime}</p>

    <!-- button to change the country -->
    <button
        class="mt-4 px-4 py-2 rounded-md text-sm bg-blue-600 text-white hover:bg-blue-400 transition-colors"
        on:click={ shownModalHandler }    
    >
        Change Country
    </button>

</div>


<style lang="postcss">

</style>