<template>

  <div class="NuxtPage">
    <NuxtPage />
  </div>

</template>
  <script setup lang="ts">
useHead({
  link : [{
                href:"https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css",
                rel:"stylesheet",
                integrity:"sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC",
                crossorigin:"anonymous" 
            }],
            script :[
                {
                    src:"https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js",
                    crossorigin:"anonymous"
                }
            ]
            
})
</script>