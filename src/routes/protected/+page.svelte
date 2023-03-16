<script lang="ts">
	import { goto } from '$app/navigation';
    import {onMount, afterUpdate, beforeUpdate} from 'svelte'
    import axios from "axios";

	async function logout() {
		await fetch('/api/v1/user/logout', {
			method: 'POST',
			body: JSON.stringify({})
		});

		goto('/');
	}

    onMount(async () => {

        const cookies = document.cookie.split(';');
        const accessTokenCookie = cookies.find(cookie => cookie.trim().startsWith('accessToken='));
        const accessToken = accessTokenCookie ? 'Bearer '+accessTokenCookie.split('=')[1] : null;

        console.log(accessToken);

        const test = await axios.get('http://localhost:8090/user/1', {
            headers: {
                'Authorization': accessToken,
            },
            withCredentials: true
        });

        console.log(test.data.data.username);

    });
</script>

<p>User's Discord ID: asdasdsa</p>

<button on:click={logout}>Logout</button>
