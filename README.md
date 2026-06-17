# midi

*Batteries-included Erlang MIDI.*

`midi` is the front door to the erlsci MIDI family. It wires realtime device I/O
([`midiio`](https://github.com/erlsci/midiio)) to message and file codecs
([`midilib`](https://github.com/erlsci/midilib)) behind one ergonomic API, so
the same `{midi, ...}` message terms flow through live playback *and* Standard
MIDI File reading/writing.

Outbound, you send message terms and `midi` encodes them to the wire for you;
inbound, raw bytes from a device arrive already decoded into terms. You depend
on one library and get the whole stack:

```
midi:send(Device, midimsg:note_on(...))   %% term ─► midibin:encode ─► midiio (out)
                                          %% midiio (in) ─► midibin:decode ─► {midi, ...}
```

## Where it sits

```
minimidio.h ─► midiio  (transport: raw bytes ⇄ OS ports)     midilib  (codec + .mid files, pure Erlang)
                            └────────────────┬────────────────────┘
                                          midi  (one batteries-included API)
```

| Library | Layer | Native build? |
|---------|-------|---------------|
| [midilib](https://github.com/erlsci/midilib) | message codec + Standard MIDI File read/write | no (pure Erlang) |
| [midiio](https://github.com/erlsci/midiio) | realtime device I/O (NIF over minimidio) | yes |
| **midi** | the whole enchilada: codec + I/O behind one API | yes (via midiio) |

Need just file/codec work with no native build? Depend on `midilib` alone.
Need raw realtime bytes and your own representation? Depend on `midiio` alone.
Want everything? Depend on `midi`.

## Status

Early placeholder — `0.0.1`. The unified API is under active development and
will change. Published to Hex to reserve the name.

## Build

    $ rebar3 compile
