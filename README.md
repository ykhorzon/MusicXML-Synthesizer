# MusicXML-Synthesizer

This program is still under development and test.

A program can convert solola results to musicXML(.musicxml) format

[Solola results] => MusicXML-Synthesizer => [musicxml format file]

Note: We will set [MuseScore](https://github.com/musescore/MuseScore) and [Tux Guitar](http://tuxguitar.com.ar/) as validation editor. It means that MusicXML-Synthesizer will ensure both of editor can successfully open output.

## Requirements

- [pipenv](https://github.com/pypa/pipenv)
- other dependencies check out Pipfile

## Usage

```shell
# basic 
python cli.py [-s SOLOLA_PATH] [-db DOWNBEAT_PATH] [-b BEAT_PATH]
              [-o OUTPUT_PATH] [-x EXECTUE_VALIDATION]

python cli.py -s /e/workplace/projects/solola/MusicXML-Synthesizer/input_mock/bend/FinalNotes.txt -db /e/workplace/projects/solola/MusicXML-Synthesizer/input_mock/bend/downbeats.txt -b /e/workplace/projects/solola/MusicXML-Synthesizer/input_mock/bend/beats.txt -o outputs/test/test.musicxml

# detail
python cli.py -h
```

```python
# example
solola_list = parse_notes_meta_to_list(
      solola_path)
beats_list = parse_notes_meta_to_list(
      beat_path)
downbeats_list = parse_notes_meta_to_list(
      downbeat_path)

output_path = './output.mzxml'

# setup
synthesizer = Synthesizer()
synthesizer.save(solola_list, downbeats_list, beats_list)

# synthesize musicXML
xml = synthesizer.execute(output_path)

# create folder and write file to file system
write_file(output_path, xml)
```
## Test

We use pytest and pytest-watch. Use ptw command in director root 
<pre>
# pytest show verbose & print
pytest -s -v

# pytest watch
ptw . -- -vvv

# coverage
python -m pytest tests -s -v --cov=./

# run single test case
ptw tests/test_unit_Synthesizer.py  -- -k 'test_Synthesizer_annotate_rest_and_tech
nique' -vvv --cov=./
</pre>

# Appendix

## SoloLa output format (Input)

Notation: 
<pre>
(0)    (1)   (2)   (3)   (4)   (5)   (6)   (7)   (8)   (9)  (10)  (11) # index
Pit     On   Dur  PreB     B     R     P     H     S    SI    SO     V # attribute abbr. name
</pre>

Example:      
<pre>
[    66   1.24   0.5     2     0     0     0     0     1     2     1     1]
</pre>

value
  
- Pit:    pitch (MIDI number)
- On:     onset (sec.)
- Dur:    duration (sec.)
- PreB:   pre-bend 

      - 0 for none,
      - 1 for bend by 1 semitone,
      - 2 for bend by 2 semitone,
      - 3 for bend by 3 semitone

- B:      string bend 

      - 0 for none,
      - 1 for bend by 1 semitone,
      - 2 for bend by 2 semitone,
      - 3 for bend by 3 semitone

- R:      release  

      - 0: none, 
      - 1: release by 1 semitone,
      - 2: release by 2 semitone,
      - 3: release by 3 semitone

- P:      pull-off 

      - 0: none, 
      - 1: pull-off start,
      - 2: pull-off stop

- H:      hammer-on 

      - 0: none,
      - 1: hammer-on start,
      - 2: hammer-on stop

- S:      legato slide 

      - 0: none,
      - 1: legato slide start, 
      - 2: legato slide stop
              
- SI:     slide in 

      - 0: none,
      - 1: slide in from below,
      - 2: slide in from above

- SO:     slide out 

      - 0: none,
      - 1: slide out downward,

- V: 
      - 0: none
      - 1 for vibrato: vibrato with extent smaller or equal to 1 semitone,
      - 2 for wild vibrato: vibrato with extent larger than 1 semitone

## musicXMl (Output)
[musicXML example](https://www.musicxml.com/publications/makemusic-recordare/notation-and-analysis/a-sample-musicxml-encoding/)
