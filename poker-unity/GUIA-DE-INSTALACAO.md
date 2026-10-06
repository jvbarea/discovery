# Guia de instalação

Prepara o notebook e o Galaxy S24+ para desenvolver o Golden Spur Hold'em em Unity com o Claude Code.
Ao final, o Claude consegue abrir o editor, montar cenas, gerar o APK e instalar no celular.

- **Tempo:** de 1 a 2 horas, a maior parte esperando download.
- **Espaço:** reserve 30 GB no disco C: (o Unity com o módulo Android ocupa cerca de 12 GB; o projeto cresce com os assets).
- **Já instalado neste notebook:** Android Studio, Android SDK e `adb` (do trabalho com Flutter). O Unity traz o próprio SDK Android; não precisa mexer no que existe.

## 1. Contas

- [ ] **Unity ID** em [id.unity.com](https://id.unity.com). Serve para o Unity Hub, a Asset Store e o login do Unity CLI.
- [ ] **Adobe ID**, só quando for baixar personagens e animações do Mixamo. Pode ficar para depois.

A licença **Unity Personal** é gratuita para quem fatura até US$ 200 mil por ano.

## 2. Unity Hub e Unity CLI

1. Baixe o Unity Hub em [unity.com/download](https://unity.com/download) e instale.
2. Abra o Hub, entre com o Unity ID e aceite a licença Personal quando ele pedir.
3. O Hub instala junto o **Unity CLI**, a ferramenta de linha de comando que o Claude usa para controlar o editor. Abra um PowerShell **novo** e confira:

   ```powershell
   unity --version
   ```

   Se aparecer "unity não é reconhecido", instale o CLI separado e abra outro PowerShell:

   ```powershell
   winget install Unity.CLI
   ```

4. Entre no CLI com o mesmo Unity ID:

   ```powershell
   unity auth login
   ```

## 3. Editor Unity 6.3 LTS com módulo Android

Use a versão **6.3 LTS**, a de suporte longo recomendada para projetos novos (suporte até dezembro de 2027).

1. No Hub: **Installs → Install Editor → Unity 6.3 LTS**.
2. Na lista de módulos, marque:
   - **Android Build Support**, com os dois itens de dentro: **OpenJDK** e **Android SDK & NDK Tools**.
   - Se aparecer, desmarque o **Visual Studio**: vamos usar o VS Code.
3. Aceite as licenças do Android SDK e espere terminar.

Alternativa pelo terminal, com o mesmo resultado:

```powershell
unity install lts -m android
```

## 4. Criar o projeto fora do OneDrive

O projeto **não** fica nesta pasta nem em nada que o OneDrive sincronize. O Unity mantém milhares de arquivos temporários na pasta `Library`, e o OneDrive trava esses arquivos, deixa tudo lento e às vezes corrompe o projeto. Além disso, assets comprados não podem ir para um repositório público.

1. Crie a pasta `C:\dev` (se não existir).
2. No Hub: **Projects → New project**.
   - **Editor version:** 6000.3.x LTS (é o 6.3 LTS)
   - **Template:** **Universal 3D** (usa o URP, o pipeline de renderização leve que roda bem no celular). Não use o "Universal 3D sample", que traz uma cena de exemplo pesada.
   - **Unity organization:** deixe a sua organização pessoal, que já vem selecionada.
   - **Project name:** `golden-spur`
   - **Location:** `C:\dev`. O Hub cria `C:\dev\golden-spur`.
   - **Use Unity CLI:** **marque**. Instala o pacote Pipeline (`com.unity.pipeline`), que deixa o Claude controlar o editor.
   - **Use AI Assistant:** deixe desmarcado. É a IA do próprio Unity; os geradores de textura e animação dela podem entrar depois pelo Package Manager.
   - **Source control provider:** deixe vazio. O versionamento com Git vai ser tratado no guia de uso.
3. Clique **Create project** e espere o editor abrir. A primeira abertura demora.

## 5. Preparar o S24+

1. **Modo desenvolvedor:** Configurações → Sobre o telefone → Informações do software → toque 7 vezes em **Número de compilação** e digite o PIN.
2. **Depuração USB:** Configurações → **Opções do desenvolvedor** (aparece no fim da lista) → ligue **Depuração USB**.
3. Opcional, ajuda nos testes longos: nas mesmas opções, ligue **Permanecer ativo** para a tela não apagar enquanto carrega.
4. Ligue o celular no notebook com um cabo USB-C **de dados** (alguns cabos só carregam).
5. No celular, aceite **Permitir depuração USB?** e marque **Sempre permitir deste computador**.
6. Confira no PowerShell:

   ```powershell
   & "$env:LOCALAPPDATA\Android\Sdk\platform-tools\adb.exe" devices
   ```

   O esperado é uma linha com um número de série e a palavra `device`.

## 6. Primeiro build no celular

Serve para provar que a cadeia inteira funciona antes de o Claude entrar.

1. No editor: **File → Build Profiles → Android → Switch Platform**. Espere reimportar.
2. Em **Edit → Project Settings → Player → Android → Other Settings**, confira:
   - **Scripting Backend:** IL2CPP
   - **Target Architectures:** só **ARM64**
   - **Graphics APIs:** **Vulkan** em primeiro lugar
3. De volta em **Build Profiles**, escolha o S24+ em **Run Device** e clique **Build And Run**. Salve o APK em `C:\dev\golden-spur\Builds`.
4. O primeiro build demora de 5 a 15 minutos neste notebook. No fim, a cena de exemplo abre sozinha no celular.
5. Depois pode fechar o app: ele fica instalado, e cada **Build And Run** substitui a versão anterior. O primeiro build usa o nome de pacote do template (`com.UnityTechnologies.com.unity.template.urpblank`); o Claude troca para `com.jvbarea.goldenspur` no começo do projeto.

## 7. Claude Code com o plugin oficial do Unity

1. Abra o VS Code na pasta `C:\dev\golden-spur` e inicie o Claude Code.
2. Instale o plugin oficial do Unity (escolha o escopo **user**, para valer em todos os projetos):

   ```text
   /plugin marketplace add Unity-Technologies/unity-agent-plugin
   /plugin install unity@unity-agent-plugin
   ```

3. Confira: `/plugin` deve listar `unity` como ativado, e `/unity:` mostra as habilidades do plugin.
4. Com o **editor aberto no projeto**, confira o pacote que liga o CLI ao editor. Se você marcou **Use Unity CLI** ao criar o projeto, ele já está instalado; senão, instale no PowerShell, dentro de `C:\dev\golden-spur`, e espere o editor recompilar:

   ```powershell
   unity pipeline install
   ```

   Confira:

   ```powershell
   unity pipeline list
   ```

   O esperado é uma linha com o projeto `golden-spur` e **true** em `Running`, `Pipeline` e `Server Reachable`. A porta pode ser 7800 ou 7801.

5. Teste de fogo: peça ao Claude "crie um cubo em (0, 1, 0) na cena aberta". O cubo deve aparecer no editor.

## 8. Opcionais

- **Extensão Unity para VS Code** (Microsoft) com o C# Dev Kit, para você ler o código com realce. Depois, em **Edit → Preferences → External Tools**, escolha o VS Code como editor de scripts.
- **Blender 5.2 LTS** ([blender.org](https://www.blender.org)), para modelar e otimizar por script (D15). O Claude
  instala sozinho a versão **portátil**: baixa o `.zip` oficial, confere o SHA-256 com a lista do site e descompacta em
  `C:\dev\tools\blender-5.2.2-windows-x64`. Nada é instalado no Windows (sem administrador nem menu Iniciar); para
  abrir com janela, dois cliques no `blender.exe` dessa pasta; para remover, apagar a pasta.
- **Repositório privado** no GitHub para o projeto Unity, com Git LFS para arquivos grandes. O guia de uso vai tratar disso.

## 9. O que me mandar ao terminar

Cole no chat:

- [ ] a saída de `unity --version`
- [ ] a versão do editor instalada (aparece no Hub, em Installs)
- [ ] a saída de `adb devices`
- [ ] se a cena de exemplo abriu no S24+
- [ ] a saída de `unity pipeline list`

## Problemas comuns

| Sintoma | O que fazer |
| --- | --- |
| `unity` não é reconhecido | Feche e abra o PowerShell. No terminal do VS Code, feche e abra o VS Code inteiro: ele guarda o PATH de quando foi aberto. O CLI fica em `%LOCALAPPDATA%\Unity\bin\unity.exe`. Se não existir, `winget install Unity.CLI`. |
| Comando no editor falha com "Main thread operation timed out" | O editor estava ocupado (importando, compilando) ou com uma janela esperando clique. Espere terminar, feche a janela e tente de novo. |
| `adb devices` mostra `unauthorized` | Desbloqueie o celular e aceite o pedido de depuração USB. |
| `adb devices` não mostra nada | Troque o cabo, escolha **Transferência de arquivos** na notificação USB do celular ou instale o [driver USB da Samsung](https://developer.samsung.com/android-usb-driver). |
| Aviso de versão do `adb` diferente | Há dois `adb` (Android Studio e Unity). Feche o Android Studio e rode `adb kill-server`; depois tente de novo. |
| Build reclama de Android SDK | Em **Edit → Preferences → External Tools**, marque as opções "installed with Unity" do JDK, SDK e NDK. |
| Build falha no fim com "Burst compiler failed" e "An Application Control policy has blocked this file" | É o **Controle Inteligente de Aplicativos** do Windows 11 bloqueando o compilador Burst do Unity, que não tem assinatura digital (aconteceu neste notebook em 29/09/2026). Saída rápida: em **Edit → Project Settings → Burst AOT Settings**, desmarque **Enable Burst Compilation** para Android; o build passa, mas alguns trechos do Unity rodam mais devagar. Saída definitiva: desligar o Controle Inteligente em **Segurança do Windows → Controle de aplicativos e do navegador**. Atenção: depois de desligado, ele só volta reinstalando ou restaurando o Windows; o antivírus continua protegendo. |
| Notebook muito quente ou lento | Deixe na tomada, feche outros programas e use uma base com ventilação. O primeiro build é o mais pesado. |
| Projeto criado dentro do OneDrive | Feche o Unity, mova a pasta para `C:\dev` e abra de novo pelo Hub (**Add project from disk**). |
