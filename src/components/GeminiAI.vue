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

  const context = `Considere os schemas abaixo:
  estudantes = {
      _id: ObjectId()
      nome: string,
      cpf: string,
      periodo: number,
      curso: string
    }

  instituicoes =
{
  _id:  ObjectId(),
  nome: string,
  endereco: {
    rua: string,
    numero: string,
    bairro: string,
    cidade: string,
    estado: string,
    cep: string
  },
  telefones: string[],
  cnpj: string,
  sigla: string,
  estudantes: estudante[]
}

vaga = {
  estudante_id: ObjectId(), //referencia um estudante em instituicoes.estudantes[]
  inicio: Date,
  fim: Date,
  ativa: boolean
}

servicos = {
  _id: ObjectId(),
  nome: string,
  sigla: string,
  responsavel: {
    nome: string,
    crm: string,
  },
  capacidade: number,
  vagas: vaga[]
}

Esses schemas são implementados por DUAS coleções num banco de dados MongoDB, 'instituicoes' e 'servicos'.

A coleção 'instituicoes' possum um array de estudantes.

A coleção 'servicos' possui um array de vagas onde cada elemento desse array possui uma referência a UM estudante
do array de estudantes da coleção 'instituicoes'.

Responda a pergunta a seguir com uma consulta em MongoDB e uma explicação detalhada do seu raciocínio.

A resposta será composta de DUAS partes: a consulta em MongoDB e a explicação detalhada do raciocínio.
Formate a resposta utilizando markdown.


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
