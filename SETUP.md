# 🦎 Guia de Configuração - Brasilizia Watch

## Passo 1: Pré-requisitos

✅ Android Studio 2023.1 ou superior
✅ Java 17+
✅ Conta OpenAI com acesso a GPT-4
✅ 4GB RAM mínimo

## Passo 2: Clonar e Abrir Projeto

```bash
git clone https://github.com/arthurbeloagelo-afk/brasilizia-watch.git
cd brasilizia-watch
```

Abra no Android Studio:
- File → Open → Selecione a pasta `brasilizia-watch`
- Espere o Gradle sincronizar

## Passo 3: Obter Chave OpenAI

1. Acesse https://platform.openai.com/account/api-keys
2. Clique em "Create new secret key"
3. Copie a chave (começará com `sk-`)
4. **Nunca compartilhe essa chave!**

## Passo 4: Configurar Chave no App

Abra `app/src/main/kotlin/com/brasilizia/watch/network/OpenAIService.kt`

Procure por:
```kotlin
private val apiKey = "sk-your-api-key-here"
```

Substitua por:
```kotlin
private val apiKey = "sk-sua-chave-aqui"
```

**Alternativa segura (recomendado):**

Crie um arquivo `local.properties` na raiz do projeto:
```properties
openai.api.key=sk-sua-chave-aqui
```

Depois edite `build.gradle` para ler do arquivo.

## Passo 5: Configurar Emulador Wear OS

### Via Device Manager (Recomendado)

1. Android Studio → Tools → Device Manager
2. Clique em "Create Device"
3. Selecione "Wear OS"
4. Modelo: "Pixel Watch" ou "2.5" Round"
5. Sistema: "Android 13" (Wear OS 9)
6. Clique em "Finish"

### Ou via Terminal

```bash
# Listar AVDs disponíveis
emulator -list-avds

# Criar novo emulador Wear OS
~/Android/Sdk/tools/emulator -avd Pixel_Watch -wipe-data
```

## Passo 6: Build e Run

### Opção 1: Android Studio
1. Selecione "Wear OS" emulator no Device Manager
2. Clique no play verde (Run)
3. Selecione `app` para build

### Opção 2: Terminal
```bash
./gradlew build
./gradlew installDebug

# Abre o app no emulador
adb shell am start -n com.brasilizia.watch/.MainActivity
```

## Passo 7: Primeira Execução

Quando o app abrir no relógio:
1. Você verá: "🦎 Brasilizia - Como posso ajudar?"
2. Digitar uma mensagem no campo de input
3. Clicar em "→" para enviar
4. Aguardar resposta da IA (pode levar 2-5 segundos)

## 🐛 Problemas Comuns

### Erro: "Unable to find device"
```bash
# Verifique dispositivos conectados
adb devices

# Reinicie o emulador
adb kill-server
adb start-server
```

### Erro: "Failed to find Build Tools"
```bash
# Sincronize Gradle
./gradlew clean
./gradlew build
```

### Erro: "API Key inválida"
- Verifique se copiou a chave completa
- Teste a chave em: https://platform.openai.com/account/api-keys
- Confirme acesso a GPT-4 (pode precisar de subscription)

### App não carrega mensagens
- Verifique conexão WiFi do emulador
- Abra logcat: View → Tool Windows → Logcat
- Procure por erro de rede/API

### Tela cortada ou distorcida
- O Wear OS tem várias resoluções
- Altere resolução do emulador em Device Manager
- Ajuste tamanhos de fonte em `ChatScreen.kt`

## 🧪 Testando Localmente

### Com Mock Data

Edite `ChatViewModel.kt` para teste offline:

```kotlin
fun sendMessage() {
    val message = _userInput.value.trim()
    if (message.isBlank()) return

    _messages.value = _messages.value + ChatMessage("user", message)
    _userInput.value = ""
    
    // Mock response (sem chamar API)
    viewModelScope.launch {
        delay(1000) // Simula delay
        _messages.value = _messages.value + ChatMessage(
            "assistant",
            "Resposta mock: Você disse '$message'"
        )
    }
}
```

### Verificar Logs

```bash
# Ver logs em tempo real
adb logcat | grep brasilizia

# Salvar logs
adb logcat > logs.txt
```

## ✅ Checklist Final

- [ ] Android Studio instalado
- [ ] Gradle sincronizado
- [ ] Chave OpenAI configurada
- [ ] Emulador Wear OS criado
- [ ] Build sem erros
- [ ] App roda no emulador
- [ ] Mensagem envia e recebe resposta

## 🚀 Próximos Passos

Após configurar:
1. Testvar com diferentes perguntas
2. Customizar cores/tema em `ChatScreen.kt`
3. Adicionar mais recursos (histórico, temas, etc)
4. Build APK para testar em dispositivo real
5. Publicar na Google Play Store Wear

## 📞 Suporte

Se tiver problemas:
1. Verifique os logs: `adb logcat`
2. Teste a chave OpenAI independentemente
3. Confirme conexão WiFi do emulador
4. Verifique versões do Android/Gradle
5. Abra issue no GitHub

---

**Pronto para usar Brasilizia Watch! 🦎🚀**
