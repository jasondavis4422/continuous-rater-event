<!-- first instruction page -->
<script>
  // This is the Instructions page. It loops over the instructions array as a user reads and when click to the last page it notifies the main App.svelte component by dispatching a 'finished' event. When the last page of the instructions are reached the forward button turns into a "Take Quiz" button, but currently there is no quiz and it goes straight to the experiment
  import { createEventDispatcher } from 'svelte';
  export let ratingType;

  // Add/remove items here to create more instructions pages
  const ratingInstruct = 'In this task, you will watch a short video, provide ratings related to that video continuously as you watch, and answer a series of follow-up questions. <br><br> Specifically, you will rate when you feel a <strong>' + ratingType + '</strong> shift in the movie has occurred by using your keyboard to control a rating box (which is demoed on the next page) throughout the video. Make sure to indicate when you feel a significant shift in the movie has occurred by simply pressing the space bar and please update your rating consistently throughout the video. </p>'
  
  const instructions = [
    ratingInstruct
  ];

  const dispatch = createEventDispatcher();
  let currentPage = 0;

    function handleEnd() {
		dispatch('finished');
    };

  const backward = () => {
    currentPage -= 1;
    currentPage = Math.max(currentPage, 0);
  };
  const forward = () => {
    // Check if we're increasing the value of currentPage beyond the number of instructions, if so tell App.svelte we're ready to move to the quiz
    if (currentPage + 1 === instructions.length) {
      dispatch('finished');
    } else {
      currentPage += 1;
      currentPage = Math.min(currentPage, instructions.length - 1);
    }
  };
</script>

<style>
  .container {
    margin: 0 auto !important; 
    max-width: 800px;
    text-align: center;
  }

  .text-box {
        text-align: left;
		padding: 2%;
		background-color: rgba(255, 255, 255, 0.4);
		border: 2px solid grey;
		border-radius: 2px;
		box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);   
	}


  
  .controls {
    min-width: 25%;
  }

  button {
    background-color: lightblue;
  } 
</style>
 

<div class="container">
  <h1>Instructions</h1>  
  <div class="text-box">
    <div class="content">
      {@html instructions[currentPage]}
    </div>
  </div>
  <br>
  <button class="button is-link controls" on:click={handleEnd}>
    {#if currentPage === instructions.length - 1}
      Go To Demo
    {:else}
      <span class="icon">
        Next
        <i class="fas fa-forward" />
      </span>
    {/if}
  </button>
</div>
