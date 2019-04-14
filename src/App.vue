<template>
    <div class="wrapper">
       <div class="wrapper-content">
         <section>
           <div class="container">
             <!-- first modal -->
             <button class="btn btn-primary" type="button" name="button" @click="modalFirst = !modalFirst">Show First modal</button>
              <modals title="First modal"
              v-show="modalFirst"
              @close="modalFirst = false">
              <div slot="body">
                <p>
                  Hey there im here
                </p>
                <button class="btn btn-primary" type="button" name="button" @click="modalFirst = !modalFirst">Well done</button>
              </div>
            </modals>
            <!-- second modal -->
            <button class="btn btn-primary" type="button" name="button" @click="modalSecond.show = !modalSecond.show">Show modal with form</button>
             <modals title="Second modal"
             v-show="modalSecond.show"
             @close="modalSecond.show = false">
             <div slot="body">
               <form @submit.prevent="submitSecondForm">
                 <label>Name:</label>
                 <input required type="text" v-model="modalSecond.name">
                 <label>Email:</label>
                 <input required type="email" v-model="modalSecond.email">
                 <button class="btn btn-primary">Submit</button>
               </form>
             </div>
           </modals>
           <button class="btn btn-primary" @click="modalValidate = !modalValidate">Show modal with form + validate</button>

           <!-- modals with validate -->
           <modalValidate v-show ="modalValidate" @close="modalValidate = false"/>

           <!-- registration form -->
           <button class="btn btn-primary" @click="registration = !registration">Registration</button>
           <button class="btn btn-primary" @click="enter = !enter">Enter</button>

           <registration v-show ="registration" @close="registration = false" @changeToEnter="registration = false, enter = true"/>
           <enter v-show ="enter" @close="enter = false" @changeToRegistration="enter = false, registration = true"/>

           </div>
         </section>
       </div>
    </div>
</template>

<script>
import modals from '@/components/UI/Modal.vue'
import modalValidate from '@/components/ModalValidate.vue'
import registration from '@/components/Registration.vue'
import enter from '@/components/Enter.vue'


export default {
  components: {
    modals,
    modalValidate,
    registration,
    enter
  },
  data() {
    return{
      modalFirst: false,
      modalSecond: {
        show: false,
        name: '',
        email: ''
      },
      modalValidate: false,
      registration: false,
      enter: false
    }
  },
  methods: {
    submitSecondForm () {
      console.log({
        name: this.modalSecond.name,
        email: this.modalSecond.email
      });
      this.modalSecond.name = '',
      this.modalSecond.email = '',
      this.modalSecond.show = false
    }
  }
}
</script>

<style land="scss" scoped>

</style>
