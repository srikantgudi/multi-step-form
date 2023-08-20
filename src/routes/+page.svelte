<script>
  let plans = [
    {
      name: 'Arcade',
      price: 9,
      img: 'icon-arcade.svg'
    },
    {
      name: 'Advanced',
      price: 12,
      img: 'icon-advanced.svg'
    },
    {
      name: 'Pro',
      price: 15,
      img: 'icon-pro.svg'
    },
  ];

  let addons = [
    {
      name: 'Online Service',
      text: 'Access to multi-player games',
      price: 1
    },
    {
      name: 'Larger Storage',
      text: 'Extra 1TB of cloud save',
      price: 2
    },
    {
      name: 'Customizable Profile',
      text: 'Custom theme on your profile',
      price: 2
    },
  ]

  let steps = [
    {
      id: 1,
      name: 'YOUR INFO'
    },
    {
      id: 2,
      name: 'SELECT PLAN'
    },
    {
      id: 3,
      name: 'ADD ONS'
    },
    {
      id: 4,
      name: 'SUMMARY'
    },
  ]
  let cursteps = [1];
  let curstep = 1;

  let data = {
    name: 'James',
    email: 'james@lorem.net',
    phone: '+1879879879',
    plan: plans[0],
    addons: [],
    monthly: true
  }

  const goBack = () => {
    if (curstep > 1) {
      curstep--;
    }
    if (!cursteps.includes(curstep)) {
      cursteps = [...cursteps, curstep]
    }
  }

  const goNext = () => {
    if (curstep < 4) {
      curstep++;
    }
    if (!cursteps.includes(curstep)) {
      cursteps = [...cursteps, curstep]
    }
  }

  $: totalPrice = data.plan.price + data.addons.reduce((t,a) => t += a.price, 0);

</script>

<style>
  .grid {
    display: grid;
    grid-template-columns: 20rem 2fr;
    height: inherit;
  } 
  .content {
    box-sizing: border-box;
    padding: 1rem;
    box-shadow: 0 0 4px #999;
    border-radius: 1rem;
  }
  .border-bottom {
    border-bottom: 1px solid #ccc;
  }
  .border-bottom:hover {
    border-bottom: 1px solid #999;
  }
  @media screen and (max-width: 480px) {
    .grid {
      grid-template-columns: 1fr;
    }
    .content {
      font-size: 150%;
      padding: 1vh 8vw;
      margin: 1vh 8vw;
      height: auto;
    }
  }
</style>

<main class="desktop">
  <div class="app-title">FrontendMentor: Multi-Step App </div>
  <div class="grid">
    <div class="desktop-sidebar">
      {#each steps as step, stepNo}
        <div class="step-card">
          <div class="step-id" class:active={cursteps.includes(step.id)}>{step.id}</div>
          <div class="step-card-text">
            <div class="step-title">STEP {step.id}</div>
            <div class="step-name">{step.name}</div>
          </div>
        </div>
      {/each}
    </div>
    <div class="mobile-sidebar">
      <div class="flex justify-center">
        {#each steps as step, stepNo}
          <div class="step-id" class:active={cursteps.includes(step.id)}>{step.id}</div>
        {/each}
      </div>
    </div>
    <div class="content">
      {#if (curstep === 1)}
      <div class="step-info card">
        <h2 class="text-3xl">Personal Info</h2>
        <div class="my-2">
            <div>Name: </div>
            <input type="text" bind:value={data.name} placeholder="name.." class="border-2 p-1 rounded-md" />
        </div>
        <div class="mb-2">
            <div>Email:</div>
            <div><input type="email" bind:value={data.email} placeholder="email.." class="border-2 p-1 rounded-md" /></div>
        </div>
        <div class="mb-2">
          <div>Phone:</div>
          <div><input type="text" bind:value={data.phone} placeholder="phone.." class="border-2 p-1 rounded-md" /></div>
        </div>
      </div>
      {:else if (curstep === 2)}
        <div class="step-info">
          <h2 class="text-3xl">Select your plan</h2>
          <div>
            {#each plans as plan}
            <button on:click={() => {data.plan=plan}} class="border-{data.plan.name===plan.name ? '4':'2'} hover:border-slate-400 border-slate-{data.plan===plan ? '600' : '200'} bg-black-{data.plan===plan ? '500': '100'} bg-gray-50 hover:bg-gray-100 hover:border-{data.plan===plan ? '4': '2'} rounded-md flex gap-4 my-4 p-4 w-full">
              <img src={`/images/${plan.img}`} alt="img">
              <div class="flex flex-col wrap gap-2">
                <div class="text-xl">{plan.name}</div>
                <div>${plan.price}/mo</div>
              </div>
            </button>
            {/each}
          </div>
          <div class="text-center flex gap-2 align-center">
            <div class="text-{data.monthly ? 'xl' : '2'} font-{data.monthly ? 'bold':'thin'}">Monthly</div>
            <input type="checkbox" bind:checked={data.monthly} />
            <div class="text-{data.monthly ? '2' : 'xl'} font-{data.monthly ? 'thin' : 'bold'}">Yearly</div>
            <div class="text-sm text-gray-400">(un-select for yearly)</div>
          </div>
        </div>
      {:else if (curstep === 3)}
        <div class="step-info">
          <h2 class="text-3xl">Pick add-ons</h2>
          {#each addons as addon}
          <div>
            <button class="w-full">
              <label class="flex align-center gap-4 bg-gray-{data.addons.includes(addon) ? '200' : '50'}
                border-2 border-slate-200 rounded-md my-2 p-4">
                  <input type="checkbox" class="w-[1.5rem]" value={addon} bind:group={data.addons} />
                  <div class="text-left w-[15rem]">
                    <div class="text-xl">{addon.name}</div>
                    <div class="text-2">{addon.text}</div>
                  </div>
                  <div>${addon.price}/mo</div>
              </label>
            </button>
          </div>
          {/each}
        </div>
      {:else}
        <div class="step-info">
          <h2 class="text-3xl">Finishing Up</h2>
          <h2 class="text-l text-gray-500">Double check everything looks OK before confirming</h2>
          <div class="step-info">
            <div class="my-2 flex justify-between font-bold">{data.plan.name}/({data.monthly?'Monthly':'Yearly'})</div>
            <div class="flex justify-between"><button class="border-bottom" on:click={() => {curstep=2}}>Change</button> <span>${data.plan.price}</span></div>
            <div class="my-2">
              {#each data.addons as ad}
                <div class="flex justify-between">
                  <div>{ad.name}</div>
                  <div>${ad.price}</div>
                </div>
              {/each}
            </div>
            <div class="my-4 flex justify-between">
              <span>Monthly:</span> <span>${totalPrice}/mo</span>
            </div>
            {#if !data.monthly}
            <div class="my-4 flex justify-between">
              <span>Total:</span>
              <span>${totalPrice * (data.monthly ? 1 : 12)}/{data.monthly ? 'mo' : 'yr'}</span>
            </div>
            {/if}
          </div>
        </div>
        {/if}
        <div class="flex justify-between gap-2">
          {#if curstep > 1}
          <button class="border-2 bg-gray-100 p-1 px-4 rounded-md" on:click={() => goBack()}>Prev</button>
          {/if}
          {#if curstep < 4}
          <button class="border-2 bg-blue-100 p-1 px-4 rounded-md" on:click={() => goNext()}>Next</button>
          {/if}
          {#if curstep === 4}
          <button class="border-2 bg-blue-300 p-1 px-4 rounded-md">Confirm</button>
          {/if}
        </div>
    </div>
  </div>
</main>
