<script>
  import { createEventDispatcher } from 'svelte'
  import { onMount } from 'svelte'

  import config from '../../config.js'
  import environment from '../../environment.js'

  let generatedResults = [
    {
      faction: 'aquila',
      names: ['', '', '', '', '', ''],
    },
    {
      faction: 'dugo',
      names: ['', '', '', '', '', ''],
    },
    {
      faction: 'ekanesh',
      names: ['', '', '', '', '', ''],
    },
    {
      faction: 'pendzal',
      names: ['', '', '', '', '', ''],
    },
    {
      faction: 'sona',
      names: ['', '', '', '', '', ''],
    },
  ]
  const dispatch = createEventDispatcher()

  onMount(() => {
    rollNewNames()
    setTimeout(function () {
      rollNewNames()
    }, 500)
  })

  async function rollNewNames() {
    generatedResults = await Promise.all(
      config.Factions.map(getThisFactionNames),
    ).then(dispatch('rolledNames', generatedResults))
  }

  async function getThisFactionNames(selectedFaction) {
    let fetchResult = await fetch(
      environment.self + '/names?faction=' + selectedFaction + '&amount=6',
      // DO NOT COMMIT WITH ABOVE LINE IN PLACE
      // LIVE BELOW
      //'./api/names?faction=' + selectedFaction + '&amount=6',
    )
    let jsonResult = await fetchResult.json()
    return { faction: selectedFaction, names: jsonResult }
  }
</script>

<style>
  button {
    top: clamp(1.25em, 2em, calc(5% - 1.75rem));
    right: 10%;
    z-index: 2;
    position: absolute;
    font-size: 1rem;
    padding: 0.5rem;
    margin-bottom: -1rem;
    color: #ccd1dd;
    background: #386ae8;
  }
  button:before {
    content: 'Roll New Names ';
  }
  button:after {
    content: '🎲';
  }
  button:active:after {
    display: inline-block;
    content: '🎲';
    animation-name: rollDice;
    animation-duration: 0.4s;
    animation-iteration-count: infinite;
  }
  @keyframes rollDice {
    0% {
      transform: rotate(0deg) translate(0, 0);
    }
    33% {
      transform: rotate(120deg) translate(0.09375em, -0.09375em);
    }
    66% {
      transform: rotate(240deg) translate(-0.09375em, -0.09375em);
    }
    100% {
      transform: rotate(360deg) translate(0, 0);
    }
  }
  /* Tablet size or smaller */
  @media screen and (max-width: 80.5em) {
    button {
      border-radius: 50%;
      width: 3.5rem;
      height: 3.5rem;
      font-size: 2rem;
      padding: 0rem;
      margin-bottom: 0rem;
      box-shadow: 0 3px 5px -1px rgba(0, 0, 0, 0.2),
        0 6px 10px 0 rgba(0, 0, 0, 0.14), 0 1px 18px 0 rgba(0, 0, 0, 0.12);
    }
    button:hover,
    button:active {
      box-shadow: 0 7px 8px -4px rgba(0, 0, 0, 0.2),
        0 12px 17px 2px rgba(0, 0, 0, 0.14), 0 5px 22px 4px rgba(0, 0, 0, 0.12);
    }
    button:before {
      content: '';
    }
  }

  /* Phone size or smaller */
  @media screen and (max-width: 47em) {
    button {
      position: fixed;
      width: 2.5rem;
      height: 2.5rem;
      font-size: 1.25em;
      top: unset;
      bottom: calc(15% - 2.5rem);
    }
  }
</style>

<button on:click={rollNewNames}>
  <mat-ripple color="#ccd1dd33" />
</button>
