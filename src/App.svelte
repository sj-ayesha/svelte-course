<script>
  import Header from "./UI/Header.svelte";
  import MeetupGrid from "./Meetups/MeetupGrid.svelte";
  import TextInput from "./UI/TextInput.svelte";
  import Button from './UI/Button.svelte';

  let title = "";
  let subtitle = "";
  let address = "";
  let email = "";
  let description = "";
  let imageUrl = "";

  let meetups = [
    {
      id: "m1",
      title: "Coding Bootcamp",
      subtitle: "Learn to code in 2hours",
      description:
        "In this meetup, we will have some experts that teach you how to code",
      imageUrl:
        "https://images.adsttc.com/media/images/5515/bff1/e58e/ceba/3f00/016a/large_jpg/shutterstock_148972376.jpg?1427488734",
      address: "27th Nerd Road, 32523 NY",
      contact: "code@test.com"
    },
    {
      id: "m2",
      title: "Swim Together",
      subtitle: "Learn how to swim",
      description:
        "In this meetup, we will have some experts that teach you how to swim",
      imageUrl:
        "https://scitechdaily.com/images/Man-in-Swimming-Pool-777x518.jpg",
      address: "27th Nerd Road, 32523 NY",
      contact: "swim@test.com"
    }
  ];

  function addMeetup() {
    const newMeetup = {
      id: Math.random.toString(),
      title: title,
      subtitle: subtitle,
      description: description,
      address: address,
      imageUrl: imageUrl,
      contact: email
    };

    //   meetups.push(newMeetup); //DOES NOT WORK
    meetups = [newMeetup, ...meetups];
  }
</script>

<style>
  main {
    margin-top: 5rem;
  }

  form {
      width: 30rem;
      max-width: 90%;
      margin: auto;
  }
</style>

<Header />

<main>
  <form id="form-meetup" on:submit|preventDefault={addMeetup}>
    <TextInput
      id="title"
      label="Title"
      type="text"
      value={title}
      on:input={event => {
        title = event.target.value;
      }} />

    <TextInput
      id="subtitle"
      label="Subtitle"
      type="text"
      value={subtitle}
      on:input={event => {
        subtitle = event.target.value;
      }} />
    <TextInput
      controlType="textarea"
      rows="3"
      id="description"
      label="description"
      value={description}
      on:input={event => {
        description = event.target.value;
      }} />
    <TextInput
      id="address"
      label="Address"
      type="text"
      value={address}
      on:input={event => {
        address = event.target.value;
      }} />
    <TextInput
      id="imageUrl"
      label="ImageUrl"
      type="text"
      value={imageUrl}
      on:input={event => {
        imageUrl = event.target.value;
      }} />
    <TextInput
      id="email"
      label="E-mail"
      type="email"
      value={email}
      on:input={event => {
        email = event.target.value;
      }} />
    <Button type="submit" caption="Save" />
  </form>
  <MeetupGrid {meetups} />
</main>
