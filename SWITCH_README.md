# Pekka Kana 2 – port para Nintendo Switch (homebrew)

Baseado no código de danilolc/pk2 (SDL2). Alterações para o Switch:
- `src/version.hpp`: plataforma "(Switch)".
- `src/engine/PRender.cpp`: no Switch usa o renderizador SDL (OpenGL ES via Mesa), tela cheia, sem o renderizador OpenGL desktop.
- `src/engine/PUtils.cpp`: pasta do jogo fixa em `sdmc:/switch/pk2/res`.
- `Makefile.switch`: build com devkitPro/libnx, Lua 5.4 embutido (external/lua).

## Compilar (opção 1 – no seu PC com Windows)
1. Instale o devkitPro (instalador gráfico: https://github.com/devkitPro/installer/releases), marcando **Switch Development**.
2. Abra o terminal **MSYS2** que vem com ele e rode:
   ```
   pacman -S --needed switch-dev switch-sdl2 switch-sdl2_image switch-sdl2_mixer
   cd /c/Users/marlo/Downloads/pk2-switch
   make -f Makefile.switch sd -j8
   ```
3. O resultado fica em `switch/sd/`.

## Compilar (opção 2 – GitHub, sem instalar nada)
Suba esta pasta para um repositório seu no GitHub. A Action `.github/workflows/switch.yml`
compila sozinha; baixe o artefato **pk2-switch-sd** na aba *Actions*.

## Instalar no Switch
Copie o conteúdo de `switch/sd/` para a raiz do cartão SD. Vai ficar:
```
sdmc:/switch/pk2/pk2.nro
sdmc:/switch/pk2/res/...
```
Abra pelo Homebrew Menu, de preferência em modo título (segure **R** ao abrir um jogo).
Log em `sdmc:/switch/pk2/res/data/log.txt`.
