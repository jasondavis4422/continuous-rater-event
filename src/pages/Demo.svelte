<script>
	import { createEventDispatcher } from 'svelte';
	import RatingBox from '../RatingBox.svelte';
	import { experiment } from '../utils.js';
  
	const dispatch = createEventDispatcher();
	export let time = 0;
	export let ratingType; // `ratingType` should be passed as a prop
  
	let pathway = `${experiment}/demo`;
	let paused = false;
	let rating = 50.0;
	// New variable to hold the instruction text
	let instructions = '';
  
	function handleEnd() {
	  dispatch('finished');
	}
  
	function handleBack() {
	  dispatch('back');
	}
  
	function updateInstructions() {
	  if (ratingType === "narrative") {
		instructions = `
		  <p><strong>Space bar:</strong> indicate when shift has occurred.</p>
		  <p><strong>Up arrow key:</strong> move rating bar up.</p>
		  <p><strong>Down arrow key:</strong> move rating bar down.</p>
		  <p>Press the space bar immediately any time there is a change in the location or setting in which the narrative is taking place (e.g., a scene in a home shifts to an outdoor setting).</p>
		  <p>To indicate that you are paying attention, please randomly press up and down on the rating bar during parts of the movie when nothing is changing.</p>
		  <p><strong>If you do not complete this, it will be assumed you are not paying attention and you will not receive compensation.</strong></p>
		`;
	  } else if (ratingType === "emotional") {
		instructions = `
		  <p><strong>Space bar:</strong> indicate when shift has occurred.</p>
		  <p><strong>Up arrow key:</strong> move rating bar up.</p>
		  <p><strong>Down arrow key:</strong> move rating bar down.</p>
		  <p>Press the space bar immediately any time there is a change in the emotional tone of the narrative (e.g., a pleasant conversation turns unpleasant, or vice versa).</p>
		  <p>To indicate that you are paying attention, please randomly press up and down on the rating bar during parts of the movie when nothing is changing.</p>
		  <p><strong>If you do not complete this, it will be assumed you are not paying attention and you will not receive compensation.</strong></p>
		`;
	  } else if (ratingType === "social") {
		instructions = `
		  <p><strong>Space bar:</strong> indicate when shift has occurred.</p>
		  <p><strong>Up arrow key:</strong> move rating bar up.</p>
		  <p><strong>Down arrow key:</strong> move rating bar down.</p>
		  <p>Press the space bar immediately any time there is a change in the social dynamics (e.g., a new person enters the room, or a person leaves; e.g., three people are talking and two of them decide to exclude the third person).</p>
		  <p>To indicate that you are paying attention, please randomly press up and down on the rating bar during parts of the movie when nothing is changing.</p>
		  <p><strong>If you do not complete this, it will be assumed you are not paying attention and you will not receive compensation.</strong></p>
		`;
	  } else if (ratingType === "auditory soundtrack") {
		instructions = `
		  <p><strong>Space bar:</strong> indicate when shift has occurred.</p>
		  <p><strong>Up arrow key:</strong> move rating bar up.</p>
		  <p><strong>Down arrow key:</strong> move rating bar down.</p>
		  <p>Press the space bar immediately any time there is a change in the auditory soundtrack of the movie (e.g., a faster-paced action song plays in the background; e.g., a flashback occurs, and the music changes to match the flashback).</p>
		  <p>To indicate that you are paying attention, please randomly press up and down on the rating bar during parts of the movie when nothing is changing.</p>
		  <p><strong>If you do not complete this, it will be assumed you are not paying attention and you will not receive compensation.</strong></p>
		`;
	  } else if (ratingType === "visual scene") {
		instructions = `
		  <p><strong>Space bar:</strong> indicate when shift has occurred.</p>
		  <p><strong>Up arrow key:</strong> move rating bar up.</p>
		  <p><strong>Down arrow key:</strong> move rating bar down.</p>
		  <p>Press the space bar immediately any time there is a change in the visual scene of the movie (e.g., the movie switches to a new location; e.g., a flashback occurs, and this takes the viewer to a previous timepoint in a new place).</p>
		  <p>To indicate that you are paying attention, please randomly press up and down on the rating bar during parts of the movie when nothing is changing.</p>
		  <p><strong>If you do not complete this, it will be assumed you are not paying attention and you will not receive compensation.</strong></p>
		`;
	  } else {
		instructions = "Invalid rating type. Please check the rating type variable.";
	  }
	}
  
	// Call the function when the component mounts to initialize instructions.
	updateInstructions();
  </script>
  
  <style>
	p {
	  font-weight: normal;
	  padding: none;
	}
	.container {
	  display: flex;
	  flex-direction: column;
	  align-items: center;
	  padding: 1em;
	  margin: 0 auto !important;
	  min-width: 400px !important;
	  max-width: 1000px !important;
	}
	.next {
	  background-color: lightblue;
	}
	.back {
	  background-color: lightcoral;
	}
	.key-box {
	  align-items: center;
	  text-align: center;
	  margin: 0 auto !important;
	}
	.keys {
	  align-items: center;
	  text-align: left;
	  margin: 0 auto !important;
	  width: 25%;
	  padding: 2%;
	  background-color: rgba(255, 255, 255, 0.4);
	  border: 2px solid grey;
	  border-radius: 2px;
	  box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);
	}
	h1 {
	  text-align: center;
	}
  </style>
  
  <div>
	<h1>Demo</h1>
	<div class="container">
	  <RatingBox
		pathway={pathway}
		rating={rating}
		bind:time={time}
		paused={paused}
		ratingType={ratingType}
		ratingBoxUse='demo page'>
	  </RatingBox>
	</div>
	<div class="key-box">
	  <div class="keys">
		<!-- Use the instructions variable directly -->
		<div>{@html instructions}</div>
	  </div>
	  <br>
	  <button class="back" on:click={handleBack}>Back</button>
	  <button class="next" on:click={handleEnd}>Next</button>
	</div>
  </div>