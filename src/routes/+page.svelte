<script lang="ts">
	import { localStorageStore } from "@skeletonlabs/skeleton";
	import { writable, type Writable } from 'svelte/store';
	
	let inputName = "";
	let inputNumber ="";

	//writable type
	interface contact{
		name: string
		phoneNumber:string
	}

    //creating localStorageStore
	const contactStore: Writable<contact[]> = localStorageStore("contactStore", [])

	//add contact function
	function addContact() {
		$contactStore = [
			{
				name: inputName,
				phoneNumber: inputNumber
			},
			...$contactStore
		]

		inputName = "";
		inputNumber ="";
	}

	//delete contact function
	function deleteContact(index=-1){
		if ( window.confirm("are our sure you want to delete " + $contactStore[index].name)){
			 $contactStore = $contactStore.filter((contact, contactIndex) => contactIndex !==index);
		}
	}
</script>

<main class="container h-full mx-auto flex justify-center items-center">
	<div class="space-y-5">
		<h1 class="h1">contactList</h1>
		<p>All your contacts in one persistent state!</p>

		<label class="label">
			<span>Name</span>
			<input bind:value={inputName} type="text" class="input" placeholder="Name">
		</label>

		<label class="label">
			<span>Phone Number</span>
			<input bind:value={inputNumber} type="text" class="input" placeholder="Phone Number">
		</label>

		<button on:click={addContact} type="button" class="btn variant-filled">Add contact</button>
		<hr />

		<h2 class="h2">Your contacts</h2>
		{#each $contactStore as contact, index}
			<div class="card p-2">
				<h1>{contact.name}</h1>
				<h1>{contact.phoneNumber}</h1>
				<button on:click={() =>deleteContact(index)} type="button" class="btn btn-sm variant-filled-error">Delete</button>
			</div>
		{/each}
	</div>
</main>