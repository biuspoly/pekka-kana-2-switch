# Pekka Kana 2 para Nintendo Switch (homebrew)

Port não oficial e nativo de **Pekka Kana 2** (Pekka the Rooster 2) para o Nintendo Switch com
custom firmware. Ele roda direto no console, sem emulador, compilado para o processador do
Switch a partir do código aberto do jogo.

> O jogo é gratuito e de código aberto (licença MIT). Este repositório é um fork do
> [danilolc/pk2](https://github.com/danilolc/pk2) com as mudanças necessárias para o Switch.
> O README original do projeto está em [README_ORIGINAL.md](README_ORIGINAL.md).

## Instalação no Switch

**Requisitos:** um Switch com custom firmware (por exemplo [Atmosphère](https://github.com/Atmosphere-NX/Atmosphere))
e acesso ao menu de homebrew (hbmenu).

1. Baixe o [`PekkaKana2_Switch_SD.zip`](https://github.com/biuspoly/pekka-kana-2-switch/releases/latest/download/PekkaKana2_Switch_SD.zip)
   da página de [Releases](https://github.com/biuspoly/pekka-kana-2-switch/releases).
2. Extraia o conteúdo na **raiz do cartão SD**. A estrutura deve ficar assim:
   ```
   SD:/switch/pk2/pk2.nro
   SD:/switch/pk2/res/        (gráficos, sons, músicas e fases)
   ```
   Para copiar sem tirar o cartão, você pode usar o modo MTP do DBI ou outro gerenciador de arquivos.
3. Abra o menu de homebrew **segurando R enquanto abre qualquer jogo** (o modo pelo Álbum
   funciona, mas tem menos memória).
4. Escolha **Pekka Kana 2**.

O idioma é o mesmo do console. Com o Switch em português, o jogo abre em português do Brasil.
Saves e configurações ficam em `SD:/switch/pk2/res/data/`.

### Controles
O jogo usa os Joy-Cons e o Pro Controller como controle (SDL GameController). Os botões podem
ser alterados no menu de opções do próprio jogo.

### Se algo der errado
- `SD:/switch/pk2/pk2_log.txt`: registro de tudo o que o jogo fez ao iniciar.
- `SD:/switch/pk2/pk2_crash.txt`: criado só se o jogo travar, com o endereço do erro.

Abra uma issue com esses dois arquivos.

## Como o port foi feito

O Pekka Kana 2 já usa SDL2, que tem versão para o Switch. O trabalho foi adaptar o que dependia
de PC ou Android:

| Arquivo | Mudança |
|---|---|
| `src/engine/PRender.cpp` | No Switch usa o renderizador SDL (OpenGL ES via Mesa), sempre em tela cheia, em vez do OpenGL de PC. |
| `src/engine/PUtils.cpp` | Pasta de dados no cartão SD. O idioma vem do sistema via libnx (`setGetSystemLanguage`), porque o SDL do Switch não informa o idioma e o jogo travava ao iniciar. |
| `src/main.cpp` | Os dados são procurados ao lado do `.nro`. Foram adicionados um log de diagnóstico e um tratador de crash (`__libnx_exception_handler`). |
| `src/version.hpp` | Identificação da plataforma "(Switch)". |
| `Makefile.switch` | Compilação com devkitA64 + libnx + SDL2/SDL2_image/SDL2_mixer, incluindo o Lua embutido, e geração do `.nro` com ícone e metadados. |
| `switch/icon.jpg` | Ícone novo, feito com a arte do próprio jogo. |

### Compilar você mesmo
Com o [devkitPro](https://devkitpro.org/wiki/Getting_Started) instalado:
```sh
dkp-pacman -S switch-dev switch-sdl2 switch-sdl2_image switch-sdl2_mixer
make -f Makefile.switch sd     # gera switch/sd/switch/pk2/ pronto para o cartão SD
```

## Agradecimentos

- **Janne Kivilahti / Piste Gamez**, criador do Pekka Kana 2, que liberou o jogo e o código sob a licença MIT.
- **[danilolc](https://github.com/danilolc/pk2)** e todos os colaboradores do projeto, que fizeram
  a versão em SDL2 (Definitive Edition) que tornou este port possível.
- **[devkitPro](https://devkitpro.org)** (devkitA64, libnx, portlibs) e a comunidade **[Switchbrew](https://switchbrew.org)**.
- **[SDL](https://www.libsdl.org)**, SDL_image, SDL_mixer e as bibliotecas que eles usam (libpng, libjpeg-turbo,
  libwebp, zlib, libogg, libvorbis, libmodplug, mpg123, opus, FLAC, Mesa).
- **[Lua](https://www.lua.org)** (PUC-Rio).
- **[Atmosphère](https://github.com/Atmosphere-NX/Atmosphere)** e as ferramentas de homebrew (hbmenu, nx-hbloader).
- Tradutores da comunidade, incluindo os autores da tradução em português do Brasil que vem com o jogo.

Port para o Switch: **[biuspoly](https://github.com/biuspoly)**, com ajuda do Claude (Anthropic).

## Licença

MIT, igual ao projeto original. Veja [licence.txt](licence.txt). Este projeto não tem ligação com a Nintendo.
