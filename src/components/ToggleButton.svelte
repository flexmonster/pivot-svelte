<script lang="ts">
    export let id: string;
    export let labelOn: string;
    export let labelOff: string;
    export let checked: boolean = true;
    export let onChange: (checked: boolean) => void;

    function onClick() {
        checked = !checked;
        onChange(!checked);
    }
</script>

<div class="toggle-button noselect">
    <input
        class="button-checkbox"
        type="checkbox"
        id={id}
        checked={checked}
        on:click={onClick}
    />
    <label for={id} class="button-checkbox-label">
        <span class="on">{checked ? labelOn : labelOff}</span>
    </label>
</div>
