# PDF2ARCHIVE
A simple `Ghostscript` or `Libreoffice` based PDF to PDF/A-3B converter and other tools.

## Requirements
+ A `Bash`-compatible shell
+ A recent version of `Ghostscript` or `Libreoffice`
+ A recent version of `Exiftool`
+ Java if you need validation (`veraPDF` is included)

## Installation
Clone this repository.

## Scripts included
All scripts are supposed to be run via `parallel`, so many options may be omitted if script is run standalone.

### pdf2archive
This script converts selected PDF into PDF/A-3b format. Supports validation via `veraPDF`, metadata edition via `exiftool` and watermark addition. Creates output path automatically if it doesn't exist. This script uses `/dev/shm` as tmp file path.
```
  ./pdf2archive [options] input.pdf [output.pdf]
  --quality=<value>     Set  the  quality of  the  output  when  downsampling:
                        high -> -dPDFSETTINGS=/printer
                        medium -> -dPDFSETTINGS=/ebook
                        low -> -dPDFSETTINGS=/screen
                        by default -dPDFSETTINGS is not set
  --title=<value>       Title of the resulting PDF/A file
  --author=<value>      Author of the resulting PDF/A file
  --subject=<value>     Subject of the resulting PDF/A file
  --keywords=<value>    Comma-separated keywords of the resulting PDF/A file
  --creator=<value>     PDF/A creator program name
  --mark=<value>        Adds watermark in topright corner
  --cpolicy=<value>     Ghostscript policy (see -dPDFACompatibilityPolicy)
  --good=<filename>     Saves all valid filenames into this logfile (proceeded and validated)
  --bad=<filename>      Saves all invalid filenames into this logfile (proceeded but failed to validate)
  --pass=<filename>     Saves all skipped filenames into this logfile (not proceeded or just missing)
  --cleanmetadata       Clean  all the standard  metadata  fields
  --validate=<veraPDF>  Path to veraPDF executable (if none validation is disabled)
  --debug               Write additional debug information on screen
  -v, --version         Show the program version
```

### pdf2libre
This is `Libreoffice` version of `pdf2archive` script. It doesn't support `--mark` and `--cpolicy`. It also uses `/dev/shm` as tmp file path. All other options are the same as `pdf2archive`.
```
  ./pdf2libre [options] input.pdf [output.pdf]
  --title=<value>       Title of the resulting PDF/A file
  --author=<value>      Author of the resulting PDF/A file
  --subject=<value>     Subject of the resulting PDF/A file
  --keywords=<value>    Comma-separated keywords of the resulting PDF/A file
  --creator=<value>     PDF/A creator program name
  --good=<filename>     Saves all valid filenames into this logfile (proceeded and validated)
  --bad=<filename>      Saves all invalid filenames into this logfile (proceeded but failed to validate)
  --pass=<filename>     Saves all skipped filenames into this logfile (not proceeded or just missing)
  --cleanmetadata       Clean  all the standard  metadata  fields
  --validate=<veraPDF>  Path to veraPDF executable (if noone validation is disabled)
  --debug               Write additional debug information on screen
  -v, --version         Show the program version
```

### pdf2mark
This script adds watermark in topright corner to selected PDF file, also it supports validation after it's done.
```
  ./pdf2archive [options] input.pdf [output.pdf]
  --mark=<value>        Adds watermark in topright corner
  --good=<filename>     Saves all valid filenames into this logfile (proceeded and validated)
  --bad=<filename>      Saves all invalid filenames into this logfile (proceeded but failed to validate)
  --pass=<filename>     Saves all skipped filenames into this logfile (not proceeded or just missing)
  --validate=<veraPDF>  Path to veraPDF executable (if none validation is disabled)
  --debug               Write additional debug information on screen
  -v, --version         Show the program version
```

### pdf2meta
This script alters selected PDF metadata. Also supports validation after it's done. It modify the original file.
```
  ./pdf2meta [options] inout.pdf
  --title=<value>       Title of the resulting PDF/A file
  --author=<value>      Author of the resulting PDF/A file
  --subject=<value>     Subject of the resulting PDF/A file
  --keywords=<value>    Comma-separated keywords of the resulting PDF/A file
  --creator=<value>     PDF/A creator program name
  --good=<filename>     Saves all valid filenames into this logfile (proceeded and validated)
  --bad=<filename>      Saves all invalid filenames into this logfile (proceeded but failed to validate)
  --pass=<filename>     Saves all skipped filenames into this logfile (not proceeded or just missing)
  --cleanmetadata       Clean  all the standard  metadata  fields
  --validate=<veraPDF>  Path to veraPDF executable (if none validation is disabled)
  --debug               Write additional debug information on screen
  -v, --version         Show the program version
```

## Parallel runners
To enable running described scripts parallel there are a few example scripts using `parallel` program. There are no options for this scripts, so you need to modify them if you need.
+ run2archive -- runs pdf2archive parallel
+ run2mark -- runs pdf2mark parallel
+ run2meta -- runs pdf2meta parallel

By default described scripts uses files:
+ lista.csv -- as data source, separated by `;`, where `{x}` is column index (counted from 1)
+ robione.txt -- proceeded files (done and positive validated)
+ pominiete.txt -- not proceeded files (missing or error occurred)
+ bledne.txt -- invalid files (proceeded but negative validated)

If you need to alter column order, column separator or anything related to input data, just edit selected scriptfile. Also remember to alter `--jobs` and `--delay` to achieve best performance for your machine.

## Examples
Convert 'input.pdf' in PDF/A-1B format; the output is 'input-PDFA.pdf':
```
  ./pdf2archive input.pdf
```

Convert 'input.pdf' in PDF/A-1B format; the output is 'output.pdf':
```
  ./pdf2archive input.pdf output.pdf
```

Convert 'input.pdf' in PDF/A-1B format and perform a high-quality compression:
```
  ./pdf2archive --quality=high input.pdf
```

Convert 'input.pdf' in PDF/A-1B format and specify the document title:
```
  ./pdf2archive --title="Title of your nice document" input.pdf
```

Convert 'input.pdf' in PDF/A-1B format and validate the result:
```
  ./pdf2archive --validate input.pdf
```

## Licensing
+ __PDF2ARCHIVE__ is under GPLv3+.
+ The file __`AdobeRGB1998.icc`__ (included in binary form for portability reasons) is licensed in compliance with Adobe's license agreement: https://www.adobe.com/support/downloads/iccprofiles/icc_eula_win_dist.html.
+ __VeraPDF__ is dual-licensed under MPLv2+ and GPLv3+: http://verapdf.org/home/#licensing. The launcher scripts have been slightly modified to include a conditional command-line option that deals with the changes introduced by Java 9.
