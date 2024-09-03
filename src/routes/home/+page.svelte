<script>
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';

  async function handleLogout() {
    try {
      const token = localStorage.getItem('token');

      // Hacer la solicitud al endpoint de logout con GET
      const response = await fetch('http://localhost:8000/api/logout', {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${token}`
        }
      });

      if (!response.ok) {
        throw new Error('Logout failed');
      }

      // Eliminar el token del almacenamiento local
      localStorage.removeItem('token');

      // Verificar si el token existe en el almacenamiento local
      if (!localStorage.getItem('token')) {
        // Redirigir a la página de inicio de sesión si el token no está presente
        goto('/login');
      }
      // Redirigir a la página de login
      //goto('/login');
    } catch (err) {
      console.error(err.message);
    }
  }

  onMount(() => {
    const token = localStorage.getItem('token');
    if (!token) {
      goto('/login'); // Redirige a la página de inicio de sesión si no hay token
    }
  });
</script>

<h1>Welcome to Home</h1>
<button on:click={handleLogout}>Logout</button>
