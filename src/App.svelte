<script lang="ts">
  import logo from "./assets/svelte.png";
  import Counter from "./lib/Counter.svelte";
  import Tailwind from "./Tailwind.svelte";
  import { onMount } from "svelte";
  import { count } from "./lib/stores";
  import { each } from "svelte/internal";

  let selectedCountry = "Thailand";
  let data = [
    {
      updated: 0,
      country: "Thailand",
      cases: "Loading",
      todayCases: "Loading",
      deaths: "Loading",
      todayDeaths: "Loading",
      recovered: "Loading",
      todayRecovered: "Loading",
      active: "Loading",
      critical: "Loading",
      casesPerOneMillion: "Loading",
      deathsPerOneMillion: "Loading",
      tests: "Loading",
      testsPerOneMillion: "Loading",
      population: "Loading",
      continent: "Loading",
      oneCasePerPeople: "Loading",
      oneDeathPerPeople: "Loading",
      oneTestPerPeople: "Loading",
      activePerOneMillion: "Loading",
      recoveredPerOneMillion: "Loading",
      criticalPerOneMillion: "Loading",
    },
  ];

  $: countryData = data.find((value) => value.country == selectedCountry);

  onMount(async () => {
    const url = "https://disease.sh/v3/covid-19/countries";
    const cache = await caches.open("data");
    await cache.add(url);
    const dataC = await cache.match(url);
    data = await dataC.json();
  });
</script>

<Tailwind />

<main
  class="text-center p-4 mx-0 flex flex-col justify-center items-center w-screen min-h-screen overflow-scroll bg-red-300"
>
  <div class="text-5xl">Covid in {selectedCountry}</div>
  <select name="country" bind:value={selectedCountry} class="my-4">
    {#each data as countryData}
      <option value={countryData.country}>{countryData.country}</option>
    {/each}
  </select>
  <div class="grid md:grid-cols-3 md:gap-3 grid-cols-2">
    <div
      class="col-start-1 col-span-2 bg-white rounded-xl flex flex-col justify-between px-4 py-2 w-64"
    >
      <div class="flex flex-col text-center items-center px-3 py-2">
        <div class="text-md font-light">New Cases</div>
        <div class="text-4xl mt-2 text-red-600">{countryData.todayCases}</div>
      </div>
      <div class="h-0.5 bg-gray-300 mx-3 my-1" />
      <div class="flex flex-col text-center items-center px-3 py-2">
        <div class="text-md font-light">Total Cases</div>
        <div class="text-4xl mt-2 text-red-600">{countryData.cases}</div>
      </div>
      <div class="h-0.5 bg-gray-300 mx-3 my-1" />
      <div
        class="flex flex-row w-full text-center justify-around items-center"
      >
        <div class="flex flex-col text-center items-center px-3 py-2">
          <div class="text-md font-light">Active</div>
          <div class="text-xl text-blue-600">{countryData.active}</div>
        </div>

        <div class="w-0.5 h-14 bg-gray-300 mx-1" />

        <div class="flex flex-col text-center items-center px-3 py-2">
          <div class="text-md font-light">Critical</div>
          <div class="text-xl text-blue-600">{countryData.critical}</div>
        </div>
      </div>
    </div>
    <div
      class="flex flex-col mt-3 md:mt-0 col-start-1 col-span-2 md:col-start-3 md:col-span-1 rounded-md justify-between space-y-3"
    >
      <div class="bg-white rounded-md px-2 py-3">
        <div class="text-md font-light">New Deaths</div>
        <div class="text-xl text-gray-500">{countryData.todayDeaths}</div>
        <div class="h-0.5 bg-gray-300 mx-3 my-1" />
        <div class="text-md font-light">Total Deaths</div>
        <div class="text-xl text-gray-500">{countryData.deaths}</div>
      </div>
      <div class="bg-white rounded-md px-2 py-3">
        <div class="text-md font-light">New Recovered</div>
        <div class="text-xl text-green-400">{countryData.todayRecovered}</div>
        <div class="text-md font-light">Total Recovered</div>
        <div class="text-xl text-green-400">{countryData.recovered}</div>
      </div>
      <div class="bg-white rounded-md px-2 py-3">
        <div class="text-md font-light">Test</div>
        <div class="text-xl text-green-400">{countryData.tests}</div>
      </div>
    </div>
  </div>
</main>
