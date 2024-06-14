<template>
  <div class="flex h-screen flex-col">
    <header class="bg-gray-200 flex items-center p-4">
      <form @submit.prevent="fetchAnswer" class="w-full">
        <input
          type="text"
          id="question"
          v-model="question"
          placeholder="Digite algo..."
          class="w-4/5 p-2 border border-gray-300 rounded mr-2"
        />
        <button
          class="p-2 bg-blue-500 text-white rounded"
          type="submit"
          :disabled="!question"
        >
          {{ `${isLoading ? "Aguardando..." : "Enviar"}` }}
        </button>
      </form>
    </header>
    <div class="p-4 bg-slate-100 flex-1">
      <AIAnswer :answer="answer" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useGetGenerativeModelGP } from "../composables/useGetGenerativeModelGP.js";
import AIAnswer from "./AiAnswer.vue";

const question = ref("");
const answer = ref("");
const isLoading = ref(false);

const fetchAnswer = async () => {
  answer.value = "";
  isLoading.value = true;

  const context = `Possuo duas coleções num banco de dados MongoDB,
uma se chama instituicoes e tem documentos com o seguinte formato:
{
  "_id": {
    "$oid": "66673f1cb03fee28b774a86d"
  },
  "nome": "Universidade Federal Rural de Pernambuco",
  "endereco": {
    "rua": "Rua Dom Manoel de Medeiros",
    "numero": "521",
    "bairro": "Dois Irmãos",
    "cidade": "Recife",
    "estado": "Pernambuco",
    "cep": "52171-900"
  },
  "telefones": [
    "81999999999"
  ],
  "cnpj": "24.162.204/0001-06",
  "sigla": "ufrpe",
  "estudantes": [
    {
      "_id": {
        "$oid": "666787e6dcfb74842a7191fe"
      },
      "nome": "Reginaldo Rossi",
      "cpf": "051.058.059-51",
      "periodo": 5,
      "curso": "Fisoterapia"
    },
    {
      "_id": {
        "$oid": "66678820a22f99cc14c6dc91"
      },
      "nome": "Augusto Cesar",
      "cpf": "077.078.079-71",
      "periodo": 6,
      "curso": "Educacao Fisica"
    }
  ]
}

A outra se chama servicos e tem documentos no seguinte formato:
{
  "_id": {
    "$oid": "666744298b35e600228fc44a"
  },
  "nome": "Oncologia",
  "sigla": "onco",
  "responsavel": {
    "nome": "Dr. José da Silva",
    "crm": "123456"
  },
  "capacidade": 10,
  "vagas": [
    {
      "estudante_id": {
        "$oid": "666741026107f4095f542723"
      },
      "inicio": {
        "$date": "2023-06-01T00:00:00.000Z"
      },
      "fim": {
        "$date": "2023-12-31T00:00:00.000Z"
      }
    },
    {
      "estudante_id": {
        "$oid": "666741eb994a98c9c7a9b95f"
      },
      "inicio": {
        "$date": "2024-06-01T00:00:00.000Z"
      },
      "fim": {
        "$date": "2024-12-31T00:00:00.000Z"
      }
    }
  ]
}

Na coleção 'servicos', cada elemento do array vagas possui um atributo estudante_id que faz referência a um elemento do array estudantes,
da coleção 'instituicoes', dessa forma servicos.vagas[i].estudante_id = instituicoes.estudantes._id.
Considerando essas coleções, eu farei uma pergunta que você deve responder com uma query mongodb, juntamente com uma explicação do que a query faz.
Esta é a pergunta:

`;

  try {
    answer.value = await useGetGenerativeModelGP(context + question.value);
  } catch (error) {
    console.log({ error });
  } finally {
    isLoading.value = false;
    question.value = "";
  }
};
</script>

<style lang="scss" scoped>
.mb-5 {
  margin-bottom: 5rem;
}
.mb-10 {
  margin-bottom: 10rem;
}
</style>
