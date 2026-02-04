# Configuração da API do Strava para BRAVØS

## 🚀 Passos para configurar a integração com Strava

### 1. Criar aplicação no Strava

1. Acesse: https://www.strava.com/settings/api
2. Clique em "Create New App"
3. Preencha os dados:
   - **Application Name**: BRAVØS Calculator
   - **Category**: Other
   - **Website**: http://localhost:8000 (para desenvolvimento)
   - **Authorization Callback Domain**: localhost
   - **Description**: Calculadora de notas IGDCC com integração Strava

### 2. Obter credenciais

Após criar o app, você terá:
- **Client ID**: Um número (ex: 123456)
- **Client Secret**: Uma string longa

### 3. Configurar no código

Edite o arquivo `js/app.js` e substitua os placeholders:

```javascript
const STRAVA_CONFIG = {
    clientId: 'SEU_CLIENT_ID_AQUI', // Substitua pelo ID numérico
    redirectUri: window.location.origin + '/strava-callback',
    scope: 'read,activity:read_all',
    apiUrl: 'https://www.strava.com/api/v3'
};
```

E na função `exchangeCodeForToken`:

```javascript
body: JSON.stringify({
    client_id: STRAVA_CONFIG.clientId,
    client_secret: 'SEU_CLIENT_SECRET_AQUI', // Substitua aqui
    code: code,
    grant_type: 'authorization_code'
})
```

E na função `refreshAccessToken`:

```javascript
body: JSON.stringify({
    client_id: STRAVA_CONFIG.clientId,
    client_secret: 'SEU_CLIENT_SECRET_AQUI', // Substitua aqui
    grant_type: 'refresh_token',
    refresh_token: this.refreshToken
})
```

### 4. Configurar Callback URL

No dashboard do Strava, adicione:
- **Authorization Callback Domain**: localhost (para desenvolvimento)
- **Para produção**: seu domínio real (ex: seudominio.com)

## 🔧 Funcionalidades implementadas

### ✅ OAuth 2.0 Authentication
- Fluxo completo de autenticação
- Refresh tokens automáticos
- Armazenamento seguro no localStorage

### ✅ Importação de Dados
- Busca últimas 10 atividades
- Filtra apenas corridas (Run)
- Importa: distância, tempo, pace
- Preenchimento automático do formulário

### ✅ Interface Amigável
- Modal de seleção de atividades
- Status messages informativos
- Tratamento de erros
- Loading states

## 📱 Como usar

1. **Primeiro acesso**: Clique em "🚴 Importar do Strava"
2. **Autorize**: Faça login no Strava e autorize o app
3. **Selecione**: Escolha uma corrida da lista
4. **Importe**: Dados são preenchidos automaticamente
5. **Calcule**: Use "⚡Desbrava !" para ver a nota

## 🛡️ Segurança

- Tokens armazenados apenas no localStorage (client-side)
- Escopo mínimo necessário (`read,activity:read_all`)
- Tokens expiram automaticamente
- Refresh automático de tokens

## 🚨 Limitações

- **Rate Limiting**: 100 requisições/hora (apps não verificados)
- **Apenas corridas**: Filtra atividades do tipo "Run"
- **Dados básicos**: Importa distância, tempo e pace

## 🔄 Para produção

1. Configure domínio real no Strava
2. Use HTTPS obrigatório
3. Considere backend para maior segurança
4. Verifique rate limits para uso intensivo

## 🐛 Troubleshooting

### "Erro na autenticação"
- Verifique Client ID e Secret
- Confirme callback URL configurada
- Limpe localStorage se necessário

### "Nenhuma corrida encontrada"
- Verifique se há atividades recentes no Strava
- Confirme se são do tipo "Run"
- Verifique permissões concedidas

### "Erro ao buscar atividades"
- Token pode estar expirado
- Verifique rate limiting
- Tente autenticar novamente
