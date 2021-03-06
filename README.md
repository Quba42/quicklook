#QuickLook versions#

* In `master` branch, there is an older version of QuickLook which does not have all features, but it is (somehow) tested. It produces GIFs and needs `imagemagick`. This branch uses `flex_81.py` reading routines by  J. Brioude (NOAA).
* In `newIO` branch, there is a more developed version of QuickLook with some additional features, like integration over species, integration over height columns etc. This version produces MP4 and needs `ffmpeg` to run. This branch uses reading routines by J. Bukhardt.
* For both versions it is recommended to have `Matplotlib` version >= 1.3

## QuickLook and FLEXPART versions##
QuickLook currently supports **FLEXPART up to version 9.0**. Newer versions > 9.0 are not currently supported due to changes in structure in `headers` and `grids`. The main problem is that the length of version string in `header` in versions >9.0 is not fixed as in FLEXPART 9.0 and binary reader can thus read some rubbish instead of data. Hopefully, this will be somehow standardised soon.

# News #
* Support of MP4 (branch `newIO`)
* Some minor improvements according to tickets (branch `newIO`)
* Integration over vertical columns now added (branch `newIO`)
* Integration over subset of species (branch `newIO`)
* Integration over time (branches `master` and `newIO`)
* **Age classes** support added - remains to be tested for dry and wet deposition
* Wet and dry **depositions** (or their sum) now can be shown!
* Quicklook now supports **multiple species**!
* New possibilities of plotting gridded data added (**pcolormesh** and **imshow**), look into `config.py`!
* Options for **NASA Bluemarble**, **shaded relief** and **etopo** map backgrounds added, look into `config.py`!
* Test data for forward and backward runs added, see **Test data and examples** Section!


# About QuickLook #

## Introduction ##

QuickLook is a free command-line tool for quick plotting of [FLEXPART](http://www.flexpart.eu) outputs. Like the following ones:)

![mother_bluemarble_small_optim.gif](https://bitbucket.org/repo/dAb4ay/images/2034626945-mother_bluemarble_small_optim.gif)

![mother_bw_small_optim.gif](https://bitbucket.org/repo/dAb4ay/images/2168236771-mother_bw_small_optim.gif)

![conc+depo_small_optim.gif](https://bitbucket.org/repo/dAb4ay/images/1711578702-conc%2Bdepo_small_optim.gif)

It is written in pure Python 2.7, so no compilation nor installation is required. You just need to fulfill the prerequisites and install all libraries needed, see Section **Prerequisites**. After obtaining OuickLook from this repository, you can easily modify it according to your needs. The easiest way how to get QuickLook is to clone the git repository:

```bash
$ git clone https://radekhofman@bitbucket.org/radekhofman/quicklook.git
```

or you can download the repository as a zip archive from [download](https://bitbucket.org/radekhofman/quicklook/downloads) page.

The software is still under development. The future development should be user-driven so do not hesitate to raise your questions, suggestions or report issues and bugs using [issues reporting system](https://bitbucket.org/radekhofman/quicklook/issues?status=new&status=open). Registration to Bitbucket is not needed, just click **+ Create issue** in the top right corner of the page.

## Licence ##

QuickLook is distributed under [GPL 2.0](http://www.gnu.org/licenses/gpl-2.0.html).

## Prerequisites ##

To run QuickLook you need [Python 2.7](https://www.python.org/downloads/) and following libraries:

* [Numpy](http://www.numpy.org/)
* [Matplotlib](http://matplotlib.org/) >1.3
* [Basemap](http://matplotlib.org/basemap/)

For producing GIF animations you also need [ImageMagick](http://www.imagemagick.org/).

Reading of FLEXPART outputs is accomplished via *flex_81.py* module by J. Brioude (NOAA).

# How to use QuickLook? #

It is quite easy:) QuickLook is a command-line tool. To display a list of possible options, simply run

```bash
$ python quick_look.py -h
```

You shoule get something like this:

```bash
Usage: quick_look.py [options]

Created by Radek Hofman 2014

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -e EXP_PATH, --explore=EXP_PATH
                        Prints info abou Flexpart output at EXP_PATH
  -i INPUT, --input=INPUT
                        Flexpart output directory
  -d DOMAIN, --domain=DOMAIN
                        Coordinates of the domain: ll_lon, ll_lat, ur_lon,
                        ur_lat
  -m, --max_lat_lon     Takes maximum available lat-lon domain (overrides -d)
  -l LEVELS, --levels=LEVELS
                        Sets vertical level range: l0, l1 (l0,l1 >= 0, l0=l1
                        to select just one level, e.g. -l 0 0 sets the ground layer)
  -t TYPE, --type=TYPE  Flexpart output type [mother|nested]
  -o OUTPUT, --output=OUTPUT
                        Images output directory
  -r RECEPTORS, --receptors=RECEPTORS
                        File with receptors
  -f DATAFACTOR, --datafactor=DATAFACTOR
                        Factor to multiply the data with
  -u UNITLABEL, --unitlabel=UNITLABEL
                        String containing units of visualized quantity
  -n FILENAME, --filename=FILENAME
                        Filename of resulting GIF
  -q, --reverse         Files are processes in reverse order
  -x TITLE, --title=TITLE
                        Title of images
  -z PROJECTION, --projection=PROJECTION
                        Map projection [cylindrical:cyl (default),
                        Marcator:merc]
  -s SPECIES, --species=SPECIES
                        species to show (default is the first one - 1)
  --conc                Show concentrations/residence times (default)
  --drydepo             Show dry depo (can be combined with --wetdepo)
  --wetdepo             Show wet depo (can be combined with --drydepo)
```

### Explore FLEXPART outputs ###

To get started, let's explore some FLEXPART outputs. To do so, execute command
```bash
$ python quick_look.py -e <path_to_output>
```
where `<path_to_output>` is path to FLEXPART Output directory, e.g.

```bash
$ python quick_look.py -e /home/radek/run_1/Output
```

You should get something like:

```bash
==================================================================
DOMAIN TYPE: mother
Header info FLEXPART V9.0:

Number of species: 1
1 TRACER    

Domain:
    Lon: -179.000 - 179.000 deg, step 1.000 deg
    Lat:  -90.000 - 89.000 deg, step 1.000 deg

Output levels:
     0 100.0 m
     1 500.0 m
     2 1000.0 m
     3 1500.0 m
     4 2000.0 m

Run type:
    Backward run

Output step:
    -10800 sec (-3.00 hours)
Release dates:
    From 2013/04/07 18:54 to 2013/04/08 06:54
==================================================================
==================================================================
DOMAIN TYPE: nested
Header info FLEXPART V9.0:

Number of species: 1
1 TRACER    

Domain:
    Lon: 110.000 - 170.000 deg, step 0.250 deg
    Lat:  15.000 - 60.000 deg, step 0.200 deg

Output levels:
     0 100.0 m
     1 500.0 m
     2 1000.0 m
     3 1500.0 m
     4 2000.0 m

Run type:
    Backward run

Output step:
    -10800 sec (-3.00 hours)
Release dates:
    From 2013/04/07 18:54 to 2013/04/08 06:54
==================================================================

```

We see that we have a backward run of FLEXPART with a global mother and a smaller nested domain. The output time step is 3 hours and we have 5 vertical levels 0-4.

### Plotting results ###

Now, when we know what we have, we can plot the results. We run:

```bash
$ python quick_look.py -i /home/radek/run_1/Output -t mother -m -l 0 4 -s 1
```

This command says:

* Plot data from /home/radek/run_1/Output (`-i /home/radek/run_1/Output`)
* Use mother domain (`-t mother`)
* Take maximum available lon-lat range (`-m`)
* Show integrated values over all levels 0-4 (`-l 0 4`)
* Take output for species no. 1

The output should be as follows:
```bash
 [INFO] - Species: 1 TRACER   
 [INFO] - Taking maximum available domain: (-179.0, -90.0, 179.0, 89.0)
 [INFO] - No output path provided, results will be saved to /home/radek/run_1/Output
 [INFO] - No filename provided, setting to anim_mother.gif
 [INFO] - cylindrical projection will be used
 [INFO] -- Minimum date value = 0.0000E+00
 [INFO] -- Maximum value of data = 1.3717E+03
 [INFO] -- Processing file 0 grid_time_20130327000000_001
 [INFO] - Creating animation /home/radek/run_1/Output/mother_0-4_anim_mother.gif
 [INFO] -- Processing file 0 grid_time_20130327000000_001
 [INFO] -- Processing file 1 grid_time_20130327030000_001
 [INFO] -- Processing file 2 grid_time_20130327060000_001
    .
    .
    .
...Done!
```

What we get is a bunch of PNGs with single time frames and a GIF animation. That was easy, wasn't it?:) Since we did not set any output directory, the output files (frames+animation) are stored in input path. Much better is to define a separate location for this using `-o` flag. We can also change the name of our animation using `-n` flag. The type of domain and selected vertical levels are appended automatically to the file name.

We can further improve our plot. For example, we can plot points of interest (PIO) into the plot by the means of providing a *file* using flag `-r <file_with_POIs>`. The format of the file should be as follows. Each PIO is on a separate line, where its longitude, latitude and label appear:
```
139.08  36.30 JPX38
```
 
To plot a sub-domain instead of the whole domain, use `-d ll_lon, ll_lat, ur_lon, ur_lat`, where parameters are longitude of lower-left corner, latitude of lower-left corner, longitude of upper-right corner and latitude of upper-right corner. This flag is overridden by `-m` flag taking maximum available domain. To select a range of vertical levels which are shown use `-l <level1> <level2>`. If you want to see just one particular level, then set *level1*=*level2*.

Title of plots can be set using `-x <Title>`. This will appear on all frames. Date-time stamp in the title is added automatically. You can also set units which will appear under the colorbar using `-u <Units>`.

If you need to multiply the data by a *Factor* before plotting, use `-f <Factor>`.

Particularly for backward runs is useful flag `-q`, which forces the output files to be processed in a backward manner.

To select which species to plot, use `-s <species number>`.

To change the map projection from default cylindrical to Mercator, use `-z merc`. **Warning:** Using of Mercator projection for a domain containing poles will raise an error.

To show dry/wet deposition instead of concentration/residence times, use `--drydepo` or `--wetdepo`. To show their sum use both, i.e. `--drydepo --wetdepo`.

## Configuring and modifying QuickLook ##

Generally, virtually everything can be changed by modifying the source code of QuickLook. However, the goal is to provide a full control over the appearance of plots via a configuration file *config.py*, where you can configure some properties. I hope that the structure of the file is quite self-explanatory.

## Test data and examples ##

Test data for backward run of FLEXPART are available in `test_data` in the root of the repository. There are test data for both forward and backward runs. Forward run includes multiple species. For example, you can run the backward example by executing the following command from `src` directory:

```bash
$ python quick_look.py -i ../test_data/bwd_run -t mother -m -l 0 4 -r ../test_data/receprors.txt -x "Backward run from JPX38" -u "(mBq/GBq)" -o ../samples/
```

An animation in `samples` should be produced.

# TO DOs #

* Prepare unit tests to maintain consistency in future development:)
* Enable users to easily access more parameters via *config.py* file
* Include *flex-diff* tool for plotting differences between two FLEXPART runs
* Improve ERROR and WARNING messages

If you have any special requirement, please let me know via [issues reporting system](https://bitbucket.org/radekhofman/quicklook/issues?status=new&status=open):)