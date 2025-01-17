Read Becker & Hickl SDT files
=============================

Sdtfile is a Python library to read SDT files produced by Becker & Hickl
SPCM software. SDT files contain time correlated single photon counting
instrumentation parameters and measurement data. Currently only the
"Setup & Data", "DLL Data", and "FCS Data" formats are supported.

`Becker & Hickl GmbH <http://www.becker-hickl.de/>`_ is a manufacturer of
equipment for photon counting.

:Author: `Christoph Gohlke <https://www.cgohlke.com>`_
:License: BSD 3-Clause
:Version: 2022.9.28

Requirements
------------

This release has been tested with the following requirements and dependencies
(other versions may work):

- `CPython 3.8.10, 3.9.13, 3.10.7, 3.11.0rc2 <https://www.python.org>`_
- `Numpy 1.22.4 <https://pypi.org/project/numpy/>`_

Revisions
---------

2022.9.28

- Convert docstrings to Google style with Sphinx directives.

2022.2.2

- Add type hints.
- Drop support for Python 3.7 and numpy < 1.19 (NEP29).

2021.11.18

- Fix reading FLIM files created by Prairie View software (#5).

2021.3.21

- Add sdt2dat script.

2020.12.10

- Fix shape of non-square frames.

2020.8.3

- Fix integer overflow (#3).
- Support os.PathLike file names.

2020.1.1

- Fix reading MCS_BLOCK data.
- Remove support for Python 2.7 and 3.5.
- Update copyright.

2019.7.28

- Fix reading compressed, multi-channel data.

2018.9.22

- Use str, not bytes for ASCII data.

2018.8.29

- Move module into sdtfile package.

2018.2.7

- Bug fixes.

2016.3.30

- Support revision 15 files and compression.

2015.1.29

- Read SPC DLL data files.

2014.9.5

- Fix reading multiple MEASURE_INFO records.

References
----------

1. W Becker. The bh TCSPC Handbook. Third Edition. Becker & Hickl GmbH 2008.
   pp 401.
2. SPC_data_file_structure.h header file. Part of the Becker & Hickl
   SPCM software.

Examples
--------

Read image and metadata from a "SPC Setup & Data File":

>>> sdt = SdtFile('image.sdt')
>>> sdt.header.revision
588
>>> sdt.info.id[1:-1]
'SPC Setup & Data File'
>>> int(sdt.measure_info[0].scan_x)
128
>>> len(sdt.data)
1
>>> sdt.data[0].shape
(128, 128, 256)
>>> sdt.times[0].shape
(256,)

Read data and metadata from a "SPC Setup & Data File" with mutliple data sets:

>>> sdt = SdtFile('fluorescein.sdt')
>>> len(sdt.data)
4
>>> sdt.data[3].shape
(1, 1024)
>>> sdt.times[3].shape
(1024,)

Read image data from a "SPC FCS Data File" as numpy array:

>>> sdt = SdtFile('fcs.sdt')
>>> sdt.info.id[1:-1]
'SPC FCS Data File'
>>> len(sdt.data)
1
>>> sdt.data[0].shape
(512, 512, 256)
>>> sdt.times[0].shape
(256,)
