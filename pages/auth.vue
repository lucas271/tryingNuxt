<template>
    <v-main class="h-screen w-screen d-flex align-center justify-center pa-0 ma-0 bg-teal-darken-2">
            <v-btn icon="mdi-arrow-left" density="comfortable" position="absolute" :style="{top: '1%', left: '0.5%', zIndex: '100'}"></v-btn>
            <v-window class="mx-auto h-100 w-100 responsive-width my-auto" v-model="step">
                <v-window-item class="h-100 w-100" :value="1">
                    <v-form class="h-100 w-100" @submit="handleSignIn">
                        <v-card class="h-100 w-100 d-flex justify-center bg-teal-darken-1 align-center">
                            <div class="w-50 h-100 pa-4 flex-column d-none d-sm-flex justify-space-evenly align-center">
                                <v-card-title class="text-uppercase font-weight-bold">Entre na sua conta</v-card-title>
                                <div class="w-100">
                                    <v-text-field placeholder="email" v-model="email"></v-text-field>
                                    <v-text-field placeholder="Senha" v-model="password"></v-text-field>
                                </div>

                                
                                <v-card-actions>
                                    <v-btn variant="tonal" type="submit" class="font-weight-bold">Entrar</v-btn>
                                </v-card-actions>
                            </div>


                            <div class="w-50 h-100  login-form-bg-image flex-grow-1">
                                <v-sheet position="absolute" class="bg-overlay">

                                </v-sheet>

                                <div class="remove-overlay d-flex justify-space-between align-center flex-column pa-4 h-100 w-100 text-white">
                                    <v-card-subtitle tag="h1" class="text-h4 text-uppercase font-weight-bold bg-teal-darken-4 pa-3 rounded-lg text-wrap text-center" :style="{lineHeight: '1.5'}">Life Cris - Login</v-card-subtitle>
                                    <div class="w-100 h-100 pa-4 flex-column d-flex d-sm-none justify-space-evenly align-center">
                                        <div class="w-100">
                                            <v-text-field placeholder="email"></v-text-field>
                                            <v-text-field placeholder="Senha" ></v-text-field>
                                        </div>
                                    </div>
                                    <v-card-actions class="d-flex flex-column ga-3">
                                        <v-btn variant="tonal" class="font-weight-bold">Entrar</v-btn>
                                        <v-card-subtitle class="text-center bg-teal-darken-4 rounded-lg pa-4">Não tem uma conta? <br/><v-btn variant="tonal" @click="step = 2">Crie!</v-btn></v-card-subtitle>
                                    </v-card-actions>
                                </div>

                            </div>


                        </v-card>
                    </v-form>
                </v-window-item>
                <v-window-item class="h-100 w-100" :value="2">
                    <v-form class="h-100 w-100" @submit="handleSubmit">
                        <v-card class="h-100 w-100 d-flex justify-center bg-teal-darken-1 align-center">

                            <div class="w-50 h-100 pa-4 flex-column d-none d-sm-flex justify-space-evenly align-center">
                                <v-card-title class="text-uppercase font-weight-bold">Crie sua conta</v-card-title>
                                <div class="w-100">
                                    <v-text-field placeholder="email"></v-text-field>
                                    <v-text-field placeholder="Senha"></v-text-field>
                                    <v-text-field placeholder="Repetir"></v-text-field>
                                </div>

                                
                                <v-card-actions>
                                    <v-btn variant="tonal" class="font-weight-bold">Criar</v-btn>
                                </v-card-actions>
                            </div>

                            <div class="w-50 h-100  register-form-bg-image flex-grow-1">
                                <v-sheet position="absolute" class="bg-overlay">

                                </v-sheet>

                                <div class="remove-overlay d-flex justify-space-between align-center flex-column pa-4 h-100 w-100 text-white">
                                    <v-card-subtitle tag="h1" class="text-h4 text-uppercase font-weight-bold bg-teal-darken-4 pa-3 rounded-lg text-wrap text-center" :style="{lineHeight: '1.5'}">Life Cris - Registrar</v-card-subtitle>
                                    <div class="w-100 h-100 pa-4 flex-column d-flex d-sm-none justify-space-evenly align-center">
                                        <div class="w-100">
                                            <v-text-field placeholder="email"  v-model="email"></v-text-field>
                                            <v-text-field placeholder="Senha"  v-model="password"></v-text-field>
                                            <v-text-field placeholder="repetir senha" class="text-yellow"  v-model="repeatPassword"></v-text-field>
                                        </div>
                                 
                                    </div>
                                    <v-card-actions class="d-flex flex-column ga-3">
                                        <v-btn type='submit' variant="tonal" class="font-weight-bold">Criar</v-btn>
                                        <v-card-subtitle class="text-center bg-teal-darken-4 rounded-lg pa-4">Já tem uma conta? <br/><v-btn variant="tonal" @click="step = 1">Entre!</v-btn></v-card-subtitle>
                                    </v-card-actions>
                                </div>

                            </div>


                        </v-card>
                    </v-form>
                </v-window-item>
            </v-window>
             
    </v-main>
</template>

<script lang="ts" setup>
const router = useRouter()
const step = ref<number>(1)
const email = ref<string>('')
const password = ref<string>('')
const repeatPassword = ref<string>('')
const errorMsg = ref<string>('')
const successMsg = ref<string>('')

const client = useSupabaseClient()
const handleSignIn = async (e) => {
    try{
        e.preventDefault()
        if(!email.value || !password.value) return console.log('b')
        const {error} = await client.auth.signInWithPassword({
            email: email.value,
            password: password.value,
            options: {
                redirectTo: 'http://localhost:3000/confirm',
            }
        })
        if(error) throw error
    }
    catch(e){
        errorMsg.value = e
    }
}
const handleSubmit = async (e) => {
    if(!email.value || !password.value) return console.log('b')

   await signUp()
}
async function signUp(): void{
    try {
        const {data, error} = client.auth.signUp({
            email: email.value,
            password: password.value
        })
        if (error) throw error

        successMsg.value = "Check Your email to confirm your account"
    } catch (error) {
        console.log(error)
        errorMsg.value = error.message
    }
}

definePageMeta({
  layout: 'empty',
  middleware: 'need-no-user'
})

</script>