<template>
  <div>
    <div class="flex items-center justify-between">
      <h1 class="font-bold text-2xl">Cadastros</h1>
    </div>

    <AppAlert
      :message="message"
      :alert="alert"
    />

    <AppDialog
      :message="message"
      :dialog="dialog"
      v-model="dialog"
      @deleteCadastro="deleteCadastro"
      @hiddenDialog="hiddenDialog"
    />

    <div class="mt-6">
      <div>
        <div class="flex items-center space-x-3">
          <div>
            <AppFormInput v-model="nome" placeholder="Nome" />
          </div>

          <div>
            <AppFormInput
              v-model="data_nascimento"
              placeholder="Data Nascimento"
              type="date"
            />
          </div>

          <AppButton @click="addCadastro"> Adicionar </AppButton>
        </div>
      </div>

      <table class="mt-4 min-w-full divide-y divide-gray-200 shadow">
        <thead class="bg-gray-50">
          <tr>
            <th
              scope="col"
              colspan="3"
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Cadastro
            </th>
            <th scope="col" class="relative px-6 py-3">
              <span class="sr-only">Editar</span>
            </th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          <tr v-for="cadastro in cadastros" :key="cadastro.id" class="bg-white">
            <td
              class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"
            >
              <div v-if="is_updating === cadastro.id" class="w-72">
                <AppFormInput v-model="cadastro.nome" />
              </div>

              <template v-else>
                {{ cadastro.nome }}
              </template>
            </td>

            <td
              class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"
            >
              <div v-if="is_updating === cadastro.id" class="w-72">
                <AppFormInput v-model="cadastro.data_nascimento" type="date" />
              </div>

              <template v-else>
                {{ $moment(cadastro.data_nascimento).format("DD/MM/YYYY") }}
              </template>
            </td>

            <td
              class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"
            >
              {{ cadastro.idade }}
            </td>

            <td
              class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium space-x-4"
            >
              <div v-if="is_updating !== cadastro.id">
                <a
                  href="#"
                  class="text-indigo-600 hover:text-indigo-900"
                  @click.stop.prevent="toUpdate(cadastro.id)"
                  >Editar
                </a>

                <a
                  href="#"
                  class="text-red-600 hover:text-red-900"
                  @click.stop.prevent="showDialog(cadastro)"
                  >Excluir
                </a>
              </div>
              <div v-else>
                <a
                  href="#"
                  class="text-indigo-600 hover:text-indigo-900"
                  @click.stop.prevent="updateCadastro(cadastro)"
                  >Salvar
                </a>

                <a
                  href="#"
                  class="text-red-600 hover:text-red-900"
                  @click.stop.prevent="toUpdate(0)"
                  >Cancelar
                </a>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import AppButton from "~/components/Ui/AppButton"
import AppFormInput from "~/components/Ui/AppFormInput"
import AppFormLabel from "~/components/Ui/AppFormLabel"
import AppAlert from "~/components/Ui/AppAlert"
import AppDialog from "~/components/Ui/AppDialog"

export default {
  nome: "cadastrosPage",
  components: {
    AppButton,
    AppFormInput,
    AppFormLabel,
    AppAlert,
    AppDialog,
  },

  data: () => ({
    nome: '',
    data_nascimento: '',
    message: '',
    alert: '',
    dialog: '',
    is_updating: 0,
    id: 0,
  }),

  async asyncData({ $axios }) {
    const cadastros = await $axios.$get("cadastros")
    return {
      cadastros,
    }
  },

  methods: {
    async loadCadastros() {
      this.cadastros = await this.$axios.$get("cadastros")
      this.toUpdate(0)
    },

    showDialog(cadastro) {
      this.id = cadastro.id
      this.message = cadastro.nome
      this.dialog = "removeCadastro"
    },

    hiddenDialog() {
      this.id = ""
      this.message = ""
      this.alert = ""
      this.dialog = ''
    },

    showNotification(message, alert) {
      this.message = message
      this.alert = alert

      setTimeout(() => {
        this.alert = ""
      }, 3000)
    },

    deleteCadastro() {
      this.$axios
        .$delete(`/cadastros/${this.id}`)
        .then((response) => {
          this.hiddenDialog()
          this.showNotification("Cadastrado inserido com sucesso!", "success")
          this.loadCadastros()
        })
        .catch((error) => {
          this.showNotification(error.message, "alert")
        })
    },

    toUpdate(id) {
      this.is_updating = id
    },

    updateCadastro(cadastro) {
      const data = {
        nome: cadastro.nome,
        data_nascimento: cadastro.data_nascimento,
      }
      this.$axios
        .$put(`/cadastros/${cadastro.id}`, data)
        .then((response) => {
          this.showNotification(
            "Cadastrado ataulizado com sucesso!",
            "success"
          )
          this.loadCadastros()
        })
        .catch((error) => {
          this.showNotification(error.message, "alert")
        })
    },

    addCadastro() {
      if (!this.nome) {
        return
      }

      if (!this.data_nascimento) {
        return
      }

      const data = {
        nome: this.nome,
        data_nascimento: this.data_nascimento,
      }
      this.$axios
        .$post("/cadastros", data)
        .then((response) => {
          this.nome = ""
          this.data_nascimento = ""
          this.showNotification("Cadastrado inserido com sucesso!", "success")
          this.loadCadastros()
        })
        .catch((error) => {
          this.showNotification(error.message, "alert")
        })
    },
  },
}
</script>