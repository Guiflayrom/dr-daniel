<template>
  <vContainer>
    <div>
      <VRow>
        <VCol>
          <v-img src="logo.png" height="100"></v-img>
        </VCol>
        <VCol class="mt-8 text-xl">
          <div>Resolva sua dívida agora mesmo e recupere sua tranquilidade</div>
          <div>Negocie em minutos com a nossa plataforma!</div>
        </VCol>
      </VRow>
    </div>
    <div
      class="text-lg pa-5 my-5"
      style="
        background-color: white;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        transition: box-shadow 0.3s ease-in-out;
      "
    >
      <VRow>
        <VCol>
          <div>Leandro Soares Marques da Luz</div>
          <div class="text-sm text-gray-700">xxx.xxx.xxx-xx</div>
        </VCol>
        <VCol>
          <div>
            Dívidas encontrada(s): <b>{{ dividas.length }}</b>
          </div>
        </VCol>
        <VCol>
          <VBtn color="success">Fale com um especialista</VBtn>
        </VCol>
      </VRow>
    </div>
    <div>
      <VRow>
        <VCol :cols="mobile ? '12' : '4'" v-for="divida in dividas">
          <CardDivida
            :divida="divida.divida"
            :credor="divida.credor"
            :valor="divida.valor"
            :vencimento="divida.vencimento"
            :proposta_credor="divida.proposta_credor"
            :show_proposta_btn="true"
          ></CardDivida>
        </VCol>
      </VRow>
    </div>
    <div>
      <VCard height="300" color="#e0e3e7" class="mt-10">
        <VCard height="100" class="mt-2 mx-5" align="center">
          <VRow>
            <VCol>
              <div class="text-2xl mt-7">Resultado da proposta</div>
            </VCol>
            <VCol>
              <div class="text-2xl mt-7">Negociação</div>
            </VCol>
          </VRow>
        </VCard>
        <VCard height="60%" class="mt-1 mx-5" align="center">
          <div class="text-2xl">
            <div>
              Sua proposta foi
              <span><b> aceita! </b> </span>
            </div>
            <div>Você pode seguir para a assinatura do acordo</div>
          </div>
          <VBtn color="info" class="mt-10" @click="dialog2 = true"
            >Assinar todas as dívidas</VBtn
          >
        </VCard>
      </VCard>
    </div>

    <v-dialog persistent v-model="dialog2" max-width="1200">
      <v-card>
        <div class="text-3xl pa-5" align="center">
          Agendar Call com especialista
        </div>
        <div class="pa-5">
          Leonardo Soares Marques da Luz
          <VTextField
            label="Valor da sua oferta"
            variant="outlined"
            class="mt-2"
          />
          <VRow>
            <VCol :cols="mobile ? '12' : '4'" v-for="divida in dividas">
              <CardDivida
                :divida="divida.divida"
                :credor="divida.credor"
                :valor="divida.valor"
                :vencimento="divida.vencimento"
                :proposta_credor="divida.proposta_credor"
                :show_proposta_btn="false"
              ></CardDivida>
            </VCol>
          </VRow>
          <div align="center">
            <v-date-picker></v-date-picker>
          </div>
        </div>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            text="Marcar Reunião"
            variant="plain"
            @click="dialog2 = false"
          ></v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </vContainer>
</template>
<script lang="ts" setup>
import { useDisplay } from "vuetify";

const { mobile } = useDisplay();

const dialog2 = ref(false);

const dividas = [
  {
    divida: "Empréstimo ABC",
    credor: "Banco ABC",
    valor: "R$150.000,00",
    vencimento: "08/09/2024",
    proposta_credor: "R$ 4.000 à vista ou 6x de R$700",
  },
  {
    divida: "Empréstimo ABC",
    credor: "Banco ABC",
    valor: "R$150.000,00",
    vencimento: "08/09/2024",
    proposta_credor: "R$ 4.000 à vista ou 6x de R$700",
  },
  {
    divida: "Empréstimo ABC",
    credor: "Banco ABC",
    valor: "R$150.000,00",
    vencimento: "08/09/2024",
    proposta_credor: "R$ 4.000 à vista ou 6x de R$700",
  },
];
</script>

<style>
body {
  background-color: #f1f4f8;
}
</style>
