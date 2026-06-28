# whisper.cpp Windows binary w/ Vulkan GPU support

Get `whisper.cpp-windows-vulkan.zip` from the [Releases page](https://github.com/jerryshell/whisper.cpp-windows-vulkan-bin/releases)

## Example: Integration to Subtitle Edit v4.x

Copy files to `%appdata%\Subtitle Edit\Whisper\Cpp`

You may need to create the directory manually

## Example: Download and Transcribe YouTube video

Bash command

### Download video

```shell
yt-dlp \
    -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best" \
    -o "video/1.mp4" \
    --no-simulate \
    --no-write-auto-subs \
    --restrict-filenames \
    --embed-thumbnail \
    --embed-chapters \
    --xattrs \
    "https://www.youtube.com/watch?v=UF8uR6Z6KLc";
```

### Extract audio and Resample

```shell
ffmpeg \
    -i "video/1.mp4" \
    -hide_banner \
    -vn \
    -loglevel error \
    -ar 16000 \
    -ac 1 \
    -c:a pcm_s16le \
    -y \
    "video/1.wav";
```

### Transcribe

https://huggingface.co/ggerganov/whisper.cpp

```shell
./whisper-cli \
    -m "model/ggml-large-v2-q5_0.bin" \
    -f "video/1.wav" \
    -sns \
    -osrt;
```

### Transcribe w/ VAD

https://huggingface.co/ggml-org/whisper-vad

```shell
./whisper-cli \
    -m "model/ggml-large-v2-q5_0.bin" \
    --vad \
    -vm "model/ggml-silero-v5.1.2.bin" \
    -f "video/1.wav" \
    -sns \
    -osrt;
```

## More usage

See [ggml-org/whisper.cpp](https://github.com/ggml-org/whisper.cpp)
