<script>
  import MatRipple from 'mat-ripple'
  import Icon from 'fa-svelte'
  import { faWindowClose } from '@fortawesome/free-solid-svg-icons/faWindowClose'
  import { faIdCard } from '@fortawesome/free-solid-svg-icons/faIdCard'
  import { faUser } from '@fortawesome/free-solid-svg-icons/faUser'
  import { faUsers } from '@fortawesome/free-solid-svg-icons/faUsers'
  import { faKey } from '@fortawesome/free-solid-svg-icons/faKey'
  import { faUserLock } from '@fortawesome/free-solid-svg-icons/faUserLock'
  import { faUserShield } from '@fortawesome/free-solid-svg-icons/faUserShield'
  import { faFireAlt } from '@fortawesome/free-solid-svg-icons/faFireAlt'
  import { faAngleDoubleUp } from '@fortawesome/free-solid-svg-icons/faAngleDoubleUp'
  import { faGraduationCap } from '@fortawesome/free-solid-svg-icons/faGraduationCap'
  import { faBirthdayCake } from '@fortawesome/free-solid-svg-icons/faBirthdayCake'
  import { faCalendarAlt } from '@fortawesome/free-solid-svg-icons/faCalendarAlt'
  import { faGlobe } from '@fortawesome/free-solid-svg-icons/faGlobe'
  import { faNotesMedical } from '@fortawesome/free-solid-svg-icons/faNotesMedical'
  import { faRedo } from '@fortawesome/free-solid-svg-icons/faRedo'
  import { faArrowLeft } from '@fortawesome/free-solid-svg-icons/faArrowLeft'
  import { faCloudUploadAlt } from '@fortawesome/free-solid-svg-icons/faCloudUploadAlt'
  import { faShieldAlt } from '@fortawesome/free-solid-svg-icons/faShieldAlt'
  import { faExclamationTriangle } from '@fortawesome/free-solid-svg-icons/faExclamationTriangle'
  import { faBomb } from '@fortawesome/free-solid-svg-icons/faBomb'
  import { faBiohazard } from '@fortawesome/free-solid-svg-icons/faBiohazard'
  import { faSkullCrossbones } from '@fortawesome/free-solid-svg-icons/faSkullCrossbones'

  import ExportButton from './ExportButton.svelte'
  import config from '../../config.js'
  export let character_name
  export let faction
  let card_id = ''
  let factions = config.Factions
  let ICC_number
  let threat_assessment = 0
  let bastion_clearance = 0
  let douane_dispositions = [
    'ICC VETTED',
    'ACCESS GRANTED',
    'ACCESS PENDING',
    'DETAIN',
    'DECEASED',
  ]
  let douane_disposition = 'ACCESS GRANTED'
  let rank
  let age = Math.floor(Math.random() * (40 - 18 + 1) + 18)
  let ic_birthday
  let homeplanet
  let homeplanets = ['Eos']
  let bloodtypes = ['O', 'A', 'B', 'AB']
  let bloodtype
  let recurring = false
  let showDialog
  export const show = () => showDialog.showModal()

  $: if (ICC_number || faction) {
    let firstNumber
    switch (faction) {
      case 'aquila':
      default:
        firstNumber = '7'
        break
      case 'dugo':
        firstNumber = '3'
        break
      case 'ekanesh':
        firstNumber = '8'
        break
      case 'pendzal':
        firstNumber = '9'
        break
      case 'sona':
        firstNumber = '5'
        break
    }
    ICC_number =
      firstNumber +
      'ddd ddddd dddd'.replace(/d/g, (d) => Math.floor(Math.random() * 10))
  }

  $: if (age) {
    let day = 0
    let month = Math.floor(Math.random() * 12) + 1
    if (month == 2) {
      day = Math.floor(Math.random() * 28) + 1
    } else if (month == 1 || 3 || 5 || 7 || 8 || 10 || 12) {
      day = Math.floor(Math.random() * 31) + 1
    } else {
      day = Math.floor(Math.random() * 30) + 1
    }
    let year = 240 - age
    ic_birthday =
      day.toString() + '-' + month.toString() + '-' + year.toString() + 'NT'
  }
  $: onNameChange(character_name)
  function onNameChange() {
    let bloodChance
    switch (faction) {
      case 'aquila':
      default:
        bloodChance = [45, 40, 11, 4]
        homeplanets = [
          'Accipiter',
          'Alcyon',
          'Alietum',
          'Ferox II',
          'Merula',
          'Noctua',
          'Sturnus',
          'Viridis',
          'Fastus',
          'Ignis',
          'Ithaginis',
          'Tigris',
        ]
        break
      case 'dugo':
        bloodChance = [30, 38, 22, 10]
        homeplanets = [
          'Kaito',
          'Batongbayal',
          'Cabatu',
          'Hideyoshi',
          'Hiroto',
          'Katsuro',
          'Minoru',
          'Shinobu',
          'Tarou',
          'Haruka',
          'Noburu',
        ]
        break
      case 'ekanesh':
        bloodChance = [51, 34, 12, 3]
        homeplanets = ['Dzar']
        break
      case 'pendzal':
        bloodChance = [33, 36, 23, 8]
        homeplanets = [
          'Dodamu',
          'Zvir',
          'Ziamlia',
          'Zorki',
          'Vady',
          'Cionma',
          'Vtotoroy',
          'Nadz',
          'Ruda',
          'Zyccio',
        ]
        break
      case 'sona':
        bloodChance = [34, 31, 29, 6]
        homeplanets = ['Andhera', 'Ghara', 'Prakhasa']
        break
    }
    let sum = bloodChance.reduce((acc, el) => acc + el, 0)
    let acc = 0
    bloodChance = bloodChance.map((el) => (acc = el + acc))
    let rand = Math.random() * sum
    bloodtype = bloodtypes[bloodChance.filter((el) => el <= rand).length]
    homeplanet = homeplanets[Math.floor(Math.random() * homeplanets.length)]
  }

  function makeid(length) {
    let result = ''
    let characters = '0123456789'
    let charactersLength = characters.length
    for (let i = 0; i < length; i++) {
      if (i == 3 || i == 9) {
        result += ' '
      } else {
        result += characters.charAt(
          Math.floor(Math.random() * charactersLength),
        )
      }
    }
    return result
  }
  function closeDialog() {
    showDialog.close()
  }
  function showExportSuccess(event) {
    if (event.detail.succeeded == false) {
      alert(event.detail.message)
    } else if (event.detail.succeeded == true) {
      closeDialog()
      setTimeout(function () {
        alert(event.detail.message)
      }, 180)
    }
  }
</script>

<style>
  dialog {
    border: none;
    inline-size: clamp(45.5em, 50vw, 48em);
    background-color: #2c3445;
    font-weight: normal;
    padding: 0;
    color: #ccd1dd;
    transition: all 0.4s cubic-bezier(0.25, 0.8, 0.25, 1);
    box-shadow: 0 0.6875em 0.9375em -0.4375em rgba(0, 0, 0, 0.2),
      0 1.5em 2.375em 0.1875em rgba(0, 0, 0, 0.14),
      0 0.5625em 2.875em 0.5em rgba(0, 0, 0, 0.12);
    contain: paint;
  }
  /* Tablet size or smaller */
  @media screen and (max-width: 76em) {
    dialog {
      inline-size: clamp(42em, 60vw, 45.5em);
    }
  }
  /* Phone size or smaller */
  @media screen and (max-width: 47em) {
    dialog {
      inline-size: 90vw;
    }
  }
  /* tiny screens / horizontal mode */
  @media screen and (max-height: 27em) {
    dialog {
      zoom: 0.75;
    }
  }
  dialog::backdrop {
    background-color: rgba(29, 32, 40, 0.6);
  }
  .backdropExitEvent {
    position: fixed;
    top: 0px;
    left: 0px;
    block-size: 100vh;
    inline-size: 100vw;
    z-index: -1;
  }
  label.styledCheckbox {
    position: relative;
    display: inline-block;
    pointer-events: none;
    top: -1.75em;
    left: -1.75em;
  }
  label.styledCheckbox::before,
  label.styledCheckbox::after {
    position: absolute;
    content: '';
    display: inline-block;
  }
  label.styledCheckbox::before {
    block-size: 1.5em;
    inline-size: 1.5em;

    border: 0.0625em solid #386ae8;
    border-radius: 0.3125em;
  }
  label.styledCheckbox::after {
    height: 1rem;
    width: 1.5rem;
    color: #386ae8;
    font-size: 2em;
    top: -0.45em;
    left: 0.1em;
    text-shadow: 0 0.0625em 0.1875em rgba(0, 0, 0, 0.12),
      0 0.0625em 0.125em rgba(0, 0, 0, 0.24);
  }
  input:not([type='range']) {
    margin-block-start: 0.5em;
  }
  input[type='checkbox'] {
    inline-size: auto;
    opacity: 0;
    margin-block-start: 0.25em;
    margin-block-end: 0em;
    zoom: 2;
  }
  input[type='checkbox'] + label::after {
    content: none;
  }
  input[type='checkbox']:checked + label::after {
    content: '✓';
  }
  input[type='checkbox']:hover + label::after,
  input[type='checkbox']:active + label::after {
    color: #ccd1dd;
  }
  input[type='checkbox']:hover + label::before,
  input[type='checkbox']:active + label::before {
    border: 0.0625em solid #507ef2;
    background: #507ef2;
    box-shadow: 0 0.0625em 0.1875em rgba(0, 0, 0, 0.12),
      0 0.0625em 0.125em rgba(0, 0, 0, 0.24);
  }
  .cancel {
    color: #ccd1dd;
    border: 0.0625em solid #ccd1dd;
    background: none;
  }
  .cancel:hover,
  .cancel:active {
    background: #424959;
    border: 0.0625em solid #ccd1dd;
  }

  input[disabled],
  input[disabled]:hover,
  input[disabled]:active {
    color: #838795;
    border-color: #838795;
  }
  div.form,
  .buttonWrapper {
    display: grid;
    grid-template-columns: 50% 50%;
  }
  .buttonWrapper {
    margin-block-start: -1em;
  }
  .Grid_inline-start {
    grid-column: 1;
    margin: 5%;
  }
  .Grid_inline-end {
    grid-column: 2;
    margin: 5%;
  }
  select {
    cursor: pointer;
    padding: 0.26em;
    width: auto;
    background: none;
    border: 0;
    border-block-end: 0.125em solid #838795;
    color: #ccd1dd;
  }
  select:hover,
  select:active {
    border: 0;
    color: #ccd1dd;
    border-block-end: 0.125em solid #386ae8;
  }
  option {
    color: #ccd1dd;
    background: #3b414e;
  }
  input[type='range'] {
    height: 0;
    margin-inline-start: 0.25em;
    block-size: 2em;
    background: #2c3445;
    border: 0;
    -webkit-appearance: none;
    color: transparent;
  }
  input[type='range']::-webkit-slider-thumb {
    block-size: 2em;
    inline-size: 1px;
    -webkit-appearance: none;
    background: transparent;
  }
  input[type='range']:focus {
    outline: 0;
  }
  .factionSelect select,
  select option {
    text-transform: capitalize;
  }
  progress {
    -webkit-appearance: none;
    appearance: none;
    position: absolute;
    pointer-events: none;
    block-size: 2em;
  }
  progress::-webkit-progress-bar {
    background-color: transparent;
  }
  input[type='range'].threat {
    inline-size: 8em;
    background-image: radial-gradient(
        circle at 0.25em,
        transparent 0.15em,
        #838795 0.25em,
        transparent 0
      ),
      radial-gradient(
        circle at 1.5em,
        #838795 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 3em,
        #838795 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 4.5em,
        #838795 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 6em,
        #838795 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 7.5em,
        #838795 0.48em,
        transparent 0.51em,
        #2c3445 0
      );
  }

  progress.threat {
    margin-inline-start: -7em;
    inline-size: 7em;
  }

  progress.threat[value]::-webkit-progress-value {
    background-image: radial-gradient(
        circle at 0.5em,
        #f2507e 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 2em,
        #f2507e 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 3.5em,
        #f2507e 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 5em,
        #f2507e 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 6.5em,
        #f2507e 0.48em,
        transparent 0.51em,
        #2c3445 0
      );
  }
  input[type='range'].clearance {
    inline-size: 5em;
    background-image: radial-gradient(
        circle at 0.25em,
        transparent 0.15em,
        #838795 0.25em,
        transparent 0
      ),
      radial-gradient(
        circle at 1.5em,
        #838795 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 3em,
        #838795 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 4.5em,
        #838795 0.48em,
        transparent 0.51em,
        #2c3445 0
      );
  }

  progress.clearance {
    margin-inline-start: -4em;
    inline-size: 4em;
  }

  progress.clearance[value]::-webkit-progress-value {
    background-image: radial-gradient(
        circle at 0.5em,
        #f2c450 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 2em,
        #f2c450 0.48em,
        transparent 0.51em,
        transparent 0
      ),
      radial-gradient(
        circle at 3.5em,
        #f2c450 0.48em,
        transparent 0.51em,
        #2c3445 0
      );
  }
  .CloseX,
  .CloseX:hover,
  .CloseX:active {
    position: absolute;
    top: 0px;
    right: 0px;
    block-size: 2em;
    inline-size: 2em;
    background: rgba(0, 0, 0, 0);
    color: #838795;
    border: none;
    padding: 0;
    margin: 0;
    box-shadow: unset;
  }
  :global(.faIcon) {
    font-size: 0.7em;
    transform: scale(1.6);
    margin: 0 0.25em;
  }
</style>

<dialog bind:this={showDialog}>
  <div class="backdropExitEvent" on:click={closeDialog} />
  <button class="CloseX" on:click={closeDialog}>
    <Icon class="faIcon" icon={faWindowClose} />
    <mat-ripple
      color="#28292c55"
      centered="true"
      unbounded="true"
      radius="15" />
  </button>
  <div class="form">
    <div class="Grid_inline-start">
      <label>
        <Icon class="faIcon" icon={faIdCard} />
        Card ID:
        <br />
        <input
          type="text"
          bind:value={card_id}
          placeholder="Scan your ID card"
          autocomplete="one-time-code"
          required />
      </label>
      <label>
        <Icon class="faIcon" icon={faUser} />
        Character Name:
        <br />
        <input type="text" bind:value={character_name} required />
      </label>
      <label class="factionSelect">
        <Icon class="faIcon" icon={faUsers} />
        Faction:
        <br />
        <select bind:value={faction}>
          {#each factions as faction}
            <option value={faction}>{faction}</option>
          {/each}
        </select>
      </label>
      <label>
        <Icon class="faIcon" icon={faKey} />
        ICC Number:
        <br />
        <input
          type="tel"
          bind:value={ICC_number}
          pattern="[0-9]{4} [0-9]{5} [0-9]{4}"
          disabled />
      </label>
      <label>
        {#if threat_assessment == 0}
          <Icon class="faIcon" icon={faShieldAlt} />
        {:else if threat_assessment == 1}
          <Icon class="faIcon" icon={faExclamationTriangle} />
        {:else if threat_assessment == 2}
          <Icon class="faIcon" icon={faFireAlt} />
        {:else if threat_assessment == 3}
          <Icon class="faIcon" icon={faBomb} />
        {:else if threat_assessment == 4}
          <Icon class="faIcon" icon={faBiohazard} />
        {:else if threat_assessment == 5}
          <Icon class="faIcon" icon={faSkullCrossbones} />
        {/if}
        Threat Assesment:
        {#if threat_assessment == 0}
          none
        {:else if threat_assessment == 1}
          minimum
        {:else if threat_assessment == 2}
          minor
        {:else if threat_assessment == 3}
          moderate
        {:else if threat_assessment == 4}
          maximum
        {:else if threat_assessment == 5}extreme{/if}
        <br />
        <input
          class="threat"
          type="range"
          min="0"
          max="5"
          bind:value={threat_assessment} />
        <progress class="threat" value={threat_assessment} max="5" />
      </label>

      <label>
        <Icon class="faIcon" icon={faUserLock} />
        Bastion Clearance: {bastion_clearance}
        <br />
        <input
          class="clearance"
          type="range"
          name="bastion_clearance"
          min="0"
          max="3"
          bind:value={bastion_clearance} />
        <progress class="clearance" value={bastion_clearance} max="3" />
      </label>
      <label>
        <Icon class="faIcon" icon={faUserShield} />
        Douane Disposition:
        <br />
        <select bind:value={douane_disposition}>
          {#each douane_dispositions as douane_disposition}
            <option value={douane_disposition}>{douane_disposition}</option>
          {/each}
        </select>
      </label>
    </div>
    <div class="Grid_inline-end">
      <label>
        <Icon class="faIcon" icon={faAngleDoubleUp} />
        Rank /
        <Icon class="faIcon" icon={faGraduationCap} />
        Job:
        <input
          type="text"
          placeholder="Your military rank or job title"
          bind:value={rank} />
      </label>
      <label>
        <Icon class="faIcon" icon={faBirthdayCake} />
        Age:
        <input type="number" bind:value={age} />
      </label>
      <label>
        <Icon class="faIcon" icon={faCalendarAlt} />
        Birthdate:
        <input type="text" bind:value={ic_birthday} />
      </label>
      <label>
        <Icon class="faIcon" icon={faGlobe} />
        Current / home planet:
        <br />
        <select bind:value={homeplanet}>
          {#each homeplanets as homeplanet}
            <option value={homeplanet}>{homeplanet}</option>
          {/each}
        </select>
      </label>
      <label>
        <Icon class="faIcon" icon={faNotesMedical} />
        Bloodtype:
        <br />
        <select bind:value={bloodtype}>
          {#each bloodtypes as bloodtype}
            <option value={bloodtype}>{bloodtype}</option>
          {/each}
        </select>
      </label>
      <label>
        <Icon class="faIcon" icon={faRedo} />
        Recurring?
        <br />
        <input type="checkbox" bind:checked={recurring} />

        <label class="styledCheckbox" />

      </label>
      <br />
      <div class="buttonWrapper">
        <button class="cancel" on:click={closeDialog}>
          <Icon class="faIcon" icon={faArrowLeft} />
          Back
          <mat-ripple color="#ccd1dd33" />
        </button>
        <ExportButton
          on:exportFinished={showExportSuccess}
          {card_id}
          {character_name}
          {faction}
          {ICC_number}
          {threat_assessment}
          {bastion_clearance}
          {douane_disposition}
          {rank}
          {ic_birthday}
          {homeplanet}
          {bloodtype}
          {recurring} />
      </div>
    </div>
  </div>
</dialog>
