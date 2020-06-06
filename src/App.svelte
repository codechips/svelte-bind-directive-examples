<style>
  :global(body) {
    font-family: Arial, Helvetica, sans-serif;
    background-color: #f3f3f3;
  }
  :global(*, *:before, *:after) {
    box-sizing: border-box;
  }
  :global(input[type='text'], input[type='select'], input[type='password'], input[type='number'], select) {
    padding: 5px;
    min-width: 30em;
  }
  .box {
    width: 100px;
    height: 100px;
    background-color: #ff4;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .fields {
    margin: 2em 0;
  }
  .fields label {
    display: block;
    margin-top: 1em;
  }
  .forms {
    margin: 2em 5em 2em 10em;
  }
  .container {
    display: flex;
  }
  .results {
    position: fixed;
    right: 0;
    width: 50%;
    height: 100%;
    background-color: #def7f7;
    padding: 10% 3em;
  }
  .results pre {
    font-size: 1.2em;
  }
</style>

<script>
  import { onMount } from 'svelte';
  import slugify from 'slugify';
  import Hotdog from './Hotdog.svelte';

  let conferencing = [
    { id: '1', label: 'Zoom' },
    { id: '2', label: 'Hangouts' },
    { id: '3', label: 'Skype' }
  ];

  // const frameworks = ['React', 'Svelte', 'Vue'];
  const frameworks = [
    { id: 1, name: 'React.js' },
    { id: 2, name: 'Svelte.js' },
    { id: 3, name: 'Vue.js' },
    { id: 4, name: 'Marko.js' }
  ];

  let username = 'jane';

  let values = {
    username: 'jane',
    startYear: 1999,
    company: '',
    slug: '',
    conferencing: 'hangouts',
    dynamic: conferencing[2],
    framework: 'Svelte',
    reactiveFrameworks: 'Vue',
    agree: true,
    tools: []
  };

  const reset = () => Object.keys(values).forEach(key => (values[key] = ''));
  const submit = () => alert(JSON.stringify(values, null, 2));

  let box = null;

  onMount(() => {
    var colors = ['#F99', '#9F9', '#99F', '#FF9', '#F9F'];

    setInterval(() => {
      let random = Math.floor(Math.random() * colors.length);
      let color = colors[random];
      box.style.background = color;
      box.innerHTML = color;
    }, 1000);
  });

  $: values.slug = slugify(values.company, { lower: true });
</script>

<div class="container">
  <div class="forms">

    <h2>Component bind</h2>
    <div class="box" bind:this={box}>color</div>
    <div>
      <h2>Simple two-way bind</h2>
      <form>
        <div>
          <input type="text" name="username" bind:value={username} />
        </div>
        <div>
          <input type="text" name="repeat" bind:value={username} />
        </div>
        <p>
          username
          <code>{username}</code>
        </p>
        <button on:click|preventDefault={() => (username = '')}>reset</button>
      </form>
    </div>

    <div>
      <h2>Complex Form</h2>
      <form on:submit|preventDefault={submit}>
        <div class="fields">
          <label for="username">Username</label>
          <input type="text" name="username" bind:value={values.username} />

          <label for="password">Password</label>
          <input type="password" name="password" bind:value={values.password} />

          <label for="company">Company name</label>
          <input type="text" name="company" bind:value={values.company} />

          <label for="slug">Slug</label>
          <input type="text" name="slug" bind:value={values.slug} />

          <label for="startYear">Start year</label>
          <input type="number" name="startYear" bind:value={values.startYear} />
        </div>

        <label>
          <input type="checkbox" bind:checked={values.agree} />
          I agree to sell my personal information to evil corporations
        </label>

        <div>
          <h3>Simple select bind with static values</h3>
          <select bind:value={values.conferencing}>
            <option value="" />
            <option value="zoom">Zoom.us</option>
            <option value="hangouts">Google Hangouts</option>
            <option value="skype">Microsoft Skype</option>
          </select>
        </div>

        <div>
          <h3>Dynamic select bind with objects</h3>
          <select bind:value={values.dynamic}>
            {#each conferencing as conference}
              <option value={conference}>{conference.label}</option>
            {/each}
          </select>
        </div>

        <div>
          <h3>Dynamic multiple select bind with objects</h3>
          <select multiple bind:value={values.tools}>
            {#each conferencing as conference}
              <option value={conference}>{conference.label}</option>
            {/each}
          </select>
        </div>

        <div>
          <h3>Static radio button bind</h3>
          <label>
            <input type="radio" bind:group={values.framework} value="React" />
            React
          </label>
          <label>
            <input type="radio" bind:group={values.framework} value="Svelte" />
            Svelte
          </label>
          <label>
            <input type="radio" bind:group={values.framework} value="Vue" />
            Vue
          </label>
        </div>

        <div>
          <h3>Dynamic radio button bind</h3>
          {#each frameworks as framework}
            <label>
              <input type="radio" bind:group={values.reactiveFrameworks} value={framework} />
              {framework.name}
            </label>
          {/each}
        </div>

        <div>
          <h3>Component bind</h3>
          <Hotdog bind:order={values.hotdog} bind:amount={values.numberOfHotdogs} />
        </div>

        <div style="margin-top:3em">
          <button on:click|preventDefault={reset}>reset</button>
          <button type="submit">submit</button>
        </div>
      </form>
    </div>
  </div>

  <div class="results">
    <h3>Complex Form Results</h3>
    <pre>{JSON.stringify(values, null, 2)}</pre>
  </div>
</div>
