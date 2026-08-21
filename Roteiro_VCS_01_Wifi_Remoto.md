# Roteiro — Vídeo 01
## Trocar SSID e senha do Wi-Fi remotamente, sem deslocar técnico

**Série:** VCS na prática — Fase 1
**Duração alvo:** 5 a 6 minutos
**Formato:** screencast + narração (sem câmera)
**Idioma:** pt-BR
**Módulo de origem:** 3.4 (editando e salvando configuração) + 7.4 (diagnóstico de Wi-Fi)

---

## 1. Preparação antes de apertar REC

### Ambiente
- [ ] Tenant/lab de demonstração — **não** gravar sobre a base de produção da Direct
- [ ] Assinante fictício criado: nome, serial GPON, MAC, PPPoE e IP inventados
- [ ] CPE de teste **online** e respondendo Inform (confirmar antes de gravar)
- [ ] SSID de partida definido (ex.: `DIRECT-DEMO-2G`) para haver o que trocar

### Navegador
- [ ] Janela anônima, sem extensões, sem barra de favoritos
- [ ] Zoom em **125%** — a UI do VCS fica ilegível no celular em 100%
- [ ] Resolução da tela em 1920×1080

### OBS
- [ ] Captura de janela (não de tela inteira — evita notificação aparecendo)
- [ ] Destaque de cursor ligado
- [ ] Gravação 1080p / 30fps
- [ ] Teste de áudio de 20 segundos antes — ouvir de volta

### Anonimização — revisar ANTES de publicar
- [ ] Nenhum serial, MAC ou IP real visível
- [ ] Nenhum nome, CPF ou telefone de assinante real
- [ ] URL do VCS da Direct borrada ou substituída
- [ ] Usuário logado não é credencial nominal de produção
- [ ] Nenhuma outra aba/aba do navegador com dado de cliente

---

## 2. Roteiro

### BLOCO 1 — Gancho (0:00 – 0:12)

**TELA:** já na lista de CPEs do VCS, parada.

**FALA:**
> "O assinante ligou: trocou de celular, não lembra a senha do Wi-Fi e quer mudar o nome da rede. Antigamente isso era visita técnica. Hoje resolve em dois minutos, do próprio NOC. Vou te mostrar."

**TEXTO NA TELA:** `Trocar SSID e senha do Wi-Fi — sem deslocar técnico`

> Nada de intro animada. Corta direto.

---

### BLOCO 2 — Localizar o assinante (0:12 – 1:00)

**TELA / AÇÃO:**
1. Campo de busca da lista de CPEs
2. Digitar o serial GPON do assinante fictício
3. Resultado aparece — clicar para abrir o CPE

**FALA:**
> "Primeiro passo é achar o equipamento. A busca aceita três coisas: o serial GPON, o usuário PPPoE ou o ID do assinante. Na prática o que você vai ter em mãos depende de onde veio o chamado — se veio do ERP, provavelmente é o ID; se o técnico passou por telefone, costuma ser o serial."
>
> "Antes de mexer em qualquer coisa, olha o indicador de status aqui. Ele precisa estar online. Se estiver offline, a configuração até vai ser aceita, mas fica na fila — e o cliente vai desligar o telefone achando que resolveu."

**TEXTO NA TELA:** `Busca: serial GPON · usuário PPPoE · ID do assinante`

> **Pausa aqui de propósito.** Deixa o status do dispositivo na tela por 2 segundos antes de seguir.

---

### BLOCO 3 — Abrir a configuração de Wi-Fi (1:00 – 1:40)

**TELA / AÇÃO:**
1. Barra de ações do CPE — mostrar rapidamente as abas disponíveis
2. Abrir a aba **WiFi**
3. Mostrar as duas rádios: 2.4 GHz e 5 GHz

**FALA:**
> "Aberto o dispositivo, você tem essa barra de abas: LAN, WAN, WiFi, Devices, VOIP, Tasks e Events. A gente vai na WiFi."
>
> "Repara que aparecem as duas rádios separadas: 2.4 e 5 gigahertz. Isso importa. Se o CPE não estiver com band steering ativo, elas são redes independentes — trocar a senha de uma não troca a da outra. É a causa número um de retorno de chamado nesse tipo de atendimento."

**TEXTO NA TELA:** `2.4 GHz e 5 GHz são redes separadas`

---

### BLOCO 4 — Editar e salvar (1:40 – 3:10)

**TELA / AÇÃO:**
1. Alterar o campo **SSID** do rádio 2.4 GHz
2. Alterar o campo **KeyPassphrase** (senha)
3. Repetir no rádio 5 GHz
4. Clicar em **Salvar / Apply**

**FALA:**
> "Troco o SSID aqui. Uma dica de campo: combine o nome com o assinante antes de salvar, porque no momento em que isso for aplicado, todo dispositivo que está conectado vai cair e vai precisar reconectar com a rede nova. Celular, TV, câmera, tudo. Se o cara está numa reunião, ele não vai gostar."
>
> "Agora a senha. E aqui vale falar de uma coisa que confunde muito técnico novo..."
>
> "Depois salvo, e repito no rádio de 5."

**TEXTO NA TELA:** `Trocar SSID derruba todos os dispositivos conectados`

---

### BLOCO 5 — Confirmar a execução (3:10 – 4:20)

**TELA / AÇÃO:**
1. Abrir a aba **Tasks**
2. Mostrar a task criada e o status dela
3. Abrir a aba **Events** e mostrar o Inform correspondente
4. Voltar na aba **WiFi** — o SSID novo aparece, o campo de senha aparece **vazio**

**FALA:**
> "Salvar não significa aplicado. O VCS cria uma task, e essa task só executa quando o CPE conversa com o servidor. Então o lugar de confirmar é aqui, na aba Tasks: status concluído significa que o equipamento recebeu e aplicou."
>
> "Na aba Events você vê o Inform que o CPE mandou de volta. É a prova real de que chegou no equipamento."
>
> "E agora o detalhe que eu te prometi. Volta na aba WiFi: o SSID novo está lá, certinho. Mas o campo da senha está vazio. E não, não deu errado."

---

### BLOCO 6 — O erro comum (4:20 – 5:10)

**TELA / AÇÃO:** campo de senha vazio em destaque.

**FALA:**
> "No padrão TR-181, o parâmetro `KeyPassphrase` é **write-only**. Você consegue escrever, nunca ler. O ACS manda a senha pro CPE, o CPE aplica, mas quando o servidor pergunta de volta 'qual é a senha?', o equipamento devolve vazio. Isso é proteção, é o comportamento correto."
>
> "O que eu vejo acontecer direto: o técnico troca a senha, volta na aba, vê o campo em branco, acha que não funcionou e troca de novo. Aí o cliente leva três senhas diferentes em cinco minutos."
>
> "A regra prática é: **você define a senha, você não consulta a senha.** Anota no chamado o que você configurou. Se o cliente esqueceu, você não recupera — você define uma nova."
>
> "Detalhe: alguns firmwares fora do padrão devolvem esse campo preenchido. Se acontecer no seu parque, não comemore — é o fabricante que está fora da especificação, e isso é um problema de segurança, não um recurso."

**TEXTO NA TELA:** `KeyPassphrase é write-only (TR-181) — você define, não consulta`

---

### BLOCO 7 — Fecho (5:10 – 5:30)

**FALA:**
> "Resumindo: acha o CPE, confirma que está online, edita nas duas rádios, salva, e confirma na aba Tasks. E lembra que senha de Wi-Fi não se lê — se redefine."
>
> "No próximo vídeo a gente vai no caso oposto: o CPE que sumiu do VCS, e o que verificar antes de abrir chamado de campo."

**TEXTO NA TELA:** `Próximo: o CPE sumiu do VCS`

---

## 3. Publicação — YouTube

**Título:**
`Como trocar a senha e o nome do Wi-Fi remotamente pelo ACS (TR-069) | VCS na prática #1`

**Descrição:**
```
Atendimento de Wi-Fi é o chamado mais comum em qualquer provedor — e o mais
fácil de resolver sem deslocar técnico. Neste vídeo eu mostro o passo a passo
completo no Aprecomm VCS: localizar o assinante, editar SSID e senha nas duas
rádios, e confirmar que a configuração realmente chegou no equipamento.

No final, o erro que faz técnico trocar a senha do cliente três vezes seguidas.

⏱ Capítulos
00:00 O chamado
00:12 Localizando o assinante
01:00 A aba WiFi e as duas rádios
01:40 Editando SSID e senha
03:10 Confirmando na aba Tasks
04:20 Por que o campo de senha volta vazio
05:10 Resumo

Série "VCS na prática" — conteúdo para equipes de NOC e suporte de provedores.
Ambiente de laboratório, dados fictícios.
```

**Tags:** `tr-069`, `acs`, `provedor de internet`, `isp`, `noc`, `suporte tecnico isp`, `cwmp`, `wifi`, `gpon`, `cpe`, `tr-181`

**Thumbnail:** campo de senha vazio em close, com um `?` grande. Texto curto: `SENHA VAZIA` — sem mais que duas palavras.

---

## 4. Publicação — LinkedIn

**Corte:** Bloco 6 (4:20–5:10), o trecho do write-only. Cerca de 50 segundos, legenda queimada, vertical 9:16.

**Texto do post:**
```
Técnico troca a senha do Wi-Fi do assinante pelo ACS. Salva. Volta na tela e o
campo está vazio.

Conclusão natural: "não funcionou". Troca de novo. E de novo.

O cliente recebe três senhas diferentes em cinco minutos, e ninguém entendeu
o que aconteceu.

O que aconteceu é que o KeyPassphrase, no TR-181, é write-only. O equipamento
aceita receber a senha e nunca a devolve. É o comportamento correto — proteção
de credencial.

A regra prática pro atendimento é simples: senha de Wi-Fi você define, não
consulta. Registra no chamado o que configurou. Se o assinante esqueceu, não
existe recuperar — existe redefinir.

E se o seu parque tem firmware que devolve esse campo preenchido: não é recurso,
é fabricante fora da especificação.

Primeiro vídeo da série que estou gravando sobre operação de ACS no dia a dia
do NOC. Link nos comentários.
```

> Link no primeiro comentário, não no corpo do post — o alcance cai bastante quando o link vai no texto.
