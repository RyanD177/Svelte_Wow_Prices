<script>
  import { onMount } from 'svelte';
  import { items, getItems } from '../getToken.svelte';
  import Navigation from '../../components/Navigation.svelte';

  onMount(async () => {
    getItems()
    .then(() => {
      items.subscribe((value) => {
        console.log(value);
      })
    })
  })
</script>
<Navigation />