<template>
    <div class="page-edit-team">
        <nav class="breadcrumb" aria-label="breadcrumbs">
            <ul>
                <li><router-link to="/dashboard">Controle</router-link></li>
                <li><router-link to="/dashboard/my-account">Minha Conta</router-link></li>
                <li class="is-active"><router-link to="/dashboard/my-account/edit-team" aria-current="true">Editar grupo</router-link></li>
            </ul>
        </nav>

        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Editar grupo</h1>
            </div>

            <div class="column is-12">
                <div class="field">
                    <label>Nome</label>
                    <div class="control">
                        <input type="text" class="input" v-model="team.name">
                    </div>
                </div>

                <div class="field">
                    <label>CNPJ</label>
                    <div class="control">
                        <input type="text" class="input" v-model="team.org_number">
                    </div>
                </div>

                <div class="field">
                    <label>Número da primeira fatura</label>
                    <div class="control">
                        <input type="number" class="input" v-model="team.first_invoice_number">
                    </div>
                </div>

                <div class="field">
                    <label>Conta do banco</label>
                    <div class="control">
                        <input type="text" class="input" v-model="team.bankaccount">
                    </div>
                </div>

                <div class="field">
                    <label>Email</label>
                    <div class="control">
                        <input type="email" class="input" v-model="team.email">
                    </div>
                </div>

                <div class="field">
                    <label>Endereço</label>
                    <div class="control">
                        <input type="text" class="input" v-model="team.address1">
                    </div>
                </div>

                <div class="field">
                    <label>Endereço 2</label>
                    <div class="control">
                        <input type="text" class="input" v-model="team.address2">
                    </div>
                </div>

                <div class="field">
                    <label>CEP</label>
                    <div class="control">
                        <input type="text" class="input" v-model="team.zipcode">
                    </div>
                </div>

                <div class="field">
                    <label>Cidade</label>
                    <div class="control">
                        <input type="text" class="input" v-model="team.place">
                    </div>
                </div>

                <div class="field">
                    <div class="control">
                        <button class="button is-success" @click="submitForm">Save</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
    name: 'EditTeam',
    data() {
        return {
            team: {}
        }
    },
    async mounted() {
        await this.getOrCreateTeam()
    },
    methods: {
        getOrCreateTeam() {
            axios
                .get('/api/v1/teams/')
                .then(response => {
                    this.team = response.data[0]
                })
                .catch(error => {
                    console.log(JSON.stringify(error))
                })
        },
        submitForm() {
            axios
                .patch(`/api/v1/teams/${this.team.id}/`, this.team)
                .then(response => {
                    toast({
                        message: 'The changes was saved',
                        type: 'is-success',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 2000,
                        position: 'bottom-right',
                    })

                    this.$router.push('/dashboard/my-account')
                })
                .catch(error => {
                    console.log(JSON.stringify(error))
                })
        }
    }
}
</script>