<script>
  import { createEventDispatcher } from "svelte";
  import TextInput from "../UI/TextInput.svelte";
  import Button from "../UI/Button.svelte";
  import Modal from "../UI/Modal.svelte";
  import { isEmpty, isValidEmail } from "../helpers/validation.js";
  import meetups from "./meetups-store.js";

  export let id = null;

  let title = "";
  let titleValid = false;
  let subtitle = "";
  let subtitleValid = false;
  let address = "";
  let addressValid = false;
  let email = "";
  let emailValid = false;
  let description = "";
  let descriptionValid = false;
  let imageUrl = "";
  let imageUrlValid = false;
  let formIsValid = false;

  if (id) {
    const unsubscribe = meetups.subscribe(items => {
      const selectedMeetup = items.find(i => i.id === id);
      title = selectedMeetup.title;
      subtitle = selectedMeetup.subtitle;
      address = selectedMeetup.address;
      email = selectedMeetup.contact;
      description = selectedMeetup.description;
      imageUrl = selectedMeetup.imageUrl;
    });

    unsubscribe();
  }

  let dispatch = createEventDispatcher();

  $: titleValid = !isEmpty(title);
  $: subtitleValid = !isEmpty(subtitle);
  $: addressValid = !isEmpty(address);
  $: emailValid = !isValidEmail(email);
  $: descriptionValid = !isEmpty(description);
  $: imageUrlValid = !isEmpty(imageUrl);
  $: formIsValid =
    titleValid &&
    subtitleValid &&
    addressValid &&
    emailValid &&
    descriptionValid &&
    imageUrlValid;

  function submitForm() {
    const meetupData = {
      title: title,
      subtitle: subtitle,
      description: description,
      address: address,
      imageUrl: imageUrl,
      contact: email
    };

    if (id) {
      meetups.updateMeetup(id, meetupData);
    } else {
      //   meetups.push(newMeetup); //DOES NOT WORK
      meetups.addMeetup(meetupData);
    }

    dispatch("save");
  }

  function cancel() {
    dispatch("cancel");
  }

  function deleteMeetup() {
    meetups.deleteMeetup(id);
    dispatch('save');
  }
</script>

<style>
  form {
    width: 100%;
  }
</style>

<Modal title="Edit Meetup Data" on:cancel>
  <form id="form-meetup" on:submit|preventDefault={submitForm}>
    <TextInput
      id="title"
      label="Title"
      value={title}
      valid={titleValid}
      validityMessage="Please enter a valid title"
      on:input={event => {
        title = event.target.value;
      }} />

    <TextInput
      id="subtitle"
      label="Subtitle"
      value={subtitle}
      valid={subtitleValid}
      validityMessage="Please enter a valid subtitle"
      on:input={event => {
        subtitle = event.target.value;
      }} />
    <TextInput
      controlType="textarea"
      rows="3"
      id="description"
      label="description"
      value={description}
      valid={descriptionValid}
      validityMessage="Please enter a valid description"
      on:input={event => {
        description = event.target.value;
      }} />
    <TextInput
      id="address"
      label="Address"
      value={address}
      valid={addressValid}
      validityMessage="Please enter a valid address"
      on:input={event => {
        address = event.target.value;
      }} />
    <TextInput
      id="imageUrl"
      label="ImageUrl"
      value={imageUrl}
      valid={imageUrlValid}
      validityMessage="Please enter a valid imageUrl"
      on:input={event => {
        imageUrl = event.target.value;
      }} />
    <TextInput
      id="email"
      label="E-mail"
      type="email"
      value={email}
      valid={emailValid}
      validityMessage="Please enter a valid email"
      on:input={event => {
        email = event.target.value;
      }} />
  </form>
  <div slot="footer">
    <Button type="button" mode="outline" on:click={cancel}>Cancel</Button>
    <Button type="button" on:click={submitForm} disabled={!formIsValid}>
      Save
    </Button>
    {#if id}
      <Button type="button" on:click={deleteMeetup}>Delete</Button>
    {/if}
  </div>
</Modal>
