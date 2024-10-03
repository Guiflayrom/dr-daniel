<template>
  <VCard elevation="5" class="pa-5">
    <!-- Informação sobre a dívida -->
    <div class="text-lg">
      <VRow>
        <VCol>Divída</VCol>
        <VCol>{{ divida }}</VCol>
      </VRow>
      <VRow>
        <VCol>Credor</VCol>
        <VCol>{{ credor }}</VCol>
      </VRow>
      <VRow>
        <VCol>Valor</VCol>
        <VCol>{{ valor }}</VCol>
      </VRow>
      <VRow>
        <VCol>Data de Vencimento</VCol>
        <VCol>{{ vencimento }}</VCol>
      </VRow>

      <!-- Botão para abrir a proposta do credor -->
      <VRow v-if="show_proposta_btn">
        <VCol>Ver Proposta</VCol>
        <VCol>
          <v-dialog v-model="dialog" max-width="900" persistent>
            <template v-slot:activator="{ props: activatorProps }">
              <VBtn
                class="text-none font-weight-regular"
                v-bind="activatorProps"
                color="info"
                >Proposta</VBtn
              >
            </template>

            <v-card align="center">
              <VRow
                class="d-flex align-center justify-center"
                style="height: 100%"
              >
                <VCol>
                  <div class="text-2xl ml-15">Detalhes da dívida</div>
                  <div class="pa-5 text-1xl ml-7">
                    <!-- Informações da dívida e proposta do credor -->
                    <VRow>
                      <VCol>Divída</VCol>
                      <VCol>{{ divida }}</VCol>
                    </VRow>
                    <VRow>
                      <VCol>Credor</VCol>
                      <VCol>{{ credor }}</VCol>
                    </VRow>
                    <VRow>
                      <VCol>Valor Original</VCol>
                      <VCol>{{ valor }}</VCol>
                    </VRow>
                    <VRow>
                      <VCol>Proposta do credor</VCol>
                      <VCol>{{ proposta_credor }}</VCol>
                    </VRow>

                    <VRow>
                      <VCol>Aceitar proposta</VCol>
                      <VCol>
                        <VBtn color="info" @click="acceptProposal"
                          >Aceitar</VBtn
                        >
                      </VCol>
                    </VRow>
                  </div>
                </VCol>

                <VCol>
                  <VCard elevation="4" class="pa-5 ma-5 chatbot-container">
                    <!-- Área da lista de mensagens -->
                    <div ref="chatList" class="chat-list">
                      <VList>
                        <VListItem
                          v-for="(msg, index) in messages"
                          :key="index"
                        >
                          <VListItemContent>
                            <div
                              :class="
                                msg.sender === 'user'
                                  ? 'text-right'
                                  : 'text-left'
                              "
                            >
                              <strong
                                >{{
                                  msg.sender === "user" ? "Você" : "Chatbot"
                                }}:</strong
                              >
                              {{ msg.text }}
                            </div>
                          </VListItemContent>
                        </VListItem>
                      </VList>
                    </div>

                    <!-- Área do prompt (campo de mensagem) -->
                    <div class="chat-prompt">
                      <VRow class="mt-5">
                        <VCol cols="10">
                          <VTextField
                            v-model="userInput"
                            variant="outlined"
                            placeholder="Digite sua mensagem aqui..."
                          />
                        </VCol>
                        <VCol cols="2" class="mt-1">
                          <VBtn
                            color="primary"
                            @click="sendMessage"
                            icon="mdi-plus"
                          >
                            <v-icon> mdi-send</v-icon>
                          </VBtn>
                        </VCol>
                      </VRow>
                    </div>
                  </VCard>
                </VCol>
              </VRow>
              <!-- Componente do Chatbot -->

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn text="Fechar" @click="dialog = false"></v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </VCol>
      </VRow>
    </div>
  </VCard>
</template>

<script lang="ts" setup>
import { ref, toRefs, nextTick } from "vue";
import { useFetch } from "nuxt/app"; // Utiliza a função fetch do Nuxt 3

// Definindo propriedades recebidas no componente
const props = defineProps<{
  divida: string;
  credor: string;
  valor: string;
  vencimento: string;
  proposta_credor: string;
  show_proposta_btn: boolean;
}>();

const { divida, credor, valor, vencimento, proposta_credor } = toRefs(props);

// Definindo referências para controle de diálogos e histórico de mensagens
const dialog = ref(false);
const chatList = ref<HTMLDivElement | null>(null); // Referência para o container da lista de mensagens

// Referência para armazenar a mensagem do usuário e histórico de mensagens
const userInput = ref("");
const messages = ref([
  {
    sender: "Chatbot",
    text: `Olá! Vejo que você possui uma dívida de ${valor.value} com ${credor.value}. Como posso ajudar na negociação hoje?`,
  },
]);

// Função para enviar mensagem e interagir com o chatbot
const sendMessage = async () => {
  // Adiciona a mensagem do usuário no histórico
  messages.value.push({ sender: "user", text: userInput.value });

  // Simula o corpo do request para a API com o histórico completo de mensagens
  const requestBody = {
    model: "gpt-4o",
    messages: [
      {
        role: "system",
        content: `Você é um assistente especializado em negociação de dívidas. As informações do usuário são:
          - Dívida: ${divida.value}
          - Credor: ${credor.value}
          - Valor Original: ${valor.value}
          - Proposta do Credor: ${proposta_credor.value}
          - Data de Vencimento: ${vencimento.value}
          O objetivo é auxiliar o usuário a simular negociações de dívida, permitindo opções de parcelamento, desconto, adiamento de pagamento, e solicitação de reuniões.`,
      },
      ...messages.value.map((msg) => ({
        role: msg.sender === "user" ? "user" : "assistant",
        content: msg.text,
      })),
    ],
  };

  // Faz a chamada à API usando fetch e aguarda a resposta
  const { data } = await useFetch(
    "https://api.openai.com/v1/chat/completions",
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer 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`, // Adicione sua chave aqui ou configure a variável de ambiente
      },
      body: JSON.stringify(requestBody),
    }
  );

  // Obtém a resposta da API e adiciona no histórico de mensagens
  const chatbotResponse =
    data.value.choices[0].message.content ||
    "Desculpe, não entendi a solicitação. Pode repetir?";

  // Adiciona a resposta do chatbot no histórico
  messages.value.push({ sender: "Chatbot", text: chatbotResponse });

  // Limpa o campo de input
  userInput.value = "";

  // Aguarda a renderização da nova mensagem e rola o chat para o final
  await nextTick();
  if (chatList.value) {
    chatList.value.scrollTop = chatList.value.scrollHeight;
  }
};

// Função para aceitar proposta do credor
const acceptProposal = () => {
  messages.value.push({
    sender: "Chatbot",
    text: "Proposta aceita! Vamos proceder com o pagamento.",
  });

  // Rolar a lista para o final após a atualização
  nextTick(() => {
    if (chatList.value) {
      chatList.value.scrollTop = chatList.value.scrollHeight;
    }
  });
};
</script>
<style scoped>
.chatbot-container {
  background-color: rgb(230, 230, 230);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.chat-list {
  height: 300px; /* Altura fixa para a lista de mensagens */
  overflow-y: auto; /* Ativa a rolagem vertical */
  padding: 10px;
  border: 1px solid #ccc;
  margin-bottom: 10px; /* Espaço entre a lista e o prompt */
}

.chat-prompt {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 10px;
  border-top: 1px solid #ccc;
}
</style>
