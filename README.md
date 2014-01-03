# Bebop: An Audio library for Julia

Bebop is a set of wrappers for the PortAudio and libsndfile libraries which
attempts to provide a nice, idiomatic interface for manipulating audio in Julia.

Currently, Bebop is able to read data from audio files into a Julia vector
and play audio samples from a Julia vector.

## Examples

### Play a single note for 2 seconds

```julia
using Bebop

const duration = 2
const sampRate = 44100
# the A above middle C
const musicFreq = 440.0

const t = [1:duration * sampRate] / sampRate
const samples = float32(sin(2 * pi * musicFreq * t))

letsjam() do
    play(samples)
end
```

### Read and play first five seconds of song

```julia
using Bebop

if length(ARGS) < 1
    error("missing argument")
end

const duration = 5

openFile(ARGS[1]) do file
    sampRate = file.sfinfo.samplerate
    channels = file.sfinfo.channels
    frames = readFrames(file, sampRate * duration)
    # explicitly specify sample rate, number of channels, and data format
    letsjam(sampRate, channels, Int16) do
        play(frames)
    end
end
```
