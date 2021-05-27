<template>
    <module 
        title="Authorization" 
        @close="close()">

        <div slot="body">
            <form @submit.prevent="onSubmit">
                <!-- email -->
                <div class="form-item" :class="{ errorInput: $v.email.$error }">
                    <label>Email:</label>
                    <p class="errorText" v-if="!$v.email.required">Fieled is required!</p>
                    <p class="errorText" v-if="!$v.email.email">Email is not correct!</p>
                    <input 
                        v-model="email"
                        :class="{ error: $v.email.$error }"
                        @change="$v.email.$touch()">
                </div>
                <!-- password -->
                <div class="form-item" :class="{ errorInput: $v.password.$error }">
                    <label>Password:</label>
                    <p class="errorText" v-if="!$v.password.required">Fieled is required!</p>
                    <p class="errorText" v-if="!$v.password.minLength">Password must have at least {{ $v.password.$params.minLength.min }}!</p>
                    <input type="password"
                        v-model="password"
                        :class="{ error: $v.password.$error }"
                        @change="$v.password.$touch()">
                </div>
                <!-- button -->
                <button class="btn btnPrimary" >Submit</button>
                
            </form> 
            <div class="registration">
                <button class="btn btnPrimary" @click="openRegistration()">Registration</button>
            </div>
            
        </div>
    </module>
</template>


<script>
import { required, minLength, email } from 'vuelidate/lib/validators'
import module from '@/components/Module.vue'

export default {
    components: {  
        module
    },
    data () {
        return {
            email:'',
            password: ''
        }
    },
    validations: {
        email: {
            required,
            email
        },
        password: {
            required,
            minLength: minLength(8)
        },        
    },
     methods: {
        onSubmit () {
            this.$v.$touch()
            if (!this.$v.$invalid) {
                const user = {
                    email: this.email
                }
                console.log(user)
                this.resetForm ()
                this.$emit('close')
            }          
        },
        close () {
            this.$emit('close') 
            this.resetForm ()
        },
        resetForm () {
            this.email = ''
            this.password = ''
            this.$v.$reset()
        },
        openRegistration () {
            this.resetForm ()
            this.$emit('openRegistration')
        }
    }
}
</script>

<style lang="scss" >

.form-item .errorText {
    display: none;
    margin-bottom: 8px;
    font-size: 13.4px;
    color: #dd1e1e;
}

.form-item {
    &.errorInput .errorText {
    display: block;
    }
}

input.error {
    border-color: #dd1e1e;
}

.btn.btnPrimary.disable {
    background-color: #abaceb;
} 

.registration {
    margin-top: 20px;
}

</style>