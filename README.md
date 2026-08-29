# audio2resolve

Convierte audios a WAV PCM para que **DaVinci Resolve en Linux** los importe sin dramas.

Resolve en Linux no trae los decodificadores de AAC, así que los `.m4a` y `.mp3`
que salen de un móvil o de Google Drive se importan mudos, con la duración mal
o directamente fallan. Este script los pasa a WAV PCM, que Resolve lee de forma
nativa: sin decodificación, scrubbing instantáneo y sincronía exacta.

## Requisitos

- `ffmpeg`
- `unzip` (solo si le pasas archivos `.zip`)

## Instalación

```bash
curl -o ~/.local/bin/audio2resolve \
  https://raw.githubusercontent.com/jorgeress/audio2resolve/main/audio2resolve
chmod +x ~/.local/bin/audio2resolve
```

Asegúrate de que `~/.local/bin` esté en tu `PATH`.

## Uso

```bash
audio2resolve grabacion.m4a          # un archivo
audio2resolve ~/Downloads/audios/    # todos los audios de una carpeta
audio2resolve descarga.zip           # descomprime y convierte lo que haya dentro
```

Sin `-o`, los WAV van a una subcarpeta `wav_resolve/` junto al origen.
Los originales no se tocan nunca.

## Opciones

| Opción | Qué hace |
|---|---|
| `-o DIR` | Carpeta de salida |
| `-b 16\|24\|32` | Profundidad de bits (por defecto 24) |
| `-r HZ` | Frecuencia de muestreo (por defecto, la del original) |
| `-s` | Fuerza estéreo (por defecto conserva los canales del original) |
| `-n` | Normaliza a -16 LUFS con `loudnorm`, útil para voz grabada a niveles dispares |
| `-f` | Rehace los WAV que ya existan (por defecto los omite) |
| `-h` | Ayuda |

Como por defecto omite lo ya convertido, puedes relanzarlo sobre la misma
carpeta cada vez que añadas material y solo procesará lo nuevo.

## Formatos de entrada

`m4a`, `mp3`, `aac`, `opus`, `ogg`, `oga`, `wma`, `flac`, `wav`, `aiff`, `aif`
y vídeos (`mp4`, `mov`, `mkv`, `webm`), de los que extrae la pista de audio.

## Por qué 24 bits

Es el estándar de post-producción y le da margen a la mezcla sin que se note en
el tamaño para pistas cortas. Si te importa el espacio, `-b 16` vale de sobra
para voz.

## Licencia

MIT
