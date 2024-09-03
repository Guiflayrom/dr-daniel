<template>
  <vContainer>
    <div style="margin-top: 10%">
      <VForm v-model="form">
        <VRow>
          <VCol cols="12">
            <div class="text-4xl mb-5 font-light">Consultar Dívida</div>
          </VCol>
          <VCol cols="12">
            <VTextField :rules="[required]" label="Nome Completo" />
          </VCol>
          <VCol cols="12">
            <VTextField :rules="[required]" label="CPF ou CNPJ" />
          </VCol>
          <VCol cols="12" align="center">
            <VBtn
              color="info"
              align="center"
              :disabled="!form"
              @click="navigateTo('/principal')"
              >Negociar Dívida</VBtn
            >
          </VCol>
        </VRow>
      </VForm>
    </div>
  </vContainer>
</template>

<script lang="ts" setup>
const form = ref(false);

function required(v: any) {
  return !!v || "Field is required";
}
</script>
