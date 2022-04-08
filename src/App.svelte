<script>
  import Agenda from "@nylas/components-agenda";

  let today = new Date();

  let week = [];

  $: week = [...Array(7).keys()].map(day => {
    let first = today.getDate() - today.getDay() + day;
    return new Date(today.setDate(first));
  });

  function onEventClicked(_, event) {
    if (event.participants.length === 0) {
      return;
    }

    const contacts = event.participants
      .map(participant => participant.email)
      .join(";");
    window.location.href = `mailto:${contacts}`;
  }
</script>

<style>
  iframe {
    display: none;
  }
  main {
    text-align: center;
    padding: 0;
    margin: 0 auto;
    height: 100vh;
    width: 100vw;
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    grid-template-rows: 30px 1fr;
    gap: 0.5rem;
  }
  header {
    grid-column: -1 / 1;
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
</style>

<main>
  {#await Agenda}
    loading...
  {:then a}
    <header>
      <button
        on:click={() => (today = new Date(today.getFullYear(), today.getMonth(), today.getDate() - 7))}>&laquo;</button>
      <button
        on:click={() => (today = new Date(today.getFullYear(), today.getMonth(), today.getDate() + 7))}>&raquo;</button>
    </header>
    {#each week as day}
      <nylas-agenda
        click_action={onEventClicked}
        header_type="none"
        selected_date={day}
        allow_date_change={false}
        id="6484c482-4eea-45fb-bd74-331756839f27" />
    {/each}
  {/await}
</main>