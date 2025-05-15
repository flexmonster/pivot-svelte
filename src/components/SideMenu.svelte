<script lang="ts">
  import { getStores } from '$app/stores';

  const { page } = getStores();
  let path: string = '';
  $: page.subscribe(($page) => {
    path = $page.url.pathname;
  });

</script>

<div class="side-menu">
  <ol>
    <li class="tab-button">
      <a
        href="/pivot-table-demo"
        class:router-link-exact-active={path.startsWith("/pivot-table-demo")}
      >
        PIVOT TABLE DEMO
      </a>
    </li>
  </ol>

  <div class="sub-title">API and Events</div>
  <ol>
    <li class="tab-button">
      <a
        href="/handling-events"
        class:router-link-exact-active={path.startsWith("/handling-events")}
      >
        HANDLING EVENTS
      </a>
    </li>
    <li class="tab-button">
      <a
        href="/using-api-calls"
        class:router-link-exact-active={path.startsWith("/using-api-calls")}
      >
        USING API CALLS
      </a>
    </li>
    <li class="tab-button">
      <a
        href="/updating-data"
        class:router-link-exact-active={path.startsWith("/updating-data")}
      >
        UPDATING DATA
      </a>
    </li>
  </ol>

  <div class="sub-title">Customization</div>
  <ol>
    <li class="tab-button">
      <a
        href="/customizing-toolbar"
        class:router-link-exact-active={path.startsWith("/customizing-toolbar")}
      >
        CUSTOMIZING THE TOOLBAR
      </a>
    </li>
    <li class="tab-button">
      <a
        href="/customizing-grid"
        class:router-link-exact-active={path.startsWith("/customizing-grid")}
      >
        CUSTOMIZING THE GRID
      </a>
    </li>
  </ol>

  <div class="sub-title">Integration</div>
  <ol>
    <li class="tab-button">
      <a
        href="/with-highcharts"
        class:router-link-exact-active={path.startsWith("/with-highcharts")}
      >
        WITH HIGHCHARTS
      </a>
    </li>
    <li class="tab-button">
      <a
        href="/with-amcharts"
        class:router-link-exact-active={path.startsWith("/with-amcharts")}
      >
        WITH AMCHARTS
      </a>
    </li>
  </ol>
</div>
