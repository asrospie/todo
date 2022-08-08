<script>
    import {user} from "$lib/stores/sessionStore"
    import {supabase} from "$lib/supabaseClient"

    user.set(supabase.auth.user());

    supabase.auth.onAuthStateChange((state, session) => {
        if (state == 'SIGNED_IN') {
            user.set(session.user);
        } else {
            user.set(false);
        }
    });
</script>

<!DOCTYPE html>
<svelte:head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css">
</svelte:head>
<h1>Navbar Location</h1>
<button class='button is-primary'>Test</button>
<div>
    <slot></slot>
</div>