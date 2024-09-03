<script>
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
  
    let authenticated = false;
    onMount(() => {
        const token = localStorage.getItem('token');
        // Determina si el usuario está autenticado
        authenticated = !!token;

        // Redirige según la ruta actual y el estado de autenticación
        const pathname = window.location.pathname;
        if (!authenticated && pathname !== '/login') {
        goto('/login'); // Redirige a login si no está autenticado y no está en la página de login
        } else if (authenticated && pathname === '/login') {
        goto('/home'); // Redirige a home si ya está autenticado y está en la página de login
        }
    });
  
    // Función para manejar logout y redirección
    function handleLogout() {
      localStorage.removeItem('token');
      goto('/login');
    }
  </script>
  
  <main>
    <slot />
  </main>
  