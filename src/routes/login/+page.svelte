<script>
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';

  let email = '';
  let password = '';

  async function handleLogin(event) {
    event.preventDefault();

    try {
      const response = await fetch('http://localhost:8000/api/login', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ email, password }),
      });

      if (!response.ok) {
        throw new Error('Login failed');
      }

      const data = await response.json();
      console.log('Response JSON:', data); // Verifica el contenido de la respuesta

      if (data.data && data.data.token) {
        localStorage.setItem('token', data.data.token);
        console.log('Token stored successfully:', data.data.token);
        goto('/home');
      } else {
        console.error('Token not found in response:', data);
      }
    } catch (err) {
      console.error('An error occurred:', err.message);
    }
  }

  onMount(() => {
    const token = localStorage.getItem('token');
    if (token) {
      goto('/home'); // Redirige a la página de inicio si el usuario ya está autenticado
    }
  });
</script>

<form on:submit={handleLogin}>
  <label>
    Email:
    <input type="email" bind:value={email} required />
  </label>
  <label>
    Password:
    <input type="password" bind:value={password} required />
  </label>
  <button type="submit">Login</button>
</form>
