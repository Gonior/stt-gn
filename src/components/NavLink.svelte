
<script lang="ts">
    import { Link } from "svelte-routing";
  
    export let to:string;
    let active:boolean = false;
    function getProps({ href, isPartiallyCurrent, isCurrent }):object {
      const isActive = href === "/" ? isCurrent : isPartiallyCurrent || isCurrent;
      // The object returned here is spread on the anchor element's attributes
      if (isActive) return { class : "active"}
      else return {}
    }

</script>


<Link to="{to}" getProps="{getProps}">
  <slot />
</Link>