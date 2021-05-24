<script>
  import { Router } from "@roxi/routify";
  import { routes } from "../.routify/routes";
</script>

<style  global>
  @import "../assets/global.css";

  :global(:root) {
    --linen: #F7ECE1;
    --purple: #725AC1;
    --light-purple: #8D86C9;
    --dark-purple: #242038;
    --gray: #707070;
    --gold: #c9b112;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  :global(body) {
    padding: 0;
  }
</style>

<Router {routes} />