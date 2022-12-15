<script lang='ts'>
  import RadioButton from '$components/RadioButton.svelte'
  import Alert from '$components/Alert.svelte'

  // @ts-ignore: next-line
  import html2pdf from 'html2pdf.js'

  const textInputCSS = `
    form-control
    block
    w-full
    mb-5
    px-3
    py-1.5
    text-base
    font-normal
    text-gray-700
    bg-white bg-clip-padding
    border border-solid border-gray-300
    rounded
    transition
    ease-in-out
    m-0
    focus:text-gray-700 focus:bg-white focus:border-blue-500 focus:outline-none
  `

  let organisation: string
  let dataservice: string
  let showQuestions = false
  
  let hasIAMConnected: boolean
  let hasKVK: boolean
  let authorisedToSign: boolean
  let licensedData: boolean
  let hasTOS: boolean
  let hasFunctionalDocs: boolean
  let relevantForLogistics: boolean
  
  let isAPI: boolean
  let isBroker: boolean
  let canValidateTokens: boolean
  let hasTechnicalDocs: boolean
  let implementsStandard: boolean

  const ja_nee_options = [{
    value: true,
    label: 'Ja'
  }, {
    value: false,
    label: 'Nee'
  }]
  
  let timeoutId: ReturnType<typeof setTimeout>
  const debounce = (fn: Function, ms = 800) => {
    return function (this: any, ...args: any[]) {
      clearTimeout(timeoutId)
      timeoutId = setTimeout(() => fn.apply(this, args), ms)
    }
  }

  const downloadPDF = () => {
    const opt ={
      margin: 1,
      filename: 'DEFLog_checklist.pdf',
      enableLinks: true,
      pagebreak: { mode: 'avoid-all' }
    }
    const element = document.getElementById('main')
    html2pdf().set(opt).from(element).save()
  }

  let progressCSS = 'width: 0;'
  $: if (organisation && progressCSS !== 'width: 100%;')                                              progressCSS = 'width: 7%;'
  $: if (dataservice && progressCSS !== 'width: 100%;')                                               progressCSS = 'width: 14%;'
  $: if ((hasIAMConnected || hasIAMConnected === false) && progressCSS !== 'width: 100%;')            progressCSS = 'width: 21%;'
  $: if ((hasKVK || hasKVK === false) && progressCSS !== 'width: 100%;')                              progressCSS = 'width: 28%;'
  $: if ((authorisedToSign || authorisedToSign === false) && progressCSS !== 'width: 100%;')          progressCSS = 'width: 35%;'
  $: if ((licensedData || licensedData === false) && progressCSS !== 'width: 100%;')                  progressCSS = 'width: 42%;'
  $: if ((hasTOS || hasTOS === false) && progressCSS !== 'width: 100%;')                              progressCSS = 'width: 49%;'
  $: if ((hasFunctionalDocs || hasFunctionalDocs === false) && progressCSS !== 'width: 100%;')        progressCSS = 'width: 56%;'
  $: if ((relevantForLogistics || relevantForLogistics === false) && progressCSS !== 'width: 100%;')  progressCSS = 'width: 63%;'
  $: if ((isAPI || isAPI === false) && progressCSS !== 'width: 100%;')                                progressCSS = 'width: 70%;'
  $: if ((isBroker || isBroker === false) && progressCSS !== 'width: 100%;')                          progressCSS = 'width: 77%;'
  $: if ((canValidateTokens || canValidateTokens === false) && progressCSS !== 'width: 100%;')        progressCSS = 'width: 84%;'
  $: if ((hasTechnicalDocs || hasTechnicalDocs === false) && progressCSS !== 'width: 100%;')          progressCSS = 'width: 91%;'
  $: if (implementsStandard || implementsStandard === false)                                          progressCSS = 'width: 100%;'

  let onboardingOK: boolean = true
  $: if ((hasKVK || hasIAMConnected) && (isAPI || isBroker) && canValidateTokens && relevantForLogistics) {
    onboardingOK = true
  } else {
    onboardingOK = false
  }

</script>

<main class="p-20 max-w-3xl" id="main">
  <img class="mb-5" src="/deflog_logo.svg" alt="DL" />
  <h1>DEFLog dataservice onboarding checklist</h1>

  <div class="relative bg-gray-400 w-full rounded-xl h-5 mb-5">
    <div style={progressCSS} class="absolute bg-green-300 rounded-xl h-5 z-1"></div>
  </div>

  <p class="mb-5">U heeft een dataservice en wilt deze aanbieden op DEFLog?<br>Doorloop de checklist en leer of uw dataservice geschikt is!<p>
  <p class="font-bold">Hoe heet uw organisatie?</p>
  <input 
    class={textInputCSS}
    bind:value={organisation}
  />

  <p class="font-bold">Hoe heet uw dataservice?</p>
  <input 
    on:keyup={debounce(() => { if (dataservice) showQuestions = true })}
    class={textInputCSS}
    bind:value={dataservice}
  />
  
  {#if showQuestions && organisation}
    <p class="font-bold mt-8 mb-5 text-lg underline underline-offset-2">Administratieve vragen</p>

    <div class="mb-5">
      <p><a class="text-blue-500 underline" href="https://iamconnected.eu" target="_blank" rel="noreferrer">IAMConnected</a> is de identiteitsprovider service van Portbase</p>
      <RadioButton
        options={ja_nee_options}
        question={`Staat ${organisation} geregistreerd bij IAMConnected?`}
        bind:userSelected={hasIAMConnected}
      />
    </div>

    {#if hasIAMConnected === false}
      <div class="mb-5">
        <RadioButton
          options={ja_nee_options}
          question={`Staat ${organisation} geregistreerd bij de KVK?`}
          bind:userSelected={hasKVK}
        />
      </div>
    {/if}

    {#if hasIAMConnected === false}
      <div class="mb-5">
        <RadioButton
          options={ja_nee_options}
          question={`Bent u tekenbevoegd namens ${organisation}?`}
          bind:userSelected={authorisedToSign}
        />
      </div>
    {/if}

    <div class="mb-5">
      <RadioButton
        options={ja_nee_options}
        question={`Wordt de data van ${dataservice} uitgegeven onder een licentie?`}
        bind:userSelected={licensedData}
      />
    </div>
    <div class="mb-5">
      <RadioButton
        options={ja_nee_options}
        question={`Zijn er algemene voorwaarden op ${dataservice} van toepassing?`}
        bind:userSelected={hasTOS}
      />
    </div>
    <div class="mb-5">
      <RadioButton
        options={ja_nee_options}
        question={`Is er functionele documentatie over ${dataservice} beschikbaar?`}
        bind:userSelected={hasFunctionalDocs}
      />
    </div>
    <div class="mb-5">
      <RadioButton
        options={ja_nee_options}
        question={`Is ${dataservice} relevant voor logistieke dienstverleners of logistieke IT-leveranciers?`}
        bind:userSelected={relevantForLogistics}
      />
    </div>

    {#if relevantForLogistics || relevantForLogistics === false}
      <p class="font-bold mt-8 text-lg underline underline-offset-2">Technische vragen</p>
      <p class="mb-5">Voor het beantwoorden van deze vragen heeft u mogelijk een developer of uw IT-leverancier nodig.</p>
    
      <div class="mb-5">
        <RadioButton
          options={ja_nee_options}
          question={`Kan de data van ${dataservice} geconsumeerd worden via een HTTP request naar een API endpoint?`}
          bind:userSelected={isAPI}
        />
      </div>
      {#if isAPI === false}
        <div class="mb-5">
          <RadioButton
            options={ja_nee_options}
            question={`Kan de data van ${dataservice} geconsumeerd worden middels een subscriptie op een message broker?`}
            bind:userSelected={isBroker}
          />
        </div>
      {/if}

      <p class="mb-2">
        De gebruikers (data subscribers) van DEFLog ontvangen een token wanneer zij zich 
        op een data service abonneren. Met elk verzoek om data wordt dit token meegezonden 
        in de headers van het request. De data provider wordt geacht om dit token bij 
        DEFLog te valideren.
      </p>
      <p>
        Voor meer informatie zie de 
        <a href="https://data.deflog.nl/documentation" target="_blank" rel="noreferrer" class="underline">
          DEFLog documentatie
        </a>.
      </p>
      <img class="my-5" src="/deflog_auth_flow.png" alt="DEFLog Authorisatie Flow" />
      
      <div class="mb-5">
        <RadioButton
          options={ja_nee_options}
          question={`Is het mogelijk om token validatie van de toekomstige DEFLog gebruikers van ${dataservice} in te bouwen in ${isAPI ? 'de API' : isBroker ? 'de message broker' : dataservice}?`}
          bind:userSelected={canValidateTokens}
        />
      </div>

      <div class="mb-5">
        <RadioButton
          options={ja_nee_options}
          question={`Is er technische documentatie van ${dataservice} beschikbaar?`}
          bind:userSelected={hasTechnicalDocs}
        />
      </div>
      
      <div class="mb-5">
        <RadioButton
          options={ja_nee_options}
          question={`Is de data van ${dataservice} volgens een standaard geformatteerd?`}
          bind:userSelected={implementsStandard}
        />
      </div>

    {/if}
  {/if}

  {#if implementsStandard || implementsStandard === false}
    <p class="font-bold mt-8 mb-5 text-lg underline underline-offset-2">Conclusie</p>
    {#if onboardingOK}
      <Alert type='success'>{dataservice} kan op DEFLog gepubliceerd worden!</Alert>
      {#if (!hasIAMConnected || !hasFunctionalDocs || !hasTechnicalDocs ||!implementsStandard || !hasTOS ||!licensedData)}
        <p>Er zijn wel een aantal aandachtspunten.</p>
        {#if hasIAMConnected === false}
          <div class="mx-auto pl-4">
            <ul class="list-disc mb-5">
              <li>
                {organisation} moet geregistreerd worden op IAMConnected. Dit kan omdat
                {organisation} een KVK-registratie heeft. De registratie moet worden
                uitgevoerd door een tekenbevoegd persoon.
              </li>
            </ul>
          </div>
          <p class="my-5">Volg de <a class="underline" href="https://www.deflog.org/images/Documenten/20221112_Procesbeschrijving_Aanmelden_DEFLog_v2.pdf">DEFLog IAMconnected instructies</a> om {organisation} te registreren!</p>
        {/if}
        {#if !hasFunctionalDocs || !hasTechnicalDocs || !implementsStandard || !hasTOS || !licensedData}
          <p>Onderstaande punten zijn geen dealbreakers, maar worden wel aanbevolen:</p>
          <div class="mx-auto pl-4">
            <ul class="list-disc mb-5">
              {#if hasFunctionalDocs === false}
                <li>
                  Er is geen functionele documentatie.
                </li>
              {/if}
              {#if hasTechnicalDocs === false}
                <li>
                  Er is geen technische documentatie.
                </li>
              {/if}
              {#if implementsStandard === false}
                <li>De data van {dataservice} is niet volgens een standaard geformatteerd.</li>
              {/if}
              {#if hasTOS === false}
                <li>Er zijn geen algemene voorwaarden van toepassing op {dataservice}.</li>
              {/if}
              {#if licensedData === false}
                <li>Er is geen licentie van toepassing op {dataservice}.</li>
              {/if}
            </ul>
          </div>
        {/if}
      {/if}
      <p class="my-5">Ga naar <a class="underline" href="https://data.deflog.nl" target="_blank" rel="noreferrer">DEFLog</a> om {dataservice} te onboarden!</p>
    {:else}
      <Alert type='danger'>{dataservice} kan <b>NIET</b> op DEFLog gepubliceerd worden!</Alert>
      <p>Om {dataservice} te kunnen onboarden op DEFLog zijn de volgende zaken vereist:</p>
      <div class="mx-auto pl-4">
        <ul class="list-disc mb-5">
          {#if !hasIAMConnected}
            <li>{organisation} moet geregistreerd worden op IAMConnected, hiervoor is een KVK-registratie verplicht.</li>
          {/if}
          {#if !isAPI && !isBroker}
            <li>De data van {dataservice} moet middels een API of een message broker beschikbaar worden gesteld voor consumptie.</li>
          {/if}
          {#if !canValidateTokens}
            <li>{dataservice} moet de tokens van DEFLog gebruikers kunnen valideren.</li>
          {/if}
          {#if !relevantForLogistics}
            <li>DEFLog is bedoeld voor de logistieke sector, de aangeboden data moet daarom logistiek relevant zijn.</li>
          {/if}
        </ul>
      </div>
    {/if}
    <p>Neem contact op met <a class="underline" href="mailto:info@deflog.org">customer support</a> voor aanvullende informatie.</p>
    <button class="mt-5 rounded-lg bg-gray-100 border-gray-300 font-normal border p-2 px-4" on:click={downloadPDF}>
      Checklist als PDF downloaden
    </button>
  {/if}
</main>
