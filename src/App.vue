<template>
  <div class="wrapper">
    <div class="wrapper-content">

      <section>
        <div class="container">
          
          <!-- authorization -->
          <button class="btn btnPrimary" @click="moduleAuthorization = !moduleAuthorization">Authorization</button>
          <authorization v-show="moduleAuthorization" @close="moduleAuthorization = false" @openRegistration="moduleAuthorization = false; 
          moduleRegistration = true"/>

          <!-- registration form -->
          <button class="btn btnPrimary" @click="moduleRegistration = !moduleRegistration">Registration</button>
          <registration v-show="moduleRegistration" @close="moduleRegistration = false"/>
        </div>
      </section>

    </div>
  </div>
</template>

<script>
import authorization from '@/components/Authorization.vue'
import registration from '@/components/Registration.vue'


export default {
  components: {
    authorization,
    registration
  },
  data () {
    return {
      moduleAuthorization: false,
      moduleRegistration: false    
    }
  }
}
</script>

<style>
</style>
