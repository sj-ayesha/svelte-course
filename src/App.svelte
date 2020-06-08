<script>
  import Header from "./UI/Header.svelte";
  import MeetupGrid from "./Meetups/MeetupGrid.svelte";
  import TextInput from "./UI/TextInput.svelte";
  import Button from "./UI/Button.svelte";
  import EditMeetup from "./Meetups/EditMeetup.svelte";

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
      contact: "code@test.com",
      isFavorite: false
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
      contact: "swim@test.com",
      isFavorite: false
    }
  ];

  let editMode;

  function addMeetup(event) {
    const newMeetup = {
      id: Math.random.toString(),
      title: event.detail.title,
      subtitle: event.detail.subtitle,
      description: event.detail.description,
      address: event.detail.address,
      imageUrl: event.detail.imageUrl,
      contact: event.detail.email
    };

    //   meetups.push(newMeetup); //DOES NOT WORK
    meetups = [newMeetup, ...meetups];
    editMode = null;
  }

  function toggleFavorite(event) {
    const id = event.detail;
    const updatedMeetup = { ...meetups.find(m => m.id === id) };
    updatedMeetup.isFavorite = !updatedMeetup.isFavorite;
    const meetupIndex = meetups.findIndex(m => m.id === id);
    const updatedMeetups = [...meetups];
    updatedMeetups[meetupIndex] = updatedMeetup;
    meetups = updatedMeetups;
  }

  function cancelEdit() {
    editMode = null;
  }
</script>

<style>
  main {
    margin-top: 5rem;
  }

  .meetup-controls {
    margin: 1rem;
  }
</style>

<Header />

<main>
  <div class="meetup-controls">
    <Button on:click={() => (editMode = 'add')}>New Meetup</Button>
  </div>

  {#if editMode === 'add'}
    <EditMeetup on:save="{addMeetup}" on:cancel="{cancelEdit}"/>
  {/if}
  <MeetupGrid {meetups} on:togglefavorite={toggleFavorite} />
</main>
