<script>
    import Auth from "$lib/components/Auth.svelte";
    import Profile from "$lib/components/Profile.svelte";
    import { user } from '$lib/stores/sessionStore';
    import { supabase } from '$lib/supabaseClient';

</script>

<div class="container" style="padding: 50px 0 100px 0;">
    {#if $user}
        <Profile />
    {:else}
        <Auth />
    {/if}
</div>