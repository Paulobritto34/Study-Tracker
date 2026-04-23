# Study Tracker

Aplicativo pessoal de rastreamento de estudos. SPA contida em um único arquivo `index.html` — sem build, sem dependências locais, basta abrir no navegador.

---

## Funcionalidades

### Dashboard
- Horas estudadas na semana atual com meta configurável
- Dias estudados e sequência (streak) de dias consecutivos
- Mapa da semana com indicadores visuais por dia
- Gráfico de barras dos últimos 7 dias
- Horas distribuídas por trilha

### Pomodoro
- Timer com modos Foco, Pausa e Pausa Longa
- Tempos totalmente configuráveis
- Indicador de ciclos e rounds por sessão
- Auto-save: ao concluir cada ciclo de foco, as horas são salvas automaticamente
- **Nota rápida pós-ciclo:** modal que abre ao concluir uma sessão para registrar aprendizados, dificuldades e próximos passos
- Notificação do navegador ao fim do ciclo

### Player Lo-fi
- 5 estações de rádio online integradas (Chillhop, Rain + Lofi, Jazz Café, Chill Beats, Lo-fi Hip Hop)
- Upload de MP3 local armazenado no IndexedDB (sem limite fixo de tamanho)
- Controle de volume
- Indicador animado de reprodução em andamento

### Registrar
- Formulário de registro manual de sessões de estudo
- Campos: data, horas, trilha, tema e anotações

### Histórico
- Lista completa de todos os registros
- Indicador visual de sessões Pomodoro
- Exclusão individual de registros

### Plano de Estudos
- Roadmap de fases totalmente editável via modal
- Barra de progresso por fase baseada em tópicos concluídos
- Rotina semanal configurável por dia
- Marcos de tempo personalizáveis
- Seção de estudos livres fora da trilha principal

### Sincronização com Firebase
- Login/cadastro via e-mail e senha (Firebase Auth)
- Sincronização em tempo real entre dispositivos via Firestore
- Indicador de status de sync no nav (cinza / amarelo piscando / verde)
- Funciona 100% offline sem conta — dados ficam no Firestore quando logado

---

## Stack

| Camada | Tecnologia |
|--------|-----------|
| Interface | HTML5 + CSS3 + Vanilla JavaScript |
| Armazenamento local | Firestore (logs e plano) + IndexedDB (MP3s) |
| Autenticação | Firebase Auth (Email/Password) |
| Sync em tempo real | Firebase Firestore `onSnapshot` |
| Fontes | Google Fonts — Syne + DM Mono |

Não há framework, bundler, transpiler nem dependências `npm`. O arquivo `index.html` é autocontido e funciona diretamente no navegador.

---

## Como usar

### Sem conta (offline)
Abra `index.html` no navegador. O Pomodoro e o player lo-fi funcionam normalmente. Os dados **não são salvos** sem login.

### Com conta (sync entre dispositivos)
1. Clique em **Entrar** no canto superior direito
2. Crie uma conta com e-mail e senha
3. Todos os registros passam a ser salvos e sincronizados via Firestore em tempo real

---

## Configurar Firebase (instância própria)

Se quiser rodar com seu próprio banco de dados:

**1. Criar projeto**
- Acesse [console.firebase.google.com](https://console.firebase.google.com) e crie um projeto

**2. Ativar Authentication**
- Build → Authentication → Get started → habilite **Email/Password**

**3. Ativar Firestore**
- Build → Firestore Database → Create database → escolha uma região

**4. Configurar regras do Firestore**
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid}/logs/{logId} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

**5. Colar a config no index.html**

Localize o objeto `FIREBASE_CONFIG` no final do `index.html` e substitua pelos valores do seu projeto:

```javascript
const FIREBASE_CONFIG = {
  apiKey: "sua-api-key",
  authDomain: "seu-projeto.firebaseapp.com",
  projectId: "seu-projeto",
  storageBucket: "seu-projeto.appspot.com",
  messagingSenderId: "000000000000",
  appId: "1:000000000000:web:xxxxxxxxxxxx"
};
```

---

## Modelo de dados

```json
{
  "id": 1712345678901,
  "date": "2024-04-05",
  "hours": 1.5,
  "trilha": "Full Stack",
  "tema": "JavaScript — Arrays e objetos",
  "notes": "Dificuldade com closures, rever amanhã",
  "pomo": false
}
```

**Trilhas:** `Full Stack` · `Power Platform` · `Projeto Prático` · `Revisão` · `Outros`

---

## Estrutura do projeto

```
study-tracker-pessoal/
├── index.html      — app completo (HTML + CSS + JS em arquivo único)
└── icon/
    └── acompanhar.png
```

---

## Adicionar estações lo-fi

**Upload de MP3:** na aba Pomodoro → *+ Adicionar estação MP3* → selecione o arquivo → salve. O arquivo é armazenado no IndexedDB do navegador e não é sincronizado via Firebase.

**Rádio online:** edite o array `PRESETS` no `index.html`:
```javascript
const PRESETS = [
  { n: '☁ Chillhop', u: 'https://streams.ilovemusic.de/iloveradio17.mp3' },
  { n: '🔥 Minha Rádio', u: 'https://url-do-stream.mp3' }, // adicione aqui
];
```

---

## Licença

Projeto pessoal — uso livre.
