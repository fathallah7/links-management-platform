<script setup>
import { RouterLink } from 'vue-router';
import { useRouter } from 'vue-router';
import { ref, onMounted } from 'vue';
import axios from 'axios';

const router = useRouter();
const googleData = ref({})

onMounted(() => {
    // Google Identity Services
    window.google.accounts.id.initialize({
        client_id: '731351321833-a0rbcof0j7gh352jevfpok78iq1fvrl3.apps.googleusercontent.com',
        callback: handleCredentialResponse
    })

    window.google.accounts.id.renderButton(
        document.getElementById('g_id_signin'),
        { theme: 'outline', size: 'large' }
    )
})
async function handleCredentialResponse(response) {

    const token = response.credential

    try {
        const googleData = await axios.post('/login/google', {
            token
        })
        // googleData.user
        // googleData.token 
        console.log('User:', googleData.user)
        console.log('API Token:', googleData.token)

        localStorage.setItem('token', googleData.token)
        router.push('/home');
    } catch (error) {
        console.error(error.response?.googleData || error.message)
    }
}


</script>


<template>

    <!-- Google Sign-In Button -->
    <div id="g_id_signin">
    </div>

</template>