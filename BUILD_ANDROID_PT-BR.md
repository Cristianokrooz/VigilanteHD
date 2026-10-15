# Como gerar o APK de desenvolvimento

## Estado atual

O código-fonte Android está preparado, mas **este pacote ainda não contém um APK compilado**. O ambiente onde foi preparado não tem Gradle/Android SDK/NDK e não consegue resolver nomes de hosts para baixar dependências. O workflow de CI foi incluído em `.github/workflows/android-apk.yml` para executar o build em um runner Ubuntu com acesso à internet.

## Compilação automática em runner com internet

Depois de colocar este projeto em um repositório GitHub:

1. Abra a aba **Actions** do repositório.
2. Escolha **Build Vigilante 8 Android APK** e pressione **Run workflow** (ou envie uma alteração em `android/` ou `game-source/`).
3. Aguarde o job `build-debug-apk` concluir.
4. Na página da execução, baixe o artefato `vigilante-8-android-debug-apk`. Ele contém `app-debug.apk`.

O workflow instala JDK 17, Gradle 8.9, Android SDK Platform 35, NDK `27.2.12479018` e CMake 3.22.1. O build ainda poderá revelar erros de código nativo que só podem ser diagnosticados quando o runner realmente executar o NDK.

## Compilar localmente

Requisitos:

- JDK 17.
- Gradle 8.9.
- Android SDK Platform 35 e Build-Tools 35.0.0.
- Android NDK `27.2.12479018`.
- CMake 3.22.1.
- Internet para baixar SDL2 2.30.11 dos servidores oficiais durante o build.

Na pasta `android/`, execute:

```sh
gradle --no-daemon :app:assembleDebug
```

O resultado esperado é `android/app/build/outputs/apk/debug/app-debug.apk`. Se o build for concluído, instale-o no aparelho e teste a inicialização antes de considerar o port funcional.

## Primeiro uso

1. Instale o APK de debug em aparelho com suporte a OpenGL ES 3.0.
2. Abra o aplicativo e toque em **Importar pasta dos dados do jogo**.
3. Selecione pelo seletor de pastas do Android a pasta com `QUEST.BIN`, `TERRAIN/`, `SHELL/` e os demais dados de uma cópia legítima do jogo.
4. Aguarde a cópia para o armazenamento privado do aplicativo.

Os dados proprietários do jogo não estão incluídos neste projeto.

## Limites conhecidos

- O teste nativo Linux passou, mas isso não valida o build Android.
- O workflow de CI foi preparado, mas **não foi executado a partir desta sessão**; nenhum APK é fornecido como se tivesse sido compilado.
- A renderização GLES, a importação e os controles táteis ainda precisam ser testados em aparelho real.
- A melhoria de texturas continua separada; Real-ESRGAN não foi executado e os PNGs de fallback não estão integrados automaticamente ao carregador do jogo.
