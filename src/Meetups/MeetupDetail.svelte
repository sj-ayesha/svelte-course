<script>
  import { onDestroy, createEventDispatcher } from "svelte";
  import meetups from "./meetups-store.js";
  import Button from "../UI/Button.svelte";

  export let id;

  let selectedMeetup;

  let dispatch = createEventDispatcher();

  const unsubscribe = meetups.subscribe(items => {
    selectedMeetup = items.find(i => i.id === id);
  });

  onDestroy(() => {
      unsubscribe();
  });
</script>

<style>
    section {
        margin-top: 4rem;
    }

    .image {
        width: 100%;
        height: 25rem;
    }

    img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

</style>

<section>
  <div class="image">
    <img src="{selectedMeetup.imageUrl}" alt="{selectedMeetup.title}" />
  </div>

  <div class="content">
    <h1>{selectedMeetup.title}</h1>
    <h2>{selectedMeetup.subtitle} - {selectedMeetup.address}</h2>
    <p>{selectedMeetup.description}</p>
    <Button href="maillto:{selectedMeetup.contact}">Contact</Button>
    <Button type="button" mode="outline" on:click={() => dispatch('close')}>Close</Button>
  </div>
</section>
