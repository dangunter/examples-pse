# examples-dev

This repository contains all example notebooks and scripts for
the IDAES-PSE code.

## Developer instructions

### Building the examples

To rebuild the examples, go to the top-level directory.

The first time, install dependencies (into whatever Python environment
you have set up) by running:

    pip install -r requirements.txt
    
Then build the documentation with:

    python build.py
    
### Adding examples

In the documentation below, references to values in the configuration
file indicate nesting with a ".", e.g. "foo.bar" refers to the value
of the "bar" item nested under "foo", which in YAML would look
like this:

    foo:
       bar: value


#### Adding a Jupyter Notebook

1. Put your example notebook in a subdirectory of `src/`

The name of your notebook file will be used for the generated
restructured text wrapper. For the page title, single underscores
will be converted to spaces. Use double underscores in the name
to create a ": " in the title of the page.

2. Add the notebook's directory to `build.yml`

This configuration tells `build.py` where to find notebooks and
how to build the Sphinx documentation. The top-level key is `notebooks`,
under which go general settings, then each directory tree containing
Jupyter Notebooks has an entry in the list under `notebooks.directories`. 
The path to the directory tree is constructed by combining the
 `notebook.source_base` prefix with the `notebook.directories.source` 
 value. Note that the 
`notebook.directories.match` value, if present, will restrict notebooks
to ones with that value included in their filename.

For example:

    # keep going even if some notebooks fail to execute
    continue: true
    # base of all notebooks
    source_base: src
    # base of all output
    output_base: docs
    # base for sphinx generated HTML pages
    html_dir: _build/html
    # template for creating Sphinx wrapper pages (see 3)
    template: docs/jupyter_notebook_sphinx.tpl
    # sub-directories for notebooks
    directories:
          # find notebooks in src/workshops/*
        - source: workshops
          # put generated docs in docs/tutorials/*
          output: tutorials
          # only process notebooks with "Solution" in filename
          match: Solution

3. Add the notebook's wrapper to the index page

This wrapper page (a simple .rst file) will be generated automatically
by the build script, so it doesn't exist before you run `build.py`.
The rules for making the new filename is very simple: the extension 
`.ipynb` will be replaced by `_doc.rst`. For example, if your notebook was in
`src/foo/Number_1_Foo_Solution.ipynb` then the output file will be
`Number_1_Foo_Solution_doc.rst`. The output directory will be the
`notebok.output_base` directory given in the configuration file (`build.yml`,
see Step 2 for details). Images and generated HTML will be copied to
the Sphinx destination HTML directory, given by `notebook.html_dir` . 


The index page is usually under
`<notebook.output_base>/<notebook.directories.*.output>/index.rst`
 . It needs to have an entry in the table of contents that names the 
generated wrapper file, e.g., "Number_1_Foo_Solution_doc".