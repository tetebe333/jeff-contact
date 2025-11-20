<script lang="ts">
	import { localStorageStore } from "@skeletonlabs/skeleton";
	import { writable, type Writable } from 'svelte/store';
	
	let inputName = "";
	let inputNumber ="";
	let dataIndex:any = null;
	let btnState = true;

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

	function editContact(index = -1){
		 dataIndex = index

		inputName = $contactStore[index].name 
		inputNumber = $contactStore[index].phoneNumber

		btnState = false
	}

	function updateContact(){
		$contactStore[dataIndex].name = inputName
		$contactStore[dataIndex].phoneNumber = inputNumber 

		 dataIndex = null

		inputName = "";
		inputNumber ="";

		btnState = true
	}

	function cancle(){ 

		 dataIndex = null

		inputName = "";
		inputNumber ="";

		btnState = true
	}

	//delete contact function
	function deleteContact(index=-1){
		if ( window.confirm("are our sure you want to delete " + $contactStore[index].name)){
			 $contactStore = $contactStore.filter((contact, contactIndex) => contactIndex !==index);
		}
	}
</script>

<main class="container h-full mx-auto flex justify-center items-center py-7">
	<div class="space-y-5">
		<h1 class="h1">Contact List</h1>
		<p>All your contacts in one persistent state!</p>

		<label class="label">
			<span>Name</span>
			<input bind:value={inputName} type="text" class="input" placeholder="Name">
		</label>

		<label class="label">
			<span>Phone Number</span>
			<input bind:value={inputNumber} type="text" class="input" placeholder="Phone Number">
		</label>

		{#if btnState == true}
			<button on:click={addContact} type="button" class="btn variant-filled">Add contact</button>
		{/if}

		{#if btnState == false}
			<button on:click={updateContact} type="button" class="btn variant-filled">update contact</button>
			<button on:click={cancle} type="button" class="btn variant-filled">Cancle</button>
		{/if}
		


		<hr />

		<h2 class="h2">Your contacts</h2>
		{#each $contactStore as contact, index}
			<div class="card p-2">
				<h1>{contact.name}</h1>
				<h1>{contact.phoneNumber}</h1>
				<button on:click={() =>deleteContact(index)} type="button" class="btn btn-sm variant-filled-error">Delete</button>
				<button on:click={() =>editContact(index)} type="button" class="btn btn-sm variant-filled-error ms-4">Edith</button>
			</div>
		{/each}
	</div>
</main>