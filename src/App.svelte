<script>
  import MapView from './lib/MapView.svelte';

  let basemap = 'street';
  let mapView;
  let layerVisible = false;
  let status = 'Basemap only. Toggle layer on to view supplier points.';

  function onToggleLayer(event) {
    layerVisible = event.target.checked;
    if (layerVisible) {
      mapView?.useMvt();
      status = 'Layer points visible. Pan the map to explore tiles.';
    } else {
      mapView?.hideMvt();
      status = 'Layer points hidden. Toggle on to view.';
    }
  }

  const options = [
    { id: 'street', label: 'MAPID Street 2D' },
    { id: 'light', label: 'MAPID Light' },
    { id: 'satellite', label: 'MAPID Satellite' },
    { id: 'demo', label: 'MapLibre Demo (fallback)' }
  ];
</script>

<div class="app">
  <aside>
    <p class="kicker">MAPID × BINUS</p>
    <h1>Supplier WebGIS</h1>
    <label for="basemap">Basemap</label>
    <select id="basemap" bind:value={basemap}>
      {#each options as option}
        <option value={option.id}>{option.label}</option>
      {/each}
    </select>

    <div class="toggle-card">
      <div class="toggle-info">
        <span class="toggle-title">Supplier Points</span>
        <span class="toggle-status" class:active={layerVisible}>
          {layerVisible ? 'Layer active' : 'Layer off'}
        </span>
      </div>
      <label class="switch" for="layer-toggle">
        <input
          id="layer-toggle"
          type="checkbox"
          checked={layerVisible}
          on:change={onToggleLayer}
          aria-label="Toggle supplier points layer"
        />
        <span class="slider"></span>
      </label>
    </div>

    <p class="hint">{status}</p>
    <p class="note">Click a green point to view supplier details.</p>
  </aside>
  <MapView bind:this={mapView} {basemap} />
</div>

<style>
  .app {
    display: flex;
    height: 100%;
    min-height: 100vh;
  }

  aside {
    width: 280px;
    flex-shrink: 0;
    padding: 20px 18px;
    background: #f4f1ea;
    border-right: 1px solid #ddd6c8;
    box-sizing: border-box;
  }

  .kicker {
    margin: 0 0 8px;
    font-size: 12px;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    color: #6b6458;
  }

  h1 {
    margin: 0 0 20px;
    font-size: 22px;
    line-height: 1.2;
    font-weight: 650;
  }

  label {
    display: block;
    margin-bottom: 6px;
    font-size: 13px;
    font-weight: 600;
  }

  select {
    width: 100%;
    padding: 8px 10px;
    font: inherit;
    background: #fff;
    border: 1px solid #c9c1b2;
    border-radius: 6px;
    cursor: pointer;
  }

  .toggle-card {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-top: 14px;
    padding: 10px 12px;
    background: #ffffff;
    border: 1px solid #ddd6c8;
    border-radius: 8px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04);
  }

  .toggle-info {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .toggle-title {
    font-size: 13px;
    font-weight: 600;
    color: #202124;
  }

  .toggle-status {
    font-size: 11px;
    font-weight: 500;
    color: #80868b;
    transition: color 0.2s ease;
  }

  .toggle-status.active {
    color: #0f6f4a;
  }

  /* Material-style Toggle Switch */
  .switch {
    position: relative;
    display: inline-block;
    width: 38px;
    height: 22px;
    flex-shrink: 0;
    cursor: pointer;
    margin: 0;
  }

  .switch input {
    opacity: 0;
    width: 0;
    height: 0;
    margin: 0;
  }

  .slider {
    position: absolute;
    inset: 0;
    background-color: #dadce0;
    border-radius: 22px;
    transition: background-color 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .slider:before {
    position: absolute;
    content: "";
    height: 16px;
    width: 16px;
    left: 3px;
    bottom: 3px;
    background-color: #ffffff;
    border-radius: 50%;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.28);
    transition: transform 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .switch input:checked + .slider {
    background-color: #0f6f4a;
  }

  .switch input:focus-visible + .slider {
    outline: 2px solid #0f6f4a;
    outline-offset: 2px;
  }

  .switch input:checked + .slider:before {
    transform: translateX(16px);
  }

  .hint,
  .note {
    margin: 16px 0 0;
    font-size: 14px;
    line-height: 1.45;
    color: #3f3a33;
  }

  .note {
    margin-top: 8px;
    color: #6b6458;
  }
</style>
