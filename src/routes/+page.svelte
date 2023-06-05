<script lang="ts">
  let linkBudgetSelect: string
  let insertionLossSelect: string
  let splitterLossSelect: string
  let attenuationSelect: string
  let addionalLossInput: string = "1"
  let lossPerConnectorInput: string = "0.05"
  let connectorCountInput: string = "1"
  let splicesPerKm: number = 1
  let spliceLoss: number = 0.05
  $: splitterLoss = Number(insertionLossSelect) + Number(splitterLossSelect)
  $: connectorLoss = Number(connectorCountInput) * Number(lossPerConnectorInput)
  $: totalSpliceLoss = splicesPerKm * spliceLoss
  $: budget = (Number(linkBudgetSelect) - splitterLoss - connectorLoss - Number(addionalLossInput) - 1).toFixed(2)
  $: reach = (Number(budget) / (Number(attenuationSelect) + totalSpliceLoss)).toFixed(2)
</script>
<div class="w-full grid grid-cols-2">
  <div>
    <div class="card mx-10 my-5 p-5">
      <h2>PON Technology Used</h2>
      <label class="label pt-3">
        <span>XGS-PON Optic</span>
        <select bind:value={linkBudgetSelect} class="select">
          <option value="32.00">Tibit N2</option>
        </select>
      </label>
      <label class="label pt-3">
        <span>Fiber Type</span>
        <select bind:value={attenuationSelect} class="select">
          <option value="0.47">G.652D</option>
        </select>
      </label>
    </div>
    <div class="card mx-10 my-5 p-5">
      <h2>Added Losses</h2>
      <label class="label pt-3">
        <span>Number of Connectors</span>
        <input bind:value={connectorCountInput} min="0" class="input" type="number"/>
      </label>
      <label class="label pt-3">
        <span>Loss per Connector (dB)</span>
        <input bind:value={lossPerConnectorInput} min="0" step="0.1" class="input" type="number"/>
      </label>
      <label class="label pt-3">
        <span>Additional Margin(dB)</span>
        <input bind:value={addionalLossInput} min="0" class="input" type="number"/>
      </label>
      <label class="label pt-3">
        <span>Splices per km</span>
        <input bind:value={splicesPerKm} min="0" class="input" type="number"/>
      </label>
      <label class="label pt-3">
        <span>Splice loss(dB)</span>
        <input bind:value={spliceLoss} min="0" step="0.01" class="input" type="number"/>
      </label>
    </div>
  </div>
  <div>
    <div class="card mx-10 my-5 p-5">
      <h2>Splitters</h2>
      <label class="label pt-3">
        <span>Total Split Ratio</span>
        <select bind:value={insertionLossSelect} class="select">
          <option value="3.36">1x2</option>
          <option value="6.72">1x4</option>
          <option value="10.08">1x8</option>
          <option value="13.44">1x16</option>
          <option value="16.8">1x32</option>
          <option value="20.16">1x64</option>
          <option value="23.52">1x128</option>
        </select>
      </label>
      <label class="label pt-3">
        <span>Number of Splitters</span>
        <select bind:value={splitterLossSelect} class="select">
          <option value="0.73">1</option>
          <option value="1.46">2</option>
          <option value="2.19">3</option>
        </select>
      </label>
    </div>
    <div class="card variant-ghost-primary mx-10 my-5 p-5">
      <h2>Calculated Outputs</h2>
      <div class="pt-3 w-full grid grid-cols-[auto_1fr] gap-2 [&>*:nth-child(2n)]:justify-self-end">
        <span>Link Budget (dB):</span>
        <span>{linkBudgetSelect}</span>
        <span>Insertion Loss (dB):</span>
        <span>{insertionLossSelect}</span>
        <span>Additional Splitter Loss (dB):</span>
        <span>{splitterLossSelect}</span>
        <span>Total Splice Loss per km (dB/km):</span>
        <span>{totalSpliceLoss}</span>
        <span>Budget Available For Fiber (dB):</span>
        <span>{budget}</span>
        <span class="font-bold">Calculated Reach (km):</span>
        <span class="font-bold">{reach}</span>
      </div>
    </div>
  </div>
</div>
