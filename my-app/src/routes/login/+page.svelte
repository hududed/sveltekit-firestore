<script lang="ts">
    import { auth, user } from "$lib/firebase";
    import { GoogleAuthProvider, signInWithPopup, signOut, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "firebase/auth";
    import { writable } from "svelte/store";

    const email = writable("");
    const password = writable("");

    async function signInWithGoogle() {
        const provider = new GoogleAuthProvider();
        try {
            const user = await signInWithPopup(auth, provider);
            console.log(user);
        } catch (error) {
            console.error("Error signing in with Google", error);
        }
    }

    async function signUpWithEmail() {
        try {
            const user = await createUserWithEmailAndPassword(auth, $email, $password);
            console.log(user);
        } catch (error) {
            console.error("Error signing up with email and password", error);
        }
    }

    async function signInWithEmail() {
        try {
            const user = await signInWithEmailAndPassword(auth, $email, $password);
            console.log(user);
        } catch (error) {
            console.error("Error signing in with email and password", error);
        }
    }
</script>
<link rel="stylesheet" href="https://demos.creative-tim.com/notus-js/assets/styles/tailwind.css">
<link rel="stylesheet" href="https://demos.creative-tim.com/notus-js/assets/vendor/@fortawesome/fontawesome-free/css/all.min.css">

  <div class="w-full lg:w-8/12 px-4 mx-auto pt-6">
    <div class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded-lg bg-blueGray-200 border-0">
      <div class="rounded-t mb-0 px-6 py-6">
        <div class="text-center mb-3">
          <h6 class="text-blueGray-500 text-sm font-bold">
            Sign in with
          </h6>
        </div>
        <div class="btn-wrapper text-center">
          <button class="bg-white active:bg-blueGray-50 text-blueGray-700 font-normal px-4 py-2 rounded outline-none focus:outline-none mr-1 mb-1 uppercase shadow hover:shadow-md inline-flex items-center font-bold text-xs ease-linear transition-all duration-150" type="button" on:click={signInWithGoogle}>
            <img alt="..." class="w-5 mr-1" src="https://demos.creative-tim.com/notus-js/assets/img/google.svg">Google
          </button>
        </div>
        <hr class="mt-6 border-b-1 border-blueGray-300">
      </div>
      <div class="flex-auto px-4 lg:px-10 py-10 pt-0">
        <div class="text-blueGray-400 text-center mb-3 font-bold">
          <small>Or sign in with credentials</small>
        </div>
        <form>
          <div class="relative w-full mb-3">
            <label class="block uppercase text-blueGray-600 text-xs font-bold mb-2" for="grid-email">Email</label>
            <input type="email" id="grid-email" class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150" placeholder="Email" bind:value={$email}>
          </div>
          <div class="relative w-full mb-3">
            <label class="block uppercase text-blueGray-600 text-xs font-bold mb-2" for="grid-password">Password</label>
            <input type="password" id="grid-password" class="border-0 px-3 py-3 placeholder-blueGray-300 text-blueGray-600 bg-white rounded text-sm shadow focus:outline-none focus:ring w-full ease-linear transition-all duration-150" placeholder="Password" bind:value={$password}>
          </div>
          <div>
            <label class="inline-flex items-center cursor-pointer">
              <input id="customCheckLogin" type="checkbox" class="form-checkbox border-0 rounded text-blueGray-700 ml-1 w-5 h-5 ease-linear transition-all duration-150">
              <span class="ml-2 text-sm font-semibold text-blueGray-600">Remember me</span>
            </label>
          </div>
          <div class="text-center mt-6">
            <button class="bg-blueGray-800 text-white active:bg-blueGray-600 text-sm font-bold uppercase px-6 py-3 rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 w-full ease-linear transition-all duration-150" type="button" on:click={signInWithEmail}>Sign In</button>
            <button class="bg-blueGray-800 text-white active:bg-blueGray-600 text-sm font-bold uppercase px-6 py-3 rounded shadow hover:shadow-lg outline-none focus:outline-none mr-1 mb-1 w-full ease-linear transition-all duration-150" type="button" on:click={signUpWithEmail}>Sign Up</button>
          </div>
        </form>
      </div>
    </div>
  </div>

  <h2>Login</h2>

  {#if $user}
    <h2 class="card-title">Welcome, {$user.displayName}</h2>
    <p class="text-center text-success">You are logged in</p>
    <button class="btn btn-warning" on:click={() => signOut(auth)}>Sign out</button>
  {:else}
    <button class="btn btn-primary" on:click={signInWithGoogle}>Sign in with Google</button>
  {/if}