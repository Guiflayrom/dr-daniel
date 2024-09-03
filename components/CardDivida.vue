<template>
  <VCard elevation="5" class="pa-5">
    <div class="text-lg">
      <VRow>
        <VCol>
          <div>Divída</div>
        </VCol>
        <VCol>
          {{ divida }}
        </VCol>
      </VRow>
      <VRow>
        <VCol> Credor </VCol>
        <VCol>
          {{ credor }}
        </VCol>
      </VRow>
      <VRow>
        <VCol> Valor </VCol>
        <VCol>
          {{ valor }}
        </VCol>
      </VRow>
      <VRow>
        <VCol> Data De Vencimento </VCol>
        <VCol>
          {{ vencimento }}
        </VCol>
      </VRow>
      <VRow v-if="show_proposta_btn">
        <VCol> Ver Proposta </VCol>

        <VCol>
          <v-dialog v-model="dialog" max-width="600" persistent>
            <template v-slot:activator="{ props: activatorProps }">
              <VBtn
                class="text-none font-weight-regular"
                text="Edit Profile"
                v-bind="activatorProps"
                color="info"
                >Proposta</VBtn
              >
            </template>

            <v-card>
              <div class="text-3xl pa-5" align="center">Detalhes da dívida</div>

              <div class="pa-5 text-1xl ml-7">
                <VRow>
                  <VCol>
                    <div>Divída</div>
                  </VCol>
                  <VCol>
                    {{ divida }}
                  </VCol>
                </VRow>
                <VRow>
                  <VCol> Credor </VCol>
                  <VCol>
                    {{ credor }}
                  </VCol>
                </VRow>
                <VRow>
                  <VCol> Valor Original</VCol>
                  <VCol>
                    {{ valor }}
                  </VCol>
                </VRow>

                <VRow>
                  <VCol> Proposta do credor</VCol>
                  <VCol>
                    {{ proposta_credor }}
                  </VCol>
                </VRow>

                <VRow>
                  <VCol> Aceitar proposta</VCol>
                  <VCol>
                    <VBtn color="info" @click="dialog = false">Assinar</VBtn>
                  </VCol>
                </VRow>
              </div>

              <VCard elevation="4" class="pa-5 ma-5">
                <VRow>
                  <VCol cols="10">
                    <VCard elevation="0" class="pa-3">
                      <div align="center">Tire suas dúvidas</div>
                      <VTextField
                        variant="outlined"
                        placeholder="Minha dúvida é a ..."
                      />
                    </VCard>
                  </VCol>
                  <VCol class="mt-7">
                    <VImg src="bot.png"></VImg>
                  </VCol>
                </VRow>
              </VCard>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  text="Fechar"
                  variant="plain"
                  @click="
                    dialog = false;
                    dialog2 = true;
                  "
                ></v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog persistent v-model="dialog2" max-width="600">
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
                <v-textarea
                  placeholder="Escreva brevemente a sua realidade financeira"
                  variant="outlined"
                ></v-textarea>

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
        </VCol>
      </VRow>
    </div>
  </VCard>
</template>

<script lang="ts" setup>
const props = defineProps<{
  divida: string;
  credor: string;
  valor: string;
  vencimento: string;
  proposta_credor: string;
  show_proposta_btn: boolean;
}>();

const dialog = ref(false);
const dialog2 = ref(false);
const { divida, credor, valor, vencimento, proposta_credor } = toRefs(props);
</script>
