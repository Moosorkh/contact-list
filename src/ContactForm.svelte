<script>
    import contactStore from "./store/contact.store";
    import {onDestroy} from 'svelte';
    let name = "";
    let phone = "";
    let description = "";
    $: title = $contactStore.editId ? 
    'Edit Contact' : 'Add Contact';
    function handleSubmit(){
        if($contactStore.editId === undefined){
            contactStore.add(name, phone, description);
        }else{
            contactStore.edit($contactStore.editId, name, phone, description);
        }
        name = '';
        phone = '';
        description = '';
    }

    const unsub = contactStore.subscribe(({contacts, editId})=>{
        if(!editId)return;
        const contact = contacts.find(c => c.id === editId);
        name = contact.name;
        description = contact.description;
        phone = contact.phone;
    });

    onDestroy(()=>{
        unsub();
    });

</script>
<div class="row">
    <div class="column">
        <h1>{title}</h1>
    </div>
</div>
<div class="row">
    <div class="column">
       <form on:submit|preventDefault={handleSubmit}>
  <fieldset>
    <label for="nameField">Name</label>
    <input bind:value={name} type="text" placeholder="CJ Patoilo" id="nameField">
        <label for="phoneField">Phone</label>
    <input bind:value={phone} type="tel" placeholder="555-5555" id="phoneField">
    <label for="description">Description</label>
    <textarea bind:value={description} id="description"></textarea>
    
    <button class="button" type="submit">{title}</button>
  </fieldset>
</form> 
    </div>
</div>