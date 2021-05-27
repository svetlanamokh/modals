<template>
    <module 
        title="Registration" 
        @close="close()">

        <div slot="body">
            <form @submit.prevent="onSubmit">
                <!-- name -->
                <div class="form-item" :class="{ errorInput: $v.name.$error }">
                    <label>Name:</label>
                    <p class="errorText" v-if="!$v.name.required">Fieled is required!</p>
                    <p class="errorText" v-if="!$v.name.minLength">Name must have at least {{ $v.name.$params.minLength.min }}!</p>
                    <input 
                        v-model="name"
                        :class="{ error: $v.name.$error }"
                        @change="$v.name.$touch()">
                </div>
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
                <!-- repeat password -->
                <div class="form-item" :class="{ errorInput: $v.repeatPassword.$error || (!isPasswordsTheSame && repeatPassword != '') }">
                    <label>Repeat password:</label>
                    <p class="errorText" v-if="!$v.repeatPassword.required">Fieled is required!</p> 
                    <p class="errorText" v-if="repeatPassword != password">Password must be the same!</p>
                    <input type="password"
                        v-model="repeatPassword"
                        :class="{ error: $v.repeatPassword.$error }"
                        @change="$v.repeatPassword.$touch()"
                        >                    
                </div>
                <!-- button -->
                <button class="btn btnPrimary" :class="{ disable: isDisabled }" >Submit</button>
            </form> 
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
            name:'',
            email:'',
            password: '',
            repeatPassword: ''
        }
    },
    computed: {
        isPasswordsTheSame () {
           return this.repeatPassword === this.password
        },
        isDisabled () {
            return this.$v.$invalid || !(this.repeatPassword === this.password && this.password != '')
        }
    },
    validations: {
        name: {
            required,
            minLength: minLength(4)
        },
        email: {
            required,
            email
        },
        password: {
            required,
            minLength: minLength(8)
        },
        repeatPassword: {
            required
        }
    },
    methods: {
        onSubmit () {
            this.$v.$touch()
            if (!this.$v.$invalid && this.repeatPassword === this.password) {
                const user = {
                    name: this.name,
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
            this.name = ''
            this.email = ''
            this.password = ''
            this.repeatPassword = ''
            this.$v.$reset()
        },
    }
}
</script>