<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">Editar Contato</p>
            </div>
        </div>
    </div>
    <div class="container mt-3">
        <div class="row align-items-center">
            <div class="col-md-4">
                <img :src="contact.photo"
                    alt="" class="contact-img">
            </div>
            <div class="col-md-4">
                <form @submit.prevent="updateSubmit()">
                    <div class="md-2">
                        <input v-model="contact.nome" type="text" class="form-control  my-1" placeholder="Nome">
                    </div>
                    <div class="md-2">
                        <input v-model="contact.photo" type="text" class="form-control  my-1" placeholder="photo URL">
                    </div>
                    <div class="md-2">
                        <input v-model="contact.email" type="email" class="form-control  my-1" placeholder="Email">
                    </div>
                    <div class="md-2">
                        <input v-model="contact.telefone" type="texto" class="form-control  my-1"
                            placeholder="Telefone">
                    </div>
                    <div class="md-2">
                        <input v-model="contact.company" type="text" class="form-control  my-1" placeholder="company">
                    </div>
                    <div class="md-2">
                        <input v-model="contact.titulo" type="text" class="form-control  my-1" placeholder="titulo">
                    </div>
                    <div class="mb-2">
                        <select v-model="contact.groupId" class="form-control " v-if="groups.length > 0">
                            <option value="">Selecione Gupo</option>
                            <option :value="group.id" v-for="group of groups" :key="group.id">{{ group.nome }}</option>
                        </select>
                    </div>
                    <div class="mb-2">
                        <input type="submit" class="btn btn-success" value="Editar">
                    </div>
                </form>
            </div>
        </div>
        <div class="row mt-3">
            <div class="col">
                <router-link to="/" class="btn btn-success"><i class="fa fa-arrow-alt-circle-left"></i></router-link>
            </div>
        </div>
    </div>
</template>

     <script>
import { ContactService } from '@/services/contactService'

export default {
    nome: "EditContact",
    data: function () {
        return {
            contactId: this.$route.params.contactId,
            loading: false,
            contact: {
                nome: '',
                company: '',
                email: '',
                titulo: '',
                telefone: '',
                photo: '',
                grupoId: '',
            },
            errorMessage: null,
            groups: []
        }
    },
    created: async function () {
        try {
            this.loading = true;
            let response = await ContactService.getContact(this.contactId)
            let groupResponse = await ContactService.getALLGroups();
            this.contact = response.data;
            this.groups = groupResponse.data;
            this.loading = false;
        }
        catch (error) {
            this.errorMessage = error;
            this.loading = false;

        }
    },
    methods: {
        updateSubmit: async function () {
            try {
                let response = await ContactService.updateContact(this.contact, this.contactId);
                if (response) {
                    return this.$router.push('/');
                }
                else {
                    return this.$router.push('/contacts/edit/${this.contactId}');
                }
            }
            catch (error) {
                console.log(error);
            }
        }
    }
}
</script>

    <style scoped>
    </style>
