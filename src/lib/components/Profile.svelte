<script>
    import { supabase } from '$lib/supabaseClient';
    import { user } from '$lib/stores/sessionStore';

    let loading = true;
    let username = null;

    async function getProfile() {
        try {
            loading = true;
            const user = supabase.auth.user();

            let { data, error, status } = await supabase
                .from('profiles')
                .select('username')
                .eq('id', user.id)
                .single();

            if (error && status !== 406) throw error;

            if (data) {
                username = data.username;
            }
        } catch (error) {
            alert(error.message);
        } finally {
            loading = false;
        }
    }

    async function updateProfile() {
        try {
            loading = true;

            const user = supabase.auth.user();

            const updates = {
                id: user.id,
                username,
                website: null,
                avatar_url: null,
                updated_at: new Date(),
            }

            let { error } = await supabase.from('profiles').upsert(updates, {
                returning: 'minimal',
            });

            if (error) throw error;
        } catch (error) {
            alert(error.message);
            alert('test')
        } finally {
            loading = false;
        }
    }

    async function signOut() {
        try {
            loading = true;
            let { error } = await supabase.auth.signOut();
            user.set(false);
            // if (error) throw error;
        } catch (error) {
            alert(error.message);
        } finally {
            loading = false;
        }
    }
</script>

<form use:getProfile class="form-widget" on:submit|preventDefault={updateProfile}>
    <div>
      <label for="email">Email</label>
      <input id="email" type="text" value={$user.email} disabled />
    </div>
    <div>
      <label for="username">Name</label>
      <input
        id="username"
        type="text"
        bind:value={username}
      />
    </div>
  
    <div>
      <input type="submit" class="button block primary" value={loading ? 'Loading ...' : 'Update'} disabled={loading}/>
    </div>
  
    <div>
      <button class="button block" on:click={signOut} disabled={loading}>
        Sign Out
      </button>
    </div>
  </form>