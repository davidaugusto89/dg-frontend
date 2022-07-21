<template>
  <div>
    <header class="bg-gray-800">
      <nav>
        <div class="max-w-7xl mx-auto px-2 sm:px-6 lg:px-8">
          <div class="relative flex items-center justify-between h-16">
            <div class="flex-1 flex items-center items-stretch justify-start">
              <div class="flex-shrink-0 flex items-center">
                <img class="block h-8 w-auto" src="https://tailwindui.com/img/logos/workflow-mark-indigo-500.svg" alt="Workflow">
              </div>

              <div class="hidden sm:block sm:ml-6">
                <div class="flex space-x-4">
                  <NuxtLink to="/" :class="{
                    'bg-gray-900 text-white px-3 py-2 rounded-md text-sm font-medium': $route.name === 'index',
                    'text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium': $route.name !== 'index'
                  }">Home</NuxtLink>
                  <NuxtLink to="/cadastros" :class="{
                    'bg-gray-900 text-white px-3 py-2 rounded-md text-sm font-medium': $route.name === 'cadastros',
                    'text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium': $route.name !== 'cadastros'
                  }">Cadastros</NuxtLink>
                </div>
              </div>
            </div>

            <div class="absolute inset-y-0 right-0 flex items-center pr-2 sm:static sm:inset-auto sm:ml-6 sm:pr-0">
              
              <div class="ml-3 relative">
                <div>
                  <a href="https://www.linkedin.com/in/david-augusto-keller-haddad-305a91161/" target="_blank" class="bg-gray-800 flex text-sm rounded-full focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-800 focus:ring-white">
                    <img class="h-8 w-8 rounded-full" src="https://avatars.githubusercontent.com/u/57589195?v=4" alt="">
                  </a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </nav>
    </header>

    <div class="max-w-7xl mx-auto p-4">
      <div class="mt-4">
        <Nuxt />
      </div>
  </div>
  </div>
</template>