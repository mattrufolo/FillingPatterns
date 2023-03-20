# Tool for the analysis of LHC filling schemes

**Authors:** G. Iadarola, A. Poyet, G. Sterbini

**Requirements:** Python 3.7, numpy, pandas

## To install:
```bash
git clone https://github.com/PyCOMPLETE/FillingPatterns.git
pip install ./FillingPatterns
```

## To run an example:
```bash
cd FillingPatterns/examples
python 001_from_csv_analyze_bb.py
```

## Usage

### Loading a filling scheme
The filling scheme can be loaded in different ways:
 * From a json file (as provided by the LPC filling scheme tool):
```python
import fillingpatterns as fp
patt = fp.FillingPattern.from_json('fname.json')
```
 * From a csv file (which can be generated by this tool):
```python
import fillingpatterns as fp
patt = fp.FillingPattern.from_csv('fname.csv')
```

 * By providing two boolean arrays with the scheme:
```python
import fillingpatterns as fp
patt = fp.FillingPattern(pattern_b1, pattern_b2)
```

### Analyzing the filling scheme
The filling scheme object has several attributes with characteristics of the filling scheme. For example:
```python
patt.b1.n_bunches
patt.n_coll_ATLAS
patt.n_coll_LHCb
patt.n_coll_ALICE
patt.b1.n_injections
patt.b1.n_unused_slots
patt.b1.inj_composition_types
patt.b1.inj_pattern_types
patt.b1.gap_lengths
patt.b1.agap_length
```
The example [000_json_to_csv_and_comparison.py](https://github.com/PyCOMPLETE/FillingPatterns/blob/master/examples/000_json_to_csv_and_comparison.py) illustrates their usage.

To compute the beam-beam schedules for the two beams:
```python
patt.compute_beam_beam_schedule(n_lr_per_side=16, use_old_algorithm = 1)
```

This attaches to the object two pandas dataframes with information on the beam-beam encounters
```python
patt.b1.bb_schedule
patt.b2.bb_schedule
```

To inspect the available information:
```python
patt.b1.bb_schedule.keys()
# Returns:
# Index(['HO partner in ALICE', '# of LR in ALICE', 'BB partners in ALICE',
#        'Positions in ALICE', 'HO partner in ATLAS/CMS', '# of LR in ATLAS/CMS',
#        'BB partners in ATLAS/CMS', 'Positions in ATLAS/CMS',
#        'HO partner in LHCB', '# of LR in LHCB', 'BB partners in LHCB',
#        'Positions in LHCB', 'collides in ATLAS/CMS', 'collides in ALICE',
#        'collides in LHCB'],
#       dtype='object')
```

To access one colum:
```python
patt.b1.bb_schedule[ 'BB partners in LHCB']
```

An example plotting some beam-beam properties is available at [001_from_csv_analyze_bb.py](https://github.com/PyCOMPLETE/FillingPatterns/blob/master/examples/001_from_csv_analyze_bb.py).

