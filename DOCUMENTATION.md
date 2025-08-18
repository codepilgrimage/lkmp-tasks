# 1. Research & Generate Kernel Documentation
The Linux kernel uses Sphinx to build HTML, PDF, or man-page documentation from the files in the Documentation/ directory.

## 1.1 Check how documentation is generated
Open the top-level Makefile and search for "docs":

```bash
grep -n "docs" Makefile
```
You’ll see something like:

```makefile
DOC_TARGETS := xmldocs latexdocs pdfdocs htmldocs epubdocs
PHONY += xmldocs latexdocs pdfdocs htmldocs epubdocs
```
These targets control documentation generation.

Also check:
```bash
ls Documentation/
```
You’ll see `.rst` files (ReStructuredText), subdirectories, and a conf.py (Sphinx config).

## 1.2 Install dependencies
For building documentation, you’ll need:
```bash
sudo apt update
sudo apt install make python3-sphinx python3-sphinx-rtd-theme \
                 texlive-latex-base texlive-latex-recommended \
                 texlive-latex-extra texlive-fonts-recommended \
                 texlive-fonts-extra texlive-xetex latexmk
```
(If you only want HTML, you can skip TeX Live.)

## 1.3 Build the docs
In the kernel source root:

```bash
make htmldocs
```
This will:
Parse Documentation/ .rst files using Sphinx

### Output HTML docs in:
```bash
Documentation/output/html/
```

### Open them in a browser:
```bash
xdg-open Documentation/output/html/index.html
```

### To build PDFs:
```bash
make pdfdocs
```
PDF will be in Documentation/output/latex/.

# 2. Fix a Warning / Spelling / Grammar Issue
## 2.1 Find warnings in documentation
Run:
```bash
make htmldocs SPHINX_WARNINGS_LOG=warnings.log
```
This will store warnings (broken links, bad formatting) in warnings.log.

Or just build normally and look for lines like:

```vbnet
WARNING: Title underline too short.
```

# 3. Run selftests in Linux
Run:
```bash
sudo make -C tools/testing/selftests run_tests
sudo make -C tools/testing/selftests run_tests 2>&1 | tee <output_file>
```
