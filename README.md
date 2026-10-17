# Silver Pines 60fps

60fps patch (`.ips`) for **Silver Pines** on Nintendo Switch (Atmosphère).

🇧🇷 [Português](#português) · 🇺🇸 [English](#english)

| | |
|---|---|
| Game version | Base version (no update installed) |
| Build ID | `91046036F8850DE3EC306056FCC6BA12` |
| Engine | Unity 6000.3.15f1 (IL2CPP) |
| Tested on | Real hardware with Atmosphère |

---

## Português

Patch que destrava o **Silver Pines** de 30fps para **60fps** no Switch.

### Instalação
1. Baixe o `.zip` na aba [Releases](../../releases), ou baixe este repositório.
2. Copie a pasta `atmosphere` para a raiz do cartão SD, juntando com a que já existe:
   ```
   atmosphere/exefs_patches/Silver Pines 60fps/91046036F8850DE3EC306056FCC6BA1200000000000000000000000000000000.ips
   ```
3. Abra o jogo. **Não precisa do FPSLocker.**

Usa o IPSwitch? O arquivo `.pchtxt` está na pasta [`pchtxt/`](pchtxt).

### Observações
- Funciona **apenas na versão base** do jogo (Build ID acima). Se o jogo receber uma atualização, o patch simplesmente deixa de ser aplicado até ser refeito para a nova versão.
- Em áreas pesadas pode haver quedas de FPS; usar overclock (sys-clk) ajuda.
- Para remover, apague a pasta `atmosphere/exefs_patches/Silver Pines 60fps`.
- Este repositório **não contém nenhum arquivo do jogo**. Você precisa ter sua própria cópia.

---

## English

Patch that unlocks **Silver Pines** from 30fps to **60fps** on Switch.

### Installation
1. Download the `.zip` from [Releases](../../releases), or download this repository.
2. Copy the `atmosphere` folder to the root of your SD card, merging with the existing one:
   ```
   atmosphere/exefs_patches/Silver Pines 60fps/91046036F8850DE3EC306056FCC6BA1200000000000000000000000000000000.ips
   ```
3. Launch the game. **FPSLocker is not needed.**

Using IPSwitch? The `.pchtxt` file is in [`pchtxt/`](pchtxt).

### Notes
- Works **only on the base version** of the game (Build ID above). If the game gets an update, the patch simply won't apply until it is remade for the new version.
- Heavy areas may drop frames; overclocking (sys-clk) helps.
- To remove it, delete `atmosphere/exefs_patches/Silver Pines 60fps`.
- This repository **contains no game files**. You need your own copy.

---

## Technical details

The game is capped at 30fps by two separate mechanisms, which is why FPSLocker alone has no effect:

1. **`Application.targetFrameRate = 30`**, set by the game's scripts (two call sites).
2. **Present interval 2** (vsync every 2nd frame), coming from the project's `QualitySettings.vSyncCount`, stored in an engine global and fed to `nvnWindowBuilderSetPresentInterval`.

Changed instructions (offsets relative to the start of `.text`, `offset_shift 0x100`):

| Offset | Original | Patched | Purpose |
|---|---|---|---|
| `01B8F9A0` | `mov w0, #30` | `mov w0, #60` | `targetFrameRate` (game init) |
| `0320BD90` | `mov w0, #30` | `mov w0, #60` | `targetFrameRate` (2nd call site) |
| `00B6F1C8` | `ldr w8, [x8, #0x88]` | `mov w8, #1` | present interval read from vSyncCount |
| `00B7770C` | `csinc w1, w1, wzr, hi` | `mov w1, #1` | vsync interval setter |
| `00B882B0` | `ldr w1, [x8]` | `mov w1, #1` | `nvnWindowBuilderSetPresentInterval` argument |

## Credits
- Method based on the tutorial [How to make 60FPS .ips Patch for Nintendo Switch Game (Ghidra Tutorial)](https://gbatemp.net/threads/how-to-make-60fps-ips-patch-for-nintendo-switch-game-ghidra-tutorial.625675/) by ChanseyIsTheBest on GBAtemp.
- Made with the help of Claude.
