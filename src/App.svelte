<script>
	import { onMount } from "svelte";
	import Grid from "gridjs-svelte";
  
	let jsonData = [];
	let tableVisible = false;
  
	onMount(async () => {
	  await fetchData();
	  tableVisible = true;
	});
  
	async function fetchData() {
	  const response = await fetch("https://api.recruitly.io/api/candidate?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E");
	  const responseData = await response.json();
	  jsonData = responseData.data.map(item => ({
		firstName: item.firstName || "", 
		surname: item.surname || "",
		email: item.email || "",
		mobile: item.mobile || "",
	  }));
	}
  
	function showPopup(firstName) {
	  // Define your logic for displaying a popup or performing any action here
	  console.log(`Showing popup for ${firstName}`);
	}
  </script>
  
  <main class="container mt-4">
	{#if tableVisible}
	  <Grid
		search
		sort
		pagination={{ enabled: true, limit: 8 }}
		data={jsonData}
		columns={[
		  {
			name: "firstName",
			formatter: (cell) => {
			  const firstName = cell.data;
			  if (!firstName) {
				return `<a href="#" on:click={() => showPopup('${firstName}')}>${firstName}</a>`;;
			  }
			  
			}
		  },
		  "surname",
		  "email",
		  "mobile",
		]}
	  />
	{/if}
  </main>
  
  <style>
	@import "https://cdn.jsdelivr.net/npm/gridjs/dist/theme/mermaid.min.css";
  
	.gridjs-th {
	  background-color: yellow;
	}
  </style>
  
