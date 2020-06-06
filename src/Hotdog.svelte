<style>
  .addons {
    margin-top: 1em;
  }
  select,
  input {
    min-width: 0;
  }
</style>

<script>
  export let order = {};
  export let amount = 0;
</script>

<h4>Order your hot dog!</h4>

<label for="kind">Choose kind</label>
<select name="kind" class="kind" bind:value={order.kind}>
  <option value="" />
  <option value="bratwurst">Bratwürst</option>
  <option value="kielbasa">Kielbasa</option>
  <option value="frakfurter">Frakfurter</option>
  <option value="merguez">Merguez</option>
  <option value="chorizo">Chorizo</option>
</select>

<label for="amount">How many?</label>
<input name="amount" type="number" bind:value={amount} />

<div class="addons">
  <label>
    <input type="checkbox" bind:checked={order.sauerkraut} />
    Sauerkraut
  </label>

  <label>
    <input type="checkbox" bind:checked={order.mustard} />
    Mustard
  </label>

  <label>
    <input type="checkbox" bind:checked={order.ketchup} />
    Ketchup
  </label>
</div>
