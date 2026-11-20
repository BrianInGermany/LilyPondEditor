# Songbook — LilyPond Player

A single-page web app for writing, viewing and playing songs written in [LilyPond](https://lilypond.org) notation, right in the browser. Type or paste LilyPond, see the score engraved as you type, hear it played back, and turn any song into a printable QR code that carries the music itself.

Everything lives in one self-contained `index.html`: no build step, no server, no external libraries or fonts.

**Live version:** `https://<your-username>.github.io/<repo-name>/`

---

## Features

- **Live engraving.** The score updates as you type. It supports chords, ties, slurs, beams, dynamics, articulations, tuplets, grace notes, multiple voices and staves, lyrics, repeats and chord names.
- **Playback.** Dynamics, accents, staccato, fermatas, trills, repeats and chord symbols all affect playback. You can choose the sound (piano, strings, organ, flute or chiptune), adjust the tempo, loop, and **tap any note to play from there**.
- **Repertoire.** Save, rename and delete songs. Songs are kept in your browser.
- **Files.** Download a song as `.ly` (LilyPond source) or `.mid` (MIDI), or open a `.ly` file from your device.
- **MIDI QR codes.** Turn a song into a QR code that contains the whole song as a MIDI file, ready to print next to the score. See [QR codes](#qr-codes) below.

## Using it

1. Pick a song from the **Repertoire** menu, or start typing in the editor.
2. Press **▶ Play**, or tap any note in the score.
3. Name the song and press **Save** (or Ctrl/⌘+S). **Save as new** keeps a copy under a new name.
4. Press **QR** to get the song's MIDI QR code. Use **Save PNG** or long-press the image to save it.

### Where songs are saved

Songs are stored in your **browser's local storage**, on that device and in that browser only. Clearing your browsing data or using a private window removes them. **Use "Download .ly" to keep real copies of your songs** (for example, commit them to this repository) and **"Open file…"** to load them again.

## Supported LilyPond

The player understands a practical subset of LilyPond for lead sheets and simple scores:

| Area | Supported |
|---|---|
| Pitches | Dutch names (`fis`, `bes`, `es`), `'` / `,` octaves, `\relative`, `\fixed`, `\transpose`, `\language "english"` / `"deutsch"` / `"italiano"` |
| Rhythm | Durations 1–128 and `\breve`, dots, `*n/m`, `\tuplet`, `\times`, `\partial`, rests `r`, full-bar rests `R1*4`, spacers `s` |
| Chords | `<c e g>4`, chord repeat `q`, `\arpeggio` |
| Lines | Ties `~`, slurs `( )`, phrasing slurs `\( \)`, beams `[ ]` plus automatic beaming |
| Expression | `\pp`…`\fff`, `\sfz`, `\fp`, hairpins `\<` `\>` `\!`, articulations (`-.` `->` `--` `-^` `\fermata` `\trill` …), fingerings, text `^"…"` |
| Grace notes | `\grace`, `\acciaccatura`, `\appoggiatura`, `\afterGrace` |
| Structure | `\clef`, `\key`, `\time`, `\tempo`, `\bar`, `\mark`, `\break`, `\repeat volta` + `\alternative`, `\repeat unfold` |
| Voices & staves | `<< { } \\ { } >>`, `\new Voice`, `\new Staff`, `PianoStaff`, `GrandStaff`, `StaffGroup`, `ChoirStaff` |
| Lyrics | `\addlyrics`, `\new Lyrics \lyricsto "voice"`, `--`, `__`, `_`, multiple verses |
| Chord names | `\chordmode`, `\chords`, `\new ChordNames`: `c:m7`, `g:7`, `f:maj7`, `b:m7.5-`, `e:7.9-`, `d:sus4`, `c:dim7`, `c/e`, … |
| Files | Variables, `\score`, `\header` (title, composer…), comments. `\layout`, `\midi`, `\paper`, `\override` and `\set` are accepted and ignored |

**Not supported:** Scheme code, fret diagrams, drums, tablature, figured bass, cross-staff notes and ottava lines. Line breaks and spacing are the player's own and won't match LilyPond's output exactly.

### Example

```lilypond
\version "2.24.0"

harmonies = \chordmode { d1 | a | d }

melody = \fixed c' {
  \key d \major \time 4/4
  fis4 a fis d | e2 cis | d1 \bar "|."
}

\score {
  <<
    \new ChordNames \harmonies
    \new Staff \melody
  >>
}
```

## QR codes

Each QR code contains the complete song as a **standard MIDI file encoded as base64 text**. The song's title is stored inside the MIDI file as its track name. Nothing is fetched from the internet, so the code keeps working as long as the printed page exists.

**To listen to a code:**

1. Scan it with any QR scanner. It reads as a long line of text starting with `TVRoZ`.
2. Copy the text and paste it into any base64 decoder.
3. Save the result as `song.mid`. If the decoder doesn't name the file, name it yourself.
4. Open it in a MIDI player. Android usually plays MIDI files directly; on iPhone, use a MIDI player app such as NS MIDI Player.

**Printing tips:** print codes at least **4–5 cm** wide (larger for long songs), black on white, with a clear white margin around each code. Test a code end to end before printing a batch.

**Size guide:** a short song of about 20 seconds is roughly 500 bytes of MIDI, which makes a medium-density code. Written-out repeats make the file larger.

## Credits

- The treble clef glyph is taken from **GNU FreeFont (FreeSerif)**, licensed under the GPL with the font exception.
- Everything else is plain HTML, CSS and JavaScript written for this project: notation parsing, engraving, the Web Audio synthesizer, the MIDI writer and the QR encoder.

## License

*Add your license here, e.g. MIT for the code, and your own terms for the songs.*

